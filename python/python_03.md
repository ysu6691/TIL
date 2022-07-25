# Python 데이터 구조 및 활용

## 1. 데이터 구조

### 데이터 구조 활용
- 데이터 구조를 활용하기 위해서는 메서드(method)를 활용
```python
# 데이터 구조.메서드() 형태로 활용
# 예시
a = [1, 2, 3] # list
a.append(10)
print(a) # [1, 2, 3, 10]

b = 'a b c' # string
print(b.split()) # ['a', 'b', 'c']

# 파이썬 공식 문서의 표기법(배커스-나우르 표기법)
# 예시
str.replace(old,new[, count]) # old, new는 필수, [] 내부는 선택적 인자
```

## 2. 시퀀스형 데이터 구조

### 문자열
- 문자열 조회/탐색 메서드
  - s.find(x): x의 첫 번째 위치를 반환. 없으면, -1을 반환
  - s.index(x): x의 첫 번째 위치를 반환. 없으면, 오류 발생
  - s.isalpha(): 알파벳 문자 여부(단순 알파벳이 아닌 유니코드 상 letter) -> 숫자는 x
  - s.isupper(): 대문자 여부
  - s.islower(): 소문자 여부
  - s.istitle(): 타이틀 형식 여부 (공백을 기준으로 각 단어의 첫 시작만 대문자인지 확인)

```python
s = 'abc'
print(s.find('a')) # 0
print(s.find('d')) # -1
print(s.index('a')) # 0
print(s.index('d')) # ValueError: substring not found
print(s.isalpha()) # True
print('ㄱㄴㄷ'.isalpha()) # True
print('a1'.isalpha()) # False(숫자 포함)
print('Ab'.isupper()) # False
print('ab'.islower()) # True
print('Title Title!'.istitle()) # True
print('Title title!'.istitle()) # False
```

- 문자열 변경 메서드
  - 문자열은 immutable이므로 기존 문자열이 변경되는 것이 아니라, 변경된 문자열을 새롭게 만들어서 반환
  - s.replace(old, new[, count]): 바꿀 대상 글자를 새로운 글자로 바꿔서 반환
  - s.strip([chars]): 공백이나 특정 문자를 제거
  - s.split(sep = None, maxsplit = -1): 공백이나 특정 문자를 기준으로 분리
  - 'separator'.join([iterable]): 구분자(separator)로 iterable을 합침
  - s.capitalize(): 가장 첫 번째 글자를 대문자로 변경
  - s.title(): 공백 기준으로 각 단어의 첫 글자는 대문자로, 나머지는 소문자로 변환
  - s.upper(): 모두 대문자로 변경
  - s.lower(): 모두 소문자로 변경
  - s.swapcase(): 대소문자 서로 변경

```python
print('abc'.replace('a', 'z')) # zbc
print('aaabc'.replace('a', 'z', 2)) # zzabc
print('  abc?  '.strip()) # 'abc' (양쪽 공백 제거)
print('  abc?  '.lstrip()) # 'abc  ' (왼쪽 공백 제거)
print('  abc?  '.rstrip()) # '  abc' (오른쪽 공백 제거)
print('  abc???'.rstrip('?')) # '  abc'
print('a,b,c'.split(',')) # ['a', 'b', 'c']
print(' '.join(['1', '2', '3'])) # '1 2 3'
print(' '.join([1, 2, 3])) # 오류 발생 (문자열만 가능)
s = 'hI! Everyone'
print(s.capitalize()) # Hi! everyone
print(s.title()) # Hi! Everyone
print(s.upper()) # HI! EVERYONE
print(s.lower()) # hi! everyone
print(s.swapcase()) # Hi! eVERYONE
```

### 리스트
- 리스트 값 추가 및 삭제
  - L.append(x): 리스트 마지막에 항목 x를 추가
  - L.insert(i, x): 리스트 인덱스 i에 항목 x를 삽입
  - L.remove(x): 리스트 가장 왼쪽에 있는 첫 번째 x를 제거, 없으면 오류 발생
  - L.pop(): 리스트 마지막 항목을 반환 후 제거
  - L.extend(m): 순회형 m의 모든 항목들의 리스트 끝에 추가 (+=과 같은 기능)
  - L.clear(): 리스트 내 모든 항목 삭제

