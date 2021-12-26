## Types
##### wchar_t: wide character type. 유니코드의 경우 표현해야 할 문자의 양 때문에 character 당 2 bytes 이상으로 표현하고, 따라서 별개의 type인 wchar_t를 사용하게 된다. wchar_t 문자와 문자열은 따옴표 앞에 L을 붙여 표시한다(wchar_t L"Hello world!"). 
##### div_t: 몫(= quot)과 나머지(= rem) 값을 갖는 structure. div 함수에서 사용된다.
##### ldiv_t: div_t의 각 값을 long int 형태로 갖는 structure. ldiv 함수에서 사용된다.

--------------------------------------

## Macros
##### EXIT_FAILURE, EXIT_SUCCESS: exit 함수의 결과 값들. 
##### RAND_MAX: rand 함수가 반환할 수 있는 최대 값.
##### MB_CUR_MAX: multi-byte character의 최대 값.

--------------------------------------

## Functions
### x to y Functions
#### `double atof(const char* str)`
str을 해당하는 floating-point number로 형변환해주는 함수. 
##### 알맞은 형태의 문자열일 시 형변환한 floating-point 값 반환, 아니라면 0.0을 반환
#### `int atoi(const char* str)`
str을 해당하는 정수로 형변환해주는 함수. 
##### 알맞은 형태의 문자열일 시 형변환한 정수 값 반환, 아니라면 0을 반환
#### `long int atol(const char* str)`
str을 해당하는 long integer 값으로 형변환해주는 함수. 
##### 알맞은 형태의 문자열일 시 형변환한 long integer 값 반환, 아니라면 0을 반환
#### `double strtod(const char* str, char** endptr)`
str의 앞 부분을 해당하는 floating-point 값으로 형변환한 후, endptr이 가리키는 char* 배열에 나머지 부분을 저장하는 함수. 
##### 문자열 앞부분에 알맞은 형태의 문자열이 있을 시 형변환한 floating-point 값 반환, 아니라면 0.0을 반환
#### `long int strtol(const char* str, char** endptr, int base)`
str의 앞 부분을 해당하는 long int 값으로 형변환한 후, endptr이 가리키는 char* 배열에 나머지 부분을 저장하는 함수. <br>
이 때 앞 부분의 값은 base에 해당하는 base 진수 값으로 받아들인다. 즉 base = 9, str = "17이지원"일 경우, 17을 9진법인 16으로 읽는다.
> 만약 base 값이 0일 경우, 앞이 0x(= 16진수)냐, 0(= 8진수)이냐, 아니냐(= 10진수)에 따라 맞춰서 판단한다.
##### 문자열 앞부분에 알맞은 형태의 문자열이 있을 시 형변환한 long integer 값 반환, 아니라면 0을 반환
#### `long int strtoul(const char* str, char** endptr, int base)`
strtol을 unsigned long int 버전으로 구현한 함수.
##### 문자열 앞부분에 알맞은 형태의 문자열이 있을 시 형변환한 unsigned long integer 값 반환, 아니라면 0을 반환
--------------------------------------

