# Python 기초

## 1. 파이썬이란?

### 파이썬 특징
- Easy to Learn
- 인터프리터 언어(Interpreter)
- 객체 지향 프로그래밍

### 파이썬 개발환경
- IDE(Integrated Development Environment)
  - Visual Studio Code
  - Pycharm
- Jupyter Notebook
- IDLE(Integrated Development and Learning Environment)



## 2. 기초 문법

### 코드 작성법
코드 스타일 가이드: 코드를 '어떻게 작성할지'에 대한 가이드라인
파이썬에서 제안하는 스타일 가이드: PEP8
- 들여쓰기: space키 4번 or Tab키 1번
- 주석: 한 줄 주석: # / 여러 줄 주석: '''로 묶기

### 변수
변수는 데이터를 저장하기 위해 사용
할당 연산자(=)를 통해 값을 할당
```python
# a에 10을 할당
a = 10
print(a) # 10
```
```python
# x = 10, y = 20 일 때, 각각의 값을 바꿔서 저장
x, y = 10, 20
y, x = x, y
print(x, y) # 20 10
```

* 식별자: 영문 알파벳, 언더스코어(_), 숫자로 구성 (처음에 숫자가 올 수 없음)
* 예약어: 식별자로 사용 불가능한 키워드 ex) if, for, and, def, True, ...

### 연산자
산술 연산자: 기본적인 사칙연산 및 수식 계산
```python
print(5 + 3) # 5 + 3 = 8
print(5 - 3) # 5 - 3 = 2
print(5 * 3) # 5 * 3 = 18
print(5 / 3) # 5 / 3 = 1.666667
print(5 // 3) # 5를 3으로 나눈 몫: 1
print(5 % 3) # 5를 3으로 나눈 나머지: 2
print(5 ** 3) # 5의 3제곱: 125
```



## 3. 자료형

### 자료형(Datatype)의 분류
- Numeric Type
  - Int
  - Float
  - Complex
- String Type
- Boolean Type

### 수치형(Numeric Type)
- 정수 자료형(Int): 0, 100, -200과 같은 정수를 표현
- 실수 자료형(Float): 0.1, 100.0, -0.001과 같은 실수를 표현
```python
# 실수 연산시 주의할 점 (부동 소수점)
print(3.2 - 3.1) # 0.100000000
print(1.2 - 1.1) # 0.099999999
# 둘의 결과가 다름

# 해결책
import math
print(math.isclose(a, b)) # True
```
- 진수 표현: 여러 진수 표현 가능
```python
# 2진수: 0b
print(0b10) # 2

# 8진수: 0o
print(0o30) # 24

# 16진수: 0x
print(0x10) # 16
```