```python
a = ['a', 'b', 'c']
a.append('d')
print(a) # ['a', 'b', 'c', 'd']

a = ['a', 'b', 'c']
a.insert(0, 'z')
print(a) # ['z', 'a', 'b', 'c']
a.insert(100, 'z') # 리스트 길이보다 큰 값을 설정한 경우, 맨 뒤로 추가
print(a) # ['z', 'a', 'b', 'c', 'z']

a = ['a', 'b', 'c']
a.extend(['d', 'e'])
print(a) # ['a', 'b', 'c', 'd', 'e']
a += ['f', 'g']
print(a) # ['a', 'b', 'c', 'd', 'e', 'f', 'g']
a.extend('end') # 문자열 그대로 추가하면 각 문자들이 추가
print(a) # ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'e', 'n', 'd']

a = ['a', 'b', 'c']
a.remove('a')
print(a) # ['b', 'c']
a.remove('z') # 오류 발생

a = ['a', 'b', 'c']
a.pop(1)
print(a) # ['a', 'c']
a.pop() # 인덱스 지정하지 않으면 마지막 항목 삭제
print(a) # ['a']

a = ['a', 'b', 'c']
a.clear
print(a) # []
```

- 리스트 탐색 및 정렬
  - L.index(x, start, end): 가장 왼쪽에 있는 항목 x의 인덱스를 반환
  - L.reverse(): 리스트를 거꾸로 정렬
  - L.sort(): 리스트를 정렬
  - L.count(x): 리스트 내 항목 x의 개수를 반환

```python
a = ['a', 'b', 'c']
print(a.index(2)) # 'c'
print(a.index(100)) # 오류 발생

a = ['a', 'a', 'a', 'b', 'c']
print(a.count('a')) # 3
print(a.count('z')) # 0

a = [1, 4, 3, 2]
a.reverse() # 기존 리스트가 변경
print(a) # [2, 3, 4, 1]
result = a.reverse()
print(result, a) # None [1, 4, 3, 2]

a = [1, 4, 3, 2]
a.sort() # 기존 리스트가 변경
print(a) # [1, 2, 3, 4]
result = a.sort()
print(result, a) # None [1, 2, 3, 4]

# .sort() vs sorted()
a = [1, 4, 3, 2]
result = sorted(a) # 기존 리스트는 변경 x
print(result, a) # [1, 2, 3, 4] [1, 4, 3, 2]
```

### 튜플
- 튜플은 immutable이므로 값에 영향을 미치지 않는 메서드만을 지원
- 리스트 메서드 중 항목을 변경하는 메서드를 제외하고 대부분 동일
```python
a = (1, 4, 3, 2)
print(a.index(2)) # 3
print(a.count(2)) # 1

a = (1, 4, 3, 2)
a.sort() # 오류 발생 (튜플 변경 불가)
b = sorted(a) # a는 변화 x, b에 새로운 리스트 생성
print(b) # [1, 2, 3, 4]

a = (1, 4, 3, 2)
print(id(a)) # 18778...9152
a.extend([4, 5]) # 오류 발생 (튜플 변경 불가)
a += (5, 6) # a 변경 (오류 발생 x)
print(a) # (1, 4, 3, 2, 5, 6)
printt(id(a)) # 18778...0192
# a의 id 값이 바뀜 (새로운 튜플로 지정됨)
```

### 시퀀스형 연산자(Sequence Type Operator)
- 산술연산자(+)
```python
a = [1, 2]
a += [3]
print(a) # [1, 2, 3]

a = (1, 2)
a += (3,)
print(a) # (1, 2, 3) # 단, 기존의 id와 같은 id는 아님

a = '12'
a += '3'
print(a) # '123'
```

- 반복연산자(*)
```python
a = [1, 2]
a *= 3
print(a) # [1, 2, 1, 2, 1, 2]

a = (1, 2)
a *= 3
print(a) # (1, 2, 1, 2, 1, 2) # 단, 기존의 id와 같은 id는 아님

a = '12'
a *= 3
print(a) # '121212'
```

## 비시퀀스형 데이터 구조

### 셋
- mutable로서, 담고 있는 요소 삽입 변경, 삭제 가능
  - s.copy(): 셋의 얕은 복사본을 반환
  - s.add(x): 항목 x가 셋에 없다면 추가
  - s.pop(): 랜덤하게 항목 반환하고 제거, 셋이 비어 있을 경우 오류 발생
  - s.remove(x): 항목 x를 셋에서 제거, x가 존재하지 않을 경우 오류 발생
  - s.discard(x): 항목 x가 셋 s에 있는 경우, x를 제거
  - s.update(t): 셋 t에 있는 모든 항목 중 셋 s에 없는 항목을 추가
  - s.clear(): 모든 항목 제거
  - s.isdisjoint(t): 셋 s와 셋 t가 서로 같은 항목을 하나라도 갖고 있지 않은 경우, True 반환
  - s.issubset(t): 셋 s가 셋 t의 하위 셋인 경우, True 반환
  - s.issuperset(t): 셋 s가 셋 t의 상위 셋인 경우, True 반환

