## types
#### vector
c++ 에서의 Array를 대체하는 class. 배열의 크기 변화에 따라 공간 재할당을 자동으로 수행한다.
#### Constructors
*  vector<T> v: T type 원소를 갖는 빈 벡터 v 생성.
*  vector<T> v(nitem, val): T type 값 val이 nitem개 담긴 벡터 생성.
*  vector<T> v(iter1, iter2): T type vector의 iter1부터 iter2까지 값을 담은 벡터 생성.
*  vector<T> v(vec): vec을 복사한 벡터 생성.
#### Operators
*  v1 = v2: v1에 v2 값을 copy. 이후 v2 값이 바뀌어도 v1엔 반영되지 않음. 
*  v1 > v2: v1과 v2의 크기를 비교하는 함수. 우선 각 벡터의 size를 비교하고, size가 같다면 0번째 value의 크기를 비교. 0번째 value도 같다면 다음 value 비교.
*  v[i]: v의 i번째 원소에 접근. 매크로라 속도가 빠르지만 범위 체크를 하진 않아 주의해야 한다.
#### Functions
##### size(): 벡터의 원소 개수를 반환. 
##### capacity(): 벡터의 용량을 반환. 
##### resize(size, [init_val=0]): 벡터의 size를 재조정. 만약 이전보다 커질 경우 해당 vector 값은 init_val로 초기화.
##### reserve(capacity): 벡터의 용량을 capacity로 재조정.
##### empty(): 벡터가 비었는지를 확인하는 함수.
##### back(), front(): 벡터의 가장 뒤, 앞 문자를 반환.
##### at(i): i번째 vector 값을 가져오는 함수. 
##### assign: 벡터의 내용을 설정하는 함수. 기존 내용은 사라짐.
*  v.assign(n, val): v를 val n개의 vector로 설정.
*  v.assign(iter1, iter2): v를 iter1부터 iter2까지의 vector로 설정.
*  v.assign(T* arr, T* arr_k): v를 T* arr부터 arr_k까지의 값으로 설정. 
##### push_back(val): 벡터 끝에 값 val을 이어 붙이는 함수.
##### pop_back(): 벡터 끝의 값을 제거하는 함수.
##### insert: 벡터에 값을 삽입하는 함수로, 추가 옵션들이 존재한다.
*  v.insert(iter, val): v의 iter 위치에 val을 삽입.
*  v.insert(iter, n, val): v의 iter 위치에 val을 n번 삽입.
*  v.insert(iter, iter_val, iter_val_end): v의 iter 위치에 iter_val부터 iter_val_end까지 삽입.
*  v.insert(iter, T* arr, T* arr_k): v의 iter 위치에 arr부터 arr_k까지를 삽입.
##### emplace: 벡터에 값을 삽입하는 함수로, insert와 삽입 하는 방식에 차이가 있다. 
*  옵션의 경우 insert 참조.
*  push_back(val)을 대체하는 emplace_back(val) 또한 존재한다.
> v.emplace의 경우 인자로 받은 값을 이용해 새 임시 객체를 생성 후, 임시 객체를 v에 삽입하는 방식. 따라서 vector 원소와 인자가 연동되어 의도치 않은 값 수정을 방지할 수 있다.
##### erase: 벡터 원소를 지우는 함수.
*  v.erase(iter): v의 iter 위치의 원소를 삭제.
*  v.erase(iter1, iter2): v의 iter1부터 iter2까지의 원소 삭제.
##### .swap(target): dest와 target의 벡터 값을 교환하는 함수.
##### clear(): vector 값을 비우는 함수. 
##### flip(): vector가 vector<bool>일 때만 가능한 함수로, vector 내의 모든 bool 값을 뒤집는 함수.

------------------------------------------