### Memory Allocation Function
#### `void* malloc(size_t size)`
size만큼의 공간을 heap에 동적 할당하는 함수. 일반적으로 형변환을 하고 사용(char* str = (char*)malloc(15);).
> malloc(0)의 경우 Memory Management error를 발생시킨다. NULL을 반환시키지 않고 가짜 address를 할당해 반환할 수 도 있기 때문.
##### 성공적으로 할당하면 할당된 메모리 공간을 가리키는 pointer 반환, 아니라면 NULL을 반환
#### `void* calloc(size_t nitems, size_t size)`
size만큼의 elements nitems개의 공간을 heap에 동적 할당하는 함수. 
> calloc은 malloc과 달리 할당한 공간을 0으로 초기화하기 때문에 더 안전하지만 느리다. 
##### 성공적으로 할당하면 할당된 메모리 공간을 가리키는 pointer 반환, 아니라면 NULL을 반환
#### `void* realloc(void* ptr, size_t size)`
malloc 또는 calloc으로 할당했던 ptr이 가리키는 공간을 size 크기로 재할당 하는 함수. 더 크게도, 더 작게도 할 수 있음.
> realloc(ptr, 0)의 경우 free(ptr)과 동일한 동작. free와 달리 중복 realloc(ptr, 0)을 해도 위험하지 않음. 하지만 realloc(ptr,0)으로 할당 해제한 ptr을 free(ptr)할 경우엔 심각한 에러 발생.
##### 성공적으로 할당하면 할당된 메모리 공간을 가리키는 pointer 반환, 아니라면 NULL을 반환
#### `void free(void* ptr)`
ptr에 할당했던 공간을 해제하는 함수.
> 상당히 주의하여 사용해야 하는데, malloc/calloc으로 할당한 공간은 반드시 free해야 하고(memory leak 문제), 이렇게 할당 해제한 공간을 중복하여 free할 경우(보통 멀티 쓰레드 상황에서 발생 가능) 치명적인 에러가 발생.
> 또한 해제된 메모리를 재참조할 경우 incorrect한 value에 접근하게 되니 주의해야 함(free 후 해당 ptr을 NULL로 설정하거나 등).

--------------------------------------------------

### System Related Function
#### `void abort(void)`
비정상적인 실행을 즉시 중단시키는 함수. 호출 즉시 프로그램이 중단된다.
#### `void exit(int status)`
호출한 프로세스를 즉시 종료하는 함수. 프로세스에 속한 FILE도 모두 닫으며, child 프로세스들을 프로세스 1번인 init에 상속시키고, 프로세스 상위 학목이 SIGCHLD 신호를 보내게 하는 작업까지 수행한다. status는 exit를 호출한 프로세스가 부모 프로세스에 보내는 상태 값으로, C에서는 EXIT_SUCESS(= 0)와 EXIT_FAILURE(= 1)로 구분한다.
> SIGCHLD 신호를 보내는 것은 자식 프로세스가 종료되었음을 부모 프로세스에 알리는 것이며, 이에 따라 부모 프로세스가 핸들러를 실행한다.
#### `int atexit(void(*func)(void))`
프로그램이 종료될 때 func를 실행하도록 설정하는 함수. 
비정상적인 실행을 즉시 중단시키는 함수. 호출 즉시 프로그램이 중단된다. 
##### 정상적으로 func를 연동하면 0을, 아니면 0이 아닌 값을 반환.
#### `char* getenv(const char* name)`
name에 해당하는 환경 값을 반환하는 함수. PATH, HOME, ROOT 등의 값을 불러온다.
##### 정상적으로 환경 값을 가져오면 해당 값을, 해당 환경 변수가 존재하지 않으면 NULL을 반환.
#### `int system(const char* command)`
command를 실행할 호스트 환경에 전달하고, 해당 command가 완료된 후 값을 반환하는 함수. 
##### 정상적으로 command가 실행되면 해당 command status 값을, 아니면 -1을 반환.
---------------------------------------------------------

