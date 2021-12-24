## Types
##### size_t: unsigned int 값, 즉 부호 없는 n-bit 정수이다. 따라서 0 ~ 2^n - 1 사이 값을 가진다. 메모리나 문자열 등의 길이를 구할 땐 size_t 값으로 반환하게 된다.
> unsigned int와 size_t의 차이는 n-bits OS일 때 몇 bit 정수 값인지에서 발생한다. unsigned int의 경우 64-bits OS라 해도 32-bits 정수 일 수도 있다. 반면 size_t의 경우 n-bits OS에선 확실하게 n bits 정수 값을 갖는다는 특징이 있다.
##### FILE: file stream 정보를 담은 object type. 
##### fpos_t: file 내의 position을 가리키는 값의 type. 
-----------------------------

## Macros
##### NULL: NULL pointer를 나타내는 상수값.  
##### _IOFBF, _IOLBF, IONBF: setvbuf 함수에서 버퍼 모드를 지정하는 데에 쓰이는 상수 값.
*  _IOFBF: full buffering 방식으로, stream에 쓰여질 데이터를 buffer에 저장했다가, buffer가 차면 쓰도록 하는 방식이다.
*  _IOLBF: 버퍼에 개행 문자(\n)가 입력될 때마다 버퍼에 있는 데이터를 stream에 쓰는 방식.
*  _IONBF: unbuffered 방식으로, 버퍼를 거치지 않고 바로바로 stream에 데이터를 쓰는 방식.
##### BUFSIZ: setbuf 함수를 사용해 버퍼를 할당할 때, 해당 버퍼의 최소한의 바이트 크기를 지정하는 상수 값.
##### EOF: File의 끝에 도달했음을 의미하는 음의 정수 값.
##### FOPEN_MAX: 시스템에서 동시에 열 수 있도록 보장된 최대 파일의 수를 나타내는 정수 값.
##### FILENAME_MAX: file_name으로 적합한 최대의 문자열 길이를 나타내는 정수 값.
##### L_tmpnam: tmpnam 함수 결과로 생성되는 파일 이름의 길이. 
##### SEEK_CUR, SEEK_END, SEEK_SET: fseek 함수를 통해 파일 내 특정 fpos를 찾을 때 인자로 주는 값들.
*  SEEK_CUR: 현재 fpos 위치
*  SEEK_SET: file 시작 지점의 위치
*  SEEK_END: 파일 끝 지점의 위치
##### TMP_MAX: tmpnam으로 생성할 수 있는 임시 파일 이름의 최대 수.
##### srderr, srdin, stdout: 기본 error, 기본 input, 기본 output stream의 FILE* 값들.
-----------------------------------------

## Functions
### FILE Management Functions
#### FILE* fopen(const char* filename, const char* mode)
_filename_ 과 같은 이름의 file을 여는 함수. mode 값은 file 접근 방식을 지정하는 데 사용한다.<br>
##### mode
*  "r": 파일을 읽기 전용으로 open한다. _filename_ 이 존재하지 않으면 실패.
*  "w": 쓰기 전용의 빈 파일을 생성 후 open한다. 만약 동일한 _filename_ 의 file이 이미 존재할 경우 내용을 다 지워 빈 파일로 만든다.
*  "a": 해당 file의 끝 부분부터 이어서 쓰기 전용 파일을 open한다. 만약 해당 파일이 존재하지 않으면 새 파일을 생성한다.
*  "r+": 읽고 쓸 수 있는 파일을 open한다. _filename_ 이 존재하지 않으면 실패.
*  "w+": 읽고 쓸 수 있는 빈 파일을 생성 후 open한다. 동일한 _filename_ 이 존재한다면 내용을 다 지워 빈 파일로 만든다. 
*  "a+": 읽고 이어 쓸 수 있는 파일을 open한다. 파일이 존재하지 않으면 새로 생성한다. 
   >  읽기 작업 때엔 fseek이나 rewind를 통해 pos를 원하는 대로 조정 가능하지만, 쓰기 작업을 할 때는 항상 파일 맨 끝으로 pos를 이동시킨다.
