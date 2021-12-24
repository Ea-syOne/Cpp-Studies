## Types
##### size_t: unsigned int 값, 즉 부호 없는 n-bit 정수이다. 따라서 0 ~ 2^n - 1 사이 값을 가진다. 메모리나 문자열 등의 길이를 구할 땐 size_t 값으로 반환하게 된다.
> unsigned int와 size_t의 차이는 n-bits OS일 때 몇 bit 정수 값인지에서 발생한다. unsigned int의 경우 64-bits OS라 해도 32-bits 정수 일 수도 있다. 반면 size_t의 경우 n-bits OS에선 확실하게 n bits 정수 값을 갖는다는 특징이 있다.
##### FILE: file stream 정보를 담은 object type. 
##### fpos_t: file 내의 position을 가리키는 값의 type. 
-----------------------------

## Macros
##### NULL: NULL pointer를 나타내는 상수값.  
##### _IOFBF, _IOLBF, IONBF: setvbuf 함수에서 버퍼 모드를 지정하는 데에 쓰이는 상수 값.
##### BUFSIZ: setbuf 함수를 사용해 버퍼를 할당할 때, 해당 버퍼의 최소한의 바이트 크기를 지정하는 상수 값.
##### EOF: File의 끝에 도달했음을 의미하는 음의 정수 값.
##### FOPEN_MAX: 시스템에서 동시에 열 수 있도록 보장된 최대 파일의 수를 나타내는 정수 값.
##### FILENAME_MAX: file_name으로 적합한 최대의 문자열 길이를 나타내는 정수 값.
##### L_tmpnam:
##### SEEK_CUR, SEEK_END, SEEK_SET: fseek 함수를 통해 파일 내 특정 fpos를 찾을 때 인자로 주는 값들로, 각각 현재 fpos 위치, file 끝부분의 위치, 파일 시작지점의 위치 값을 갖는다. 
##### TMP_MAX: tmp
##### srderr, srdin, stdout: 기본 error, 기본 input, 기본 output stream의 FILE* 값들.
-----------------------------------------

## Functions
### FILE Functions
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
#### `void clearerr(FILE* stream)`
stream 내의 EOF 표시자와 error 표시자를 모두 초기화한다. 즉, 사전에 error가 발생했더라도 clearerr(stream)을 호출하면 error가 잡히지 않는다.
#### `int fflush(FILE* stream)`
stream과 연관된 버퍼를 비우게 하는 함수. 만약 stream 값이 NULL이면 열린 모든 stream을 비운다. 
##### 버퍼를 삭제했다면 0을, 그렇지 않으면 0이 아닌 errno를 반환.

---------------------------------------
### FILE getter Functions
#### `int feof(FILE* stream)`
FILE object 내의 EOF flag가 설정되어 있는지를 반환하는 함수. 
##### 설정되어 있다면 0이 아닌 값을, 그렇지 않으면 0을 반환.
#### `int ferror(FILE* stream)`
FILE object 내의 error flag가 설정되어 있는지를 반환하는 함수. 
##### 설정되어 있다면 0이 아닌 값을, 그렇지 않으면 0을 반환.
#### `int fgetpos(FILE* stream, fpos_t* pos)`
FILE stream의 position을 찾아와 인자로 받은 pos 위치에 저장하는 함수. 
##### pos를 가져오는 데 성공 시 0을, 실패 시 0이 아닌 errno를 반환.
##### 설정되어 있다면 0이 아닌 값을, 그렇지 않으면 0을 반환.