### Useful Function
#### `void* bsearch(const void* key, const void* base, size_t nitems, size_t size, int(*compar)(const void*, const void*))`
base부터 시작해 size 크기의 nitems를 가진 배열에서 key와 일치하는 값을 compar 함수를 이용해 binary search한 결과를 가져오는 함수. 
> binary search이기 때문에 선형 탐색보다 더 빠르게 값을 찾을 수 있지만, 반드시 정렬된 배열을 사용해야 함.
> vector와 같은 C++ container에서 bsearch를 수행할 경우 base에는 그대로 vector[start_idx]의 주소를 넣어주면 된다. 
##### 정상적으로 key 값을 찾으면 해당 pointer 값을, 아니면 NULL을 반환.
#### `void qsort(void* base, size_t nitems, size_t size, int(*compar)(const void*, const void*))`
base부터 시작해 size 크기의 nitems를 가진 배열을 compar에 기반하여 정렬하는 함수. 
> 반드시 정렬이 전제로 깔려야 하는 binary search 특성상 bsearch 함수와 궁합이 좋다.
#### `int abs(int x)`
정수 값의 절댓값을 반환하는 함수.
##### x의 절댓값을 반환.
#### `long int labs(long int x)`
abs 함수의 long int 버전 함수.
##### x의 절댓값을 반환.
#### `div_t div(int numer, int denom)`
numer를 denom으로 나눈 결과를 반환하는 함수. 
> denom이 0이면 exception이 발생.
##### numer를 denom으로 나눈 몫과 나머지를 포함한 div_t 값을 반환.
#### `ldiv_t ldiv(long int numer, long int denom)`
div 함수를 long int 버전으로 구현한 함수.
##### numer를 denom으로 나눈 몫과 나머지를 포함한 ldiv_t 값을 반환.
#### `int rand(void)`
0부터 RAND_MAX 사이의 무작위 값을 반환하는 함수. rand 함수는 고도의 random 알고리즘은 아니기 때문에 완전한 무작위는 아니다. 
##### 0에서 RAND_MAX 사이의 무작위 값을 반환.
#### `void srand(unsigned int seed)`
rand 함수에서 무작위 수를 뽑는데 사용하는 seed 값을 재조정하는 함수. 
> 동일한 seed에선 동일한 rand 결과가 나오기 때문에 seed를 무작위와 유사한 값(보통 time 사용)으로 변경해줘야 더 무작위에 근접한 값이 나옴.
-------------------------------------------------------------------

### Multi-bytes to wide character Functions
#### Multi-bytes & Wide Characters
Multi-bytes Characters(이하 Mb)는 하나 이상의 byte sequence로 구성된 문자. 즉 한 문자가 1 또는 그 이상의 byte로 구성된다.
반면 Wide Characters(이하 Wc)는 각 문자가 1보다 큰 일정 크기의 byte로 표현된 문자. 즉 모든 문자가 k bytes(k > 1)로 구성된 문자이다.
##### 따라서 Mb의 경우 문자 C, 언, 어에 대해 각각 1, 2, 3 bytes일 수도 있는 반면, Wc의 경우 C, 언, 어 모두 동일한 3 bytes인 표현 방식이다.


#### `int mblen(const char* str, size_t n)`
multiple-bytes character의 길이를 최대 n만큼 확인하여 반환하는 함수. 
##### 정상적으론 해당 문자열의 길이를 반환. NULL wide character일 경우엔 0을, 잘못된 값을 받으면 -1을 반환.
#### `int wctomb(const char* str, wchar_t wchar)`
wchar를 multi-bytes 표현으로 변환하고, 이를 str이 가리키는 문자 배열에 저장하는 함수. 
##### str가 NULL이 아니면 str에 쓰인 bytes를 반환하며, 이때 wchar가 multi-byte로 표현 불가능하면 -1을 반환. 
##### str이 NULL이면 인코딩 상태에 따라 0 또는 0이 아닌 값 반환.
#### `size_t wcstombs(const char* str, const wchar_t* pwcs, size_t n)`
wchar_t 문자열인 pwc를 multi-bytes 문자열로 변환하고, 최대 n만큼 str이 가리키는 문자 배열에 저장하는 함수. 
##### str가 NULL이 아니면 str에 쓰인 bytes(\0 제외)를 반환하며, 이때 pwc가 multi-byte로 표현 불가능하면 -1을 반환. 
#### `int mbtowc(const wchar_t* pwc, const char* str, size_t n)`
multi-bytes 문자인 str을 wchar 형태로 변환하고, 이를 pwc에 n byte만큼 저장하는 함수. 
##### pwc가 NULL이 아니면 변환한 bytes 수를 반환하며, wchar_t로 표현 불가능하면 -1을 반환. 
#### `size_t mbstowcs(schar_t* pwcs, const char* str, size_t n)`
multi-bytes 문자열인 str을 wchar 형태로 변환하고, 최대 n만큼 pwcs가 가리키는 문자 배열에 저장하는 함수. 
##### pwcs가 NULL이 아니면 pwcs에 쓰인 bytes(\0 제외)를 반환하며, 이때 str이 wchar로 표현 불가능하면 -1을 반환. 