```python
a = {1, 2, 3}
a.add(4)
print(a) # {1, 2, 3, 4}

a = {1, 2, 3}
a.update([3, 4, 5])
print(a) # {1, 2, 3, 4, 5}

a = {1, 2, 3}
a.remove(1)
print(a) # {2, 3}
a.remove(4) # 오류 발생

a = {1, 2, 3}
a.discard(1)
print(a) # {2, 3}
a.discard(4) # 오류 발생 x
print(a) # {2, 3}

a = {1, 2, 3}
a.pop() # 임의 원소(1) 반환 & 제거
print(a) # {2, 3}
```

### 딕셔너리
- mutable로서, 담고 있는 요소 삽입, 변경, 제거 가능
  - d.clear(): 모든 항목을 제거
  - d.copy(): 딕셔너리의 얕은 복사본을 반환
  - d.keys(): 딕셔너리의 모든 키를 담은 뷰를 반환
  - d.values(): 딕셔너리의 모든 값을 담은 뷰를 반환
  - d.items(): 딕셔너리의 모든 키-값의 쌍을 담은 뷰를 반환
  - d.get(k): 키 k의 값을 반환하는데, k가 딕셔너리에 없을 경우 None을 반환
  - d.get(k, v): 키 k의 값을 반환하는데, k가 딕셔너리에 없을 경우 v를 반환
  - d.pop(k): 키 k의 값을 반환하고 딕셔너리에서 삭제, k가 딕셔너리에 없을 경우 오류 발생
  - d.pop(k, v): 키 k의 값을 반환하고 딕셔너리에서 삭제, k가 딕셔너리에 없을 경우 v를 반환
  - d.update([other]): 딕셔너리의 값을 매핑하여 업데이트

```python
a = {'apple': '사과', 'banana': '바나나'}
print(a.get('apple')) # 사과
print(a.get('pineapple')) # None
print(a.get('pineapple', 0)) # 0

a = {'apple': '사과', 'banana': '바나나'}
data1 = a.pop('apple')
print(data, a) # 사과 {'banana': '바나나'}
data2 = a.pop('pineapple') # 오류 발생
data3 = a.pop('pineapple', 0)
print(data3, a) # 0 {'banana': '바나나'}

a = {'apple': '사과', 'banana': '바나나'}
a.update(apple = '풋사과')
print(a) # {'apple': '풋사과', 'banana': '바나나'}
```

## 4. 얕은 복사와 깊은 복사

### 복사의 종류
- 할당(assignment)
- 얕은 복사 (Shallow copy)
- 깊은 복사 (Deep copy)

### 할당(assignment)
- 대입 연산자(=)
```python
original_list = [1, 2, 3]
copy_list = original_list # original_list와 같은 리스트를 할당 받음
print(original_list, copy_list) # [1, 2 ,3] [1, 2, 3]

original_list[0] = 10
print(original_list, copy_list) # [10, 2, 3] [10, 2, 3]
copy_list[1] = 20
print(original_list, copy_list) # [10, 20, 3] [10, 20, 3]
```

### 얕은 복사(shallow copy)
- slice 연산자 활용하여 같은 원소를 가진 리스트지만, 연산된 결과를 복사 (다른 주소를 가짐)
```python
a = [1, 2, 3]
b = a[:]
print(a, b) # [1, 2, 3] [1, 2, 3]
a[0] = 10
print(a, b) # [10, 2, 3] [1, 2, 3]

# 얕은 복사 주의사항
# 복사하는 리스트의 원소가 주소를 참조하는 경우 (2차원 이상의 리스트)
a = [1, 2, [3, 4]]
b = a[:]
print(a, b) # [1, 2, [3, 4]] [1, 2, [3, 4]]
a[2][0] = 10
print(a, b) # [1, 2, [10, 4]] [1, 2, [10 ,4]]
```

### 깊은 복사(deep copy)
- copy 모듈 활용해 같은 주소를 갖지 않게 함
```python
import copy
a = [1, 2, [3, 4]]
b = copy.deepcopy(a)
print(a, b) # [1, 2, [3, 4]] [1, 2, [3, 4]]
a[2][0] = 10
print(a, b) # [1, 2, [10, 4]] [1, 2, [3, 4]]
```

### 할당, 얕은 복사, 깊은 복사 이해하기
- [Python Tutor](https://pythontutor.com/)