*  "b": 파일을 binary 형식으로 열고 싶다면 mode 문자열 뒤에 "b"를 붙이면 된다("rb", "w+b" 등). 
>  읽고 쓸 수 있는 모드의 경우 쓰기 후 읽기를 할 땐 반드시 fflush로 스트림을 비우거나, 위치를 조정해줘야 한다.
##### 만약 파일이 성공적으로 열렸다면 해당 파일의 FILE pointer 값을, 실패했다면 NULL을 반환.
#### `int fcolose(FILE* stream)`
stream에 해당하는 file과 연관된 버퍼를 모두 삭제하고 file을 닫는 함수.
##### 성공적으로 닫으면 0을, 실패하면 errno를 반환.
#### `FILE* freopen(const char* filename, const char* mode, FILE* stream)`
stream에 해당하는 file을 닫고, close 성공 여부와 관계 없이 filename에 해당하는 file을 주어진 mode에 맞게 여는 작업. mode는 fopen과 동일.<br> freopen의 의의는 이미 열린 파일 모드를 재설정할 수 있다는 점에 있음(stderr, stdin, stdout조차 재설정 가능).
##### 만약 파일이 성공적으로 열렸다면 해당 파일의 FILE pointer 값을, 실패했다면 NULL을 반환.
##### 성공적으로 닫으면 0을, 실패하면 errno를 반환.
#### `int rename(const char* old_filename, const char* new_filename)`
old_filename에 해당하는 파일의 이름을 new_filename으로 수정하는 함수. 
> 열려있는 파일을 rename하고자 할 경우 에러가 발생한다.
##### 성공적으로 수정하면 0을, 실패하면 errno를 반환.
#### `int remove(const char* filename)`
filename에 해당하는 파일을 삭제하는 함수. 
> fclose 되지 않은 파일을 remove하고자 할 경우 에러가 발생한다.
##### 성공적으로 삭제하면 0을, 실패하면 errno를 반환.
#### `int fflush(FILE* stream)`
stream과 연관된 버퍼를 비우게 하는 함수. 만약 stream 값이 NULL이면 열린 모든 stream을 비운다. 
##### 버퍼를 삭제했다면 0을, 그렇지 않으면 0이 아닌 errno를 반환.
#### `FILE* tmpfile(void)`
읽고 쓸 수 있는 임시 binary 파일(= "w+b")을 생성하는 함수이다. 
##### 생성에 성공했다면 해당 FILE pointer을, 그렇지 않으면 NULL을 반환.
#### `char* tmpnam(char* str)`
기존 파일과 겹치지 않는 임시 파일명을 생성하여 str에 저장하는 함수. 단 str은 최소 L_tmpnam의 길이를 갖는 배열이여야 한다.
> str이 NULL일 경우 내부 static buffer에 결과를 저장. 이는 다음 tmpnam 호출 시 사라지게 됨.
##### 생성에 성공했다면 해당 임시 이름의 pointer 값을, 그렇지 않으면 NULL을 반환.

---------------------------------------
### FILE Flag Functions
#### `int feof(FILE* stream)`
FILE object 내의 EOF flag가 설정되어 있는지를 반환하는 함수. 
##### 설정되어 있다면 0이 아닌 값을, 그렇지 않으면 0을 반환.
#### `int ferror(FILE* stream)`
FILE object 내의 error flag가 설정되어 있는지를 반환하는 함수. 
##### 설정되어 있다면 0이 아닌 값을, 그렇지 않으면 0을 반환.
#### `void clearerr(FILE* stream)`
stream 내의 EOF 표시자와 error 표시자를 모두 초기화한다. 즉, 사전에 error가 발생했더라도 clearerr(stream)을 호출하면 error가 잡히지 않는다.

--------------------------------------
### FILE pos Functions
#### `int fgetpos(FILE* stream, fpos_t* pos)`
FILE stream의 position을 찾아와 인자로 받은 pos 위치에 저장하는 함수. 
##### pos를 가져오는 데 성공 시 0을, 실패 시 0이 아닌 errno를 반환.
#### `int fsetpos(FILE* stream, fpos_t* pos)`
FILE stream의 pos 값을 fpos_t pointer pos에 담긴 값으로 설정하는 함수.
> 일반적으로 pos 값은 이전에 fgetpos로 받아온 값을 이용한다.
##### pos를 설정하는 데 성공 시 0을, 실패 시 0이 아닌 errno를 반환.
#### `int fseek(FILE* stream, long int offset, int whence)`
FILE stream의 pos 값을 조정하는 함수. whence 위치부터 offset만큼 이동시키는 작업을 수행함.<br>
whence 값은 stdio.h Macro인 SEEK_SET, SEEK_CUR, SEEK_END 값을 사용한다.
##### pos를 조정하는 데 성공 시 0을, 실패 시 0이 아닌 errno를 반환.
#### `long int ftell(FILE* stream)`
FILE stream의 pos 값을 가져오는 함수. 
> fgetpos와 fsetpos를 같이 사용하듯이, ftell과 fseek도 같이 사용된다(둘 다 long int 값). 
##### pos를 가져오는 데 성공 시 해당 값을, 실패 시 0이 아닌 errno를 반환.
#### `void rewind(FILE* stream)`
FILE stream의 pos 값을 file 시작 지점으로 옮기는 함수.  

----------------------------------------
### FILE get/put function