### 문자열 자료형(String Type)
- 작은따옴표(')나 큰따옴표(")를 활용하여 표기
```python
print("hello") # hello
print('hi') # hi

# 중첩 따옴표
print("문자열 안에 '작은따옴표' 사용하려면 큰 따옴표로 묶는다.")
print('문자열 안에 "큰따옴표" 사용하려면 작은 따옴표로 묶는다.')

# 삼중 따옴표
print('''문자열 안에 '작은따옴표'나
"큰 따옴표를 사용할 수 있고
여러 줄을 사용할 때도 편리하다.''')
```

- Escape sequence: 역슬래시 뒤에 특정 문자가 와서 특수한 기능을 함
```python
# \n: 줄 바꿈
# \t: 탭
# \\: \
# \': 작은따옴표(')
# \": 큰따옴표(")
```

- 문자열 연산
```python
# 덧셈
print("Hello" + "World") # HelloWorld

# 곱셈
print("hi" * 3) # hihihi
```

- String Interpolation
```python
# f-strings
name = "Kim"
print(f'Hello, {name}!') # Hello, kim!
```

### 불린형(Boolean Type)
논리 자료형으로 참과 거짓을 표현하는 자료형
- 비교 연산자: 등호, 부등호와 동일 개념
```python
print(6 > 3) # True
print(3 <= 0) # False
print('3' != 3) # True
```

- 논리 연산자
```python
# A and B: A와 B 모두 True시, True
# A or B: A와 B 중 하나라도 True시, True
# Not: True를 False로, False를 True로

print(True and True) # True
print(True and False) # False
print(False and True) # False
print(False and False) # False

print(True or True) # True
print(True or False) # True
print(False or True) # True
print(False or False) # False

# Falsy: False는 아니지만 False로 취급되는 값들
# ex) 0, [], None, ""

# 논리 연산자도 우선순위가 존재 -> not, and, or 순으로 우선순위가 높음
print(((not True) and False) or (not False)) # True

# 논리 연산자의 단축 평가
# 결과가 정해지는 대로 값 반환
print(3 and 5) # 5
print(3 and 0) # 0
print(0 and 3) # 0

print(5 or 3) # 5
print(3 or 0) # 3
print(0 or 3) # 3
```



## 4. 컨테이너
여러 개의 값을 담을 수 있는 것(객체)
순서가 있는 데이터(Ordered) vs 순서가 없는 데이터(Unordered)
(순서가 있다 != 정렬되어 있다)

### 시퀀스형 컨테이너
- 리스트
여러 개의 값을 순서가 있는 구조로 저장하고 싶을 때 사용
어떠한 자료형도 저장 가능, 생성 이후 내용 변경 가능(mutable)
```python
# 리스트 생성
list_a = [] # list_a = ilst()와 같음
list_b = [1, 2, 3]
list_c = [1, 2, 'a', 'b', ['ABC', 'DEF']]

print(list_b[0]) # 1
print(len(list_b)) # 3
print(list_b[2][1][1]) # E

list_b[0] = 5 # 리스트 요소 변경
print(list_b) # [5, 2, 3]

# 2차원 리스트 주의할 점!
matrix = [[0]*3]*3 # 같은 [0]*3이 matrix에 할당됨
print(matrix) # [[0, 0, 0], [0, 0, 0], [0, 0, 0]]
matrix[0][0] = 1
print(matrix) # [[1, 0, 0], [1, 0, 0], [1, 0, 0]]
# 1번째 리스트의 한 요소만 바꾸고 싶은데, 전체 리스트의 요소가 다 바뀜

# 따라서 다음과 같이 만들어야 함
matrix = []
for _ in range(3):
  matrix.append([0]*3) # for문 안에서 append 할 때마다 새로운 [0]*3이 할당됨
print(matrix) # [[0, 0, 0], [0, 0, 0], [0, 0, 0]]
matrix[0][0] = 1
print(matrix) # [[1, 0, 0], [0, 0, 0], [0, 0, 0]]
```

- 튜플
여러 개의 값을 순서가 있는 구조로 저장하고 싶을 때 사용
리스트와의 차이점: 담고 있는 값 변경 불가(immutable)
```python
a = (1, 2, 3, 1)
print(a[1]) # 2

# 단일 항목 생성
tuple_a = (1,)
print(tuple_a) # (1,)

# 괄호 없이도 튜플 생성 가능
tuple_a = 1,
print(tuple_a) # (1,)
```

- Range
숫자의 시퀀스를 나타내기 위해 사용
```python
print(range(4)) # range(0, 4)
print(list(range(4))) # [0, 1, 2, 3]
print(list(range(1, 4))) # [1, 2, 3]
print(list(range(1, 4, 2))) # [1, 3]
print(list(range(6, 1, -1))) # [6, 5, 4, 3, 2]
print(list(range(6, 1, 1))) # []
```

- 슬라이싱 연산자
인덱스와 콜론을 사용하여 문자열의 특정 부분만 잘라낼 수 있음
```python
print([1, 2, 3, 4][1:4]) # [2, 3, 4]
print((1, 2, 3, 4)[:2]) # (1, 2)
print(range(10)[5:8]) # range(5, 8)
print('abcd'[2:4]) # cd
print('abcdefg'[1:5:2]) # bd
print('abcdefg'[::]) # abcdefg
print('abcdefg'[::-1]) # gfedcba
```

### 비시퀀스형 컨테이너
- 셋(set)
중복되는 요소가 없이, 순서에 상관없는 데이터들의 묶음
인덱스 이용한 접근 불가능, 가변 자료형(mutable)
```python
# 셋 생성
a = {1, 2, 3, 3, 3} # 빈 중괄호({}) 사용시, 셋 대신 딕셔너리 생성
b = set()

# 셋 사용하기
my_list = ['서울', '서울', '서울', '대전', '대전']
print(set(my_list)) # {'서울', '대전'}
print(len(set(my_list))) # 2

# 셋 연산자
A = {1, 2, 3, 4}
B = {3, 4, 5, 6}

print(A | B) # {1, 2, 3, 4, 5, 6}
print(A & B) # {3, 4}
print(A - B) # {1, 2}
print(A ^ B) # {1, 2, 5, 6}
```

- 딕셔너리
키(key)-값(value) 쌍으로 이뤄진 자료형
key는 변경 불가능한 데이터(immutable)만 활용 가능
(string, integer, float, boolean, tuple, range)
value는 어떠한 형태는 관계없음

```python
딕셔너리 생성
a = {}
b = dict()
c = {'A': 'apple', 'B': 'banana', 'list': [1, 2, 3]}
print(c) # {'A': 'apple', 'B': 'banana', 'list': [1, 2, 3]}
d = dict(A = 'apple', B = 'banana', list = [1, 2, 3])
print(d) # {'A': 'apple', 'B': 'banana', 'list': [1, 2, 3]}

print(d.keys()) # dict.keys(['A', 'B', 'list'])
print(d.values()) # dict_items(['apple', 'banana', [1, 2, 3]])
print(d.items()) # dict_items([('A', 'apple'), ('B', 'banana'), ('list', [1, 2, 3])])

# dict[]와 dict.get()의 차이점
dict = {'a': 1, 'b': 2, 'c': 3}
dict['a'] # 1
dict.get('a') # 1

dict['d'] # 오류 발생
dict.get('d') # None
dict.get('d', '없음') # None 대신 '없음' 출력
```



## 5. 형변환
데이터 형태는 서로 변환할 수 있음

- 암시적 형 변환
```python
print(True + 3) # 4
print(3 + 5.0) # 8.0
```

- 명시적 형 변환
```python
print(int('3') + 4) # 7
print(str(3)) # 3
```

