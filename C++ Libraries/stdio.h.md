### Types
##### size_t: unsigned int 값, 즉 부호 없는 n-bit 정수이다. 따라서 0 ~ 2^n - 1 사이 값을 가진다. 메모리나 문자열 등의 길이를 구할 땐 size_t 값으로 반환하게 된다.
> unsigned int와 size_t의 차이는 n-bits OS일 때 몇 bit 정수 값인지에서 발생한다. unsigned int의 경우 64-bits OS라 해도 32-bits 정수 일 수도 있다. 반면 size_t의 경우 n-bits OS에선 확실하게 n bits 정수 값을 갖는다는 특징이 있다.
##### FILE: file stream 정보를 담은 object type. 
##### fpos_t: file 내의 position을 가리키는 값의 type. 
-----------------------------

### Macros
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

### Functions
##### FILE* fopen(const char* filename, const char* mode)
##### int fcolose(FILE* stream)
stream에 해당하는 file과 연관된 버퍼를 모두 삭제하고 file을 닫는 함수.
##### 성공적으로 닫으면 0을, 실패하면 errno를 반환.
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
##### 