----------------------------------------
### FILE print/scan function
print, scan의 가장 큰 특징은 formatted I/O를 관리하는 함수라는 점이다. 
#### formatted output
%[flags][width][precision][length]specifier 형태로 관리하는 output으로, 각 항목은 다음과 같다.
*  flag: 출력 형태를 조정하는 flag. **+** - 양수 앞에 + 기호를 붙임, " " - 부호가 없으면 한칸 띄어 쓰기, **0** - 수 지정 시 빈 칸에 0 삽입 등.
*  width: 출력 값의 폭을 지정. 이때 width가 음수면 왼쪽 정렬, 양수면 오른쪽 정렬임.
*  precision: 데이터의 정밀도 지정. 보통 소수점 n번째까지 표기할때 .n 형식으로 붙여준다. 
*  length: 데이터의 정확한 크기를 지정. l을 붙이면 long int 또는 wide char이고, L을 붙이면 long double을 지정한다.
*  specifier: 어떤 type의 데이터인지 나타내는 값. **c** - char, **d** - int, **s** - string, **f** - float, **p** - pointer address 등. 특이 사항으로 **n** 의 경우 출력하는 값은 없고, 인자로 받은 int pointer 위치에 현재까지 쓰여진 문자 bytes 수를 저장한다(보안 취약점). 
#### `int printf(const char* format, ...)`
formatted output을 받아 stdout에 출력하는 함수. 
##### 출력 성공 시 출력된 문자의 byte 수를, 실패 시 음수를 반환.
#### `int fprintf(FILE* stream, const char* format, ...)`
formatted output을 받아 stream에 출력하는 함수. 
##### 출력 성공 시 출력된 문자의 byte 수를, 실패 시 음수를 반환.
#### `int sprintf(char* str, const char* format, ...)`
formatted output을 받아 str에 저장하는 함수. str 끝에 자동으로 **\0** 을 붙여주기 때문에 1칸의 여유 공간을 둬야 한다.
##### 저장 성공 시 저장된 문자열의 byte 수(\0 제외)를, 실패 시 음수를 반환. 

#### formatted input
%[*][width][modifiers]type 형태로 관리하는 input으로, 각 항목은 다음과 같다.
*  *: 들어온 데이터를 무시하라고 지시. %*d%d, i, j라면 i는 2번째 입력 값을 저장하고, j는 값을 저장하지 못한다. 
*  width: 읽어들일 최대 문자 수. 단 \0이 들어갈 충분한 공간을 고려해야 한다.
*  modifier: 입력받는 데이터의 크기를 지정하는 지정자. formatted output의 length와 동일.
*  type: 입력받을 데이터의 type. 역시 formatted output의 specifier와 동일.

#### `int scanf(const char* format, ...)`
formatted String을 받아 stdin에 입력하는 함수. 
##### 입력 성공 시 출력된 문자의 byte 수를, 실패 시 음수를 반환.
#### `int fprintf(FILE* stream, const char* format, ...)`
formatted String을 받아 stream에 출력하는 함수. 
##### 출력 성공 시 출력된 문자의 byte 수를, 실패 시 음수를 반환.
#### `int sprintf(char* str, const char* format, ...)`
formatted String을 받아 str에 저장하는 함수. str 끝에 자동으로 **\0** 을 붙여주기 때문에 1칸의 여유 공간을 둬야 한다.
##### 저장 성공 시 저장된 문자열의 byte 수(\0 제외)를, 실패 시 음수를 반환.

---------------------------------------
### FILE read/write Functions
#### `size_t fread(void* ptr, size_t size, size_t nmemb, FILE* stream)`
주어진 stream으로부터 size 크기의 데이터 nmemb개를 읽어와 ptr이 가리키는 위치에 저장하는 함수.<br>
데이터는 Stream의 pos부터 읽어들이며, pos는 읽어들인 bytes만큼 증가한다. 
##### 데이터를 읽어오는 데 성공 시 읽어들인 byte 수(= size * nmemb)를, 실패 시 다른 값을 반환.
#### `size_t fwrite(const void* ptr, size_t size, size_t nmemb, FILE* stream)`
주어진 ptr로부터 size 크기의 데이터 nmemb개를 읽어와 stream의 pos부터 쓰는 함수.<br> pos는 읽어들인 bytes만큼 증가한다.
##### 데이터를 쓰는 데 성공 시 쓰인 byte 수(= size * nmemb)를, 실패 시 다른 값을 반환.

---------------------------------------
### Buffer Management Functions
#### `void setbuf(FILE* stream, char* buffer)`
FILE stream의 buffer를 지정하는 함수. 이때 buffer는 BUFSIZ 이상의 크기를 가져야 한다.<br>
설정한 buffer는 fully buffered로 지정되며, 따라서 블록이 다 채워지지 않아도 스트림을 비우려면 fflush를 사용해야 한다.<br>
> buffer 자리에 NULL pointer를 넣을 경우 stream은 unbuffered로 설정된다. 즉, 쓰기 작업을 하자마자 바로 파일에 쓰는 방식이기에 fflush가 불필요해진다. 
#### `void setvbuf(FILE* stream, char* buffer, int mode, size_t size)`
FILE stream의 buffer를 더 세밀하게 지정하는 함수. size를 통해 버퍼 크기를 지정하며, mode를 통해 버퍼 방식을 지정 가능하다.<br>
mode는 stdio.h의 Macro인 _IOFBF, _IOLBF, _IONBF를 사용한다. 
> buffer 자리에 NULL pointer를 넣을 경우 시스템이 동적으로 size만큼의 메모리를 할당 후, buffer로써 사용하게 된다.

