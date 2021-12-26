## types
### string
c++ 에서의 문자열을 대체하는 class.
#### Constructor
*  string s1: 빈 string 생성.
*  string s1(str): str 값으로 초기화된 string 생성.
*  string s1(str, pos, [len]): str의 pos부터 len만큼의 문자열로 초기화된 string 생성.
*  string s1(c_string, len): char 배열의 len만큼 문자열로 초기화된 string 생성.
*  string s1(size, c): 문자 c size개로 이루어진 문자열로 초기화된 string 생성.
*  string s1(iter_start, iter_end): iter_start ~ end까지의 문자열로 초기화된 string 생성.
#### Functions
##### length(): 문자열의 길이를 반환.
##### empty(): 문자열이 비었는지를 반환. 비면 0, 아니면 1.
##### clear(): 문자열을 비우는 함수.
##### operator[], at(i): s[i] 또는 at(i)로 i번째 index에 접근. at(i)는 []와 달리 범위를 벗어나는지 확인하고, 그만큼 더 느림.
##### back(), front(): 문자열의 가장 뒤, 앞 문자를 반환.
##### operator +=: 문자열 뒤에 새 문자열을 이어 붙이는 operator. 
##### append: append 역시 문자열 뒤에 새 문자열을 이어 붙이는 함수로, +=와 달리 추가 옵션들이 존재한다.
*  dest.append(src): dest 뒤에 src를 이어 붙임.
*  dest.append(src, len): dest 뒤에 src부터 len만큼 이어붙임. 
*  dest.append(src, pos, len): dest 뒤에 src의 pos부터 len만큼의 substring을 이어 붙임.
*  dest.append(size, c): dest 뒤에 char c를 size번 이어 붙임.
*  dest.append(iter1, iter2): dest 뒤에 iter1부터 iter2까지의 string을 이어 붙임. 
##### push_back(c): 문자열 끝에 문자 c를 이어 붙이는 함수.
##### pop_back(): 문자열 끝의 문자를 제거하는 함수.
##### assign: 문자열의 내용을 str로 설정하는 함수. 기존 내용은 사라짐.
> append와 동일한 5가지 option이 존재.
##### insert: 문자열에 내용을 삽입하는 함수로, 추가 옵션들이 존재한다.
*  dest.insert(pos, src): dest의 pos 위치에 src를 삽입.
*  dest.insert(pos, src, src_len): dest의 pos 위치에 src의 src_len만큼의 substring을 삽입하는 함수.
*  dest.insert(pos, src, src_pos, src_len): dest의 pos 위치에 src의 src_pos부터 src_len만큼의 substring을 삽입하는 함수.
*  dest.insert(pos, size, c): dest의 pos 위치에 c를 size번 삽입.
*  dest.insert(iter, size, c): dest의 iter 위치에 c를 size번 삽입.
> insert(iter, c)는 문자를 삽입한 iterator 값을 반환한다. 
*  dest.insert(d_iter, s_iter1, s_iter2): d_iter에 s_iter1~s_iter2의 문자열을 삽입.
##### erase: 문자열 내용을 지우는 함수로, 3가지 옵션이 존재한다.
*  dest.erase(pos, n): dest의 pos부터 n개의 문자를 삭제. n 값을 주지 않으면 pos 이후 모두 삭제.
*  dest.erase(iter): dest의 iter 위치의 문자 삭제.
*  dest.erase(iter1, iter2): dest의 iter1부터 iter2까지의 문자열 삭제.
##### replace: 문자열 내용을 교체하는 함수.
*  dest.replace(pos, len, src): dest의 pos부터 len만큼을 src로 교체.
*  dest.replace(pos, len, src, src_pos, src_len): dest의 pos부터 len만큼 src의 pos부터 len만큼의 문자열로 교체.
*  dest.replace(pos, len, size, c): dest의 pos부터 len만큼을 문자 c size번으로 교체.
*  위와 같은 옵션들을 iter로 대체하여 사용 가능.
##### .swap(target): dest와 target의 문자열을 교환하는 함수.
##### c_str: dest의 char 문자열 형태를 반환하는 함수.
##### data: dest의 문자열 형태의 pointer를 반환하는 함수.
##### copy(c_arr, len, [pos]): dest의 pos부터 len만큼을 c_arr에 복사하는 함수.
##### find: string 문자열 안에서 특정 keyword 존재 여부를 확인하는 함수. 가장 첫번째 위치만을 반환.
*  dest.find(src): dest에 src와 동일한 문자열이 존재하는지 확인하는 함수. 존재하면 그 위치를, 없다면 npos를 반환.
*  dest.find(src, pos, len): dest의 pos 이후부터 src의 len만큼의 문자열이 존재하는지를 확인 후 해당 position 또는 npos를 반환.
##### rfind: string 문자열 뒤에서부터 keyword 존재 여부를 확인하는 함수. 나머지는 find와 동일.
##### find_first/last_of(str, [pos=start/end]): 문자열의 pos부터 str에 속하는 첫/마지막 character를 찾는 함수.
##### find_first/last_not_of(str, [pos=start/end]): 문자열의 pos부터 str에 속하지 않는 첫/마지막 character를 찾는 함수.
> find 계열 함수들은 탐색하지 못했을 경우 npos 값을 반환.
##### substr(start, [len]): dest의 start부터 len만큼의 문자열을 가져오는 함수.
##### compare: 현재 string과 인자로 받는 string을 비교한 결과를 출력하는 함수.
*  dest.compare([pos], [len], str): dest의 pos부터 len만큼의 문자열을 str과 비교.
*  dest.compare(pos, len, str, pos_str, str_len): dest의 pos부터 len만큼과 str의 pos_str부터 len_str만큼의 문자열을 비교.
##### operators
*  head + tail: tail을 head 뒤에 append함.
*  s1 (relational_op) s2: 두 string의 관계를 출력. >, <, != 등. 사전 순에 따라 계산.

------------------------------------------

## Macros
##### npos: size_t 값의 최대 값. 

------------------------------------------

## Functions
#### `int stoi(const string& str, size_t* idx, int base)`
base진법으로 표현된 str을 정수로 변환한 값을 반환하는 함수. 변환 결과 int 크기를 idx가 가리키는 위치에 저장.
>  integer 외에도 long int(stol), unsigned int(stoul), long long(stoll), unsigned long lon(stoull), float(stof), double(stod), long double(stold)로 변환하는 함수가 지원된다.
#### `string to_string(val)`
val을 string으로 변환한 값을 반환하는 함수. val의 경우 int, long, long long, float, double, long double이 지원됨. 
