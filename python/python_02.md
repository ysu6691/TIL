# Python 조건문, 반복문, 함수, 모듈

## 1. 조건문

### 조건문 기본
조건문은 참/거짓을 판단할 수 있는 조건식과 함께 사용

```python
# num 값의 홀수/짝수 여부를 출력
num = int(input())

if num % 2: # 1 -> True
    print('홀수입니다.')
else:
    print('짝수입니다.)
```

### 복수 조건문
복수의 조건식을 활용할 경우 elif를 활용하여 표현

```python
dust = 80

if dust > 150:
    print('매우 나쁨')
elif dust > 80:
    print('나쁨')
elif dust > 30:
    print('보통')
else:
    print('좋음')

# 보통
```

### 중첩 조건문
다른 조건문에 중첩되어 조건문 사용 가능

```python
dust = 330

if dust > 150:
    print('매우 나쁨')
    if dust > 300:
        print('실외 활동을 자제하세요.')
elif dust > 80:
    print('나쁨')
elif dust > 30:
    print('보통')
else:
    print('좋음')

# 매우 나쁨
# 실외 활동을 자제하세요.
```

### 조건 표현식
조건문을 한 줄로 표현
삼항 연산자로 부르기도 함

```python
# true인 경우 if 조건 else false인 경우

num = 2
result = '홀수입니다.' if num % 2 else '짝수입니다.'
print(result) 

# 짝수입니다.
```



## 2. 반복문

### while문
조건식이 참인 경우 반복적으로 코드를 실행
무한 루프를 하지 않도록 종료 조건이 반드시 필요

```python
# while문 예시
a = 0
while a < 3:
    print(a)
    a += 1 # 복합 연산자(연산과 할당을 합쳐 놓은 것)

# 0
# 1
# 2
```

### for문
시퀀스(string, tuple, list, range)를 포함한 순회 가능한 객체(iterable)의 요소를 모두 순회
- Iterable
  - 순회할 수 있는 자료형(string, list, dict, tuple, range, set 등)
  - 순회형 함수(range, enumerate)

```python
# for문 예시

# list 순회
for fruit in ['apple', 'mango', 'banana']:
    print(fruit)

'''
apple
mango
banana
'''


# string 순회(1)
chars = 'car'
for idx in chars:
    print(idx)

'''
c
a
r
'''


# string 순회(2)
chars = 'car'
for idx in range(len(chars)):
    print(chars[idx])

'''
c
a
r
'''


# 딕셔너리 순회(1)
# 기본적으로 key를 순회함
grades = {'john': 80, 'eric': 90}
for student in grades:
    print(student, grades[student]) # key, value 순으로 출력

'''
john 80
eric 90
'''


# 딕셔너리 순회(2)
grades = {'john': 80, 'eric': 90}
for student, grade in grades.items(): 
    # grades.items(): dict.items([('john', 80), ('eric', 90)])
    print(student, grade)

'''
john 80
eric 90
'''


# enumerate 순회(1)
# 인덱스와 객체를 쌍으로 담은 열거형 객체 반환
members = ['민수', '영희', '철수']

for idx, name in enumerate(members):
    print(idx, name)

'''
0 민수
1 영희
2 철수
'''


# enumerate 순회(2)
members = ['민수', '영희']
enumerate(members) # enumerate at ...
print(list(enumerate(members))) # [(0, '민수'), (1, '영희')]
print(list(enumerate(members, start = 1))) # [(1, '민수'), (2, '영희')]


# list comprehension
# 표현식과 제어문을 통해 리스트를 간결하게 생성
# [code for 변수 in iterable (if 조건식)]
cubic_list = []
for number in range(1, 4):
    cubic_list.append(number ** 3)
print(cubic_list) # [1, 8, 27]

cubic_list = [number ** 3 for number in range(1, 4)]
print(cubic_list) # [1, 8, 27]


# dictionary comprehension
# 표현식과 제어문을 통해 리스트를 간결하게 생성
#{key: value for 변수 in iterable (if 조건식)}
cubic_dict = {}
for number in range(1, 4):
    cubic_dict[number] = number ** 3
print(cubic_dict) # {1: 1, 2: 8, 3: 27}

cubic_dict = {number: number ** 3 for number in range(1, 4)}
print(cubic_dict) # {1: 1, 2: 8, 3: 27}
```

### 반복문 제어
- break: 반복문을 종료
- continue: 이후 코드 블록은 수행하지 않고, 다음 반복을 수행
- pass: 아무것도 하지 않음
- for-else: 끝까지 반복문을 실행한 이후에 else문 실행

```python
# break
for i in range(3):
    if i > 2:
        break
    print(i)

'''
0
1
'''

# continue
for i in range(6):
    if i % 2 == 0:
        continue
    print(i)

'''
1
3
5
'''

# pass
for i in range(3):
    if i == 2:
        pass
    print(i)

'''
0
1
2
'''

# else
for char in 'apple':
    if char == 'b':
        print('b!')
        break
else:
    print('b가 없습니다.')

# b가 없습니다.
```



## 3. 함수

### 함수의 기초
- 내장 함수: 파이썬에 기본적으로 포함된 함수
- 외장함수: import 문을 통해 사용하며, 외부 라이브러리에서 제공하는 함수
- 사용자 정의 함수: 직접 사용자가 만드는 함수

```python
def func1():
    return True
print(fun1()) # True

def func2(x, y):
    return x + y
print(fuc2(1, 2)) # 3
```

### 함수의 결과값
- void function: 명시적인 return 값이 없는 경우, None을 반환하고 종료
  - print vs return: print는 값을 출력할 뿐, 반환하지 않음.

```python
def void_function(x, y):
    print(x + y)
result = void_function(1, 2)
print(result) # None

def value_returning_function(x, y):
    return x + y
result = value_returning_function(1, 2):
print(result) # 3
```

- 튜플을 활용하여 두 개 이상의 값 반환
```python
def func(x, y):
    return x + y, x - y
result = func(5, 3)
print(result) # (8, 2)
print(type(result)) # <class 'tuple'>
```

### 함수의 입력
- Parameter와 Argument
  - Parameter: 함수를 정의할 때, 함수 내부에서 사용되는 변수
  - Argument: 함수를 호출할 때, 넣어주는 값
    - 필수 Argument: 반드시 전달되어야 하는 argument
    - 선택 Argument: 값을 전달하지 않아도 되는 경우는 기본값이 전달
    
```python
def func(ham): # parameter: ham
    return ham
func('spam') # argument: 'spam'

# positional arguments
# 위치에 따라 함수 내에 전달
def add(x, y):
    return x + y
add(2, 5) # 7

# keyword arguments
# 직접 변수의 이름으로 특정 argument 전달
def add(x, y):
    return x + y
add(x = 2, y = 5) # 7 (keyword arguments)
add(2, y = 5) # 7 (positional argument + keyword argument)
add(x = 2, 5) # Error 발생 (keyword argument 다음에 positional argument를 활용할 수 없음)

# default arguments values
기본값 지정시 argument 값 설정안해도 무방
def add(x, y = 0):
    return x + y
add(2) # 2 ()
```

- 가변 인자(*args) 
여러 개의 positional argument를 하나의 필수 parameter로 받아서 사용

```python
# 몇 개의 argument를 받을지 모르는 함수를 정의할 때 유용
def add(*args):
    for arg in args:
        print(arg)
add(2) # 2
add(2, 3, 4) # 9

def add(*args):
    print(args)
    print(type(args))
add(1, 2, 3, 'a', 'b')

'''
(1, 2, 3, 'a', 'b')
(class 'tuple')
'''

# 패킹과 언패킹
# 패킹: 여러 개의 데이터를 묶어서 변수에 할당
nums = (1, 2, 3)
print(nums) # (1, 2, 3)

# 언패킹: 시퀀스 속의 요소들을 여러 개의 변수에 나누어 할당하는 것
nums = (1, 2, 3)
a, b, c = (nums) # 변수의 개수와 요소의 개수가 같아야함
print(a, b, c) # 1 2 3 

# 언패킹시 변수에 asterisk(*)를 붙이면, 할당하고 남은 요소를 리스트에 담을 수 있음
nums = (1, 2, 3, 4)
a, *rest, b = nums
print(a, rest, b) # 1 [2, 3] 4

# 기본 인자와 가변 인자의 동시 사용
def print_family_name{father, mother, *pets}:
    print(f'아버지 : {father}')
    print(f'어머니 : {mother}')
    for name in pets:
        print(f'반려동물 : {name})
print(print_family_name('아부지', '어무니', '강아지', '고양이'))

'''
아버지 : 아부지
어머니 : 어무니
반려동물 : 강아지
반려동물 : 고양이
'''
```

- 가변 키워드 인자(**kwargs)
파라미터를 딕셔너리로 묶어서 처리

```python
# 몇 개의 키워드 인자를 받을지 모르는 함수를 정의할 때 유용
def family(**kwargs):
    for key, value in kwargs.items():
        print(key, ":", value)
family(father = '아버지', mother = '어머니', baby = '아기')

'''
father : 아버지
mother : 어머니
baby : 아기
'''

# 가변 인자와 가변 키워드 인자의 동시 사용
def print_family_name(*parents, **pets):
    print('아버지 :', parents[0])
    print('어머니 :', parents[1])
    if pets: # pets이 있으면 실행
        for title, name in pets.items():
            print(f'{title} : {name}')
print_family_name('아부지', '어무니', dog = '강아지', cat = '고양이')

'''
아버지 : 아부지
어머니 : 어무니
dog : 강아지
cat : 고양이
'''
```

### Python의 범위(Scope)
- scope
  - built-in scope
    - 파이썬이 실행된 이후부터 영원히 유지
  - global scope
    - 코드 어디에서든 참조할 수 있는 공간
    - 모듈이 호출된 시점 이후 혹은 인터프리터가 끝날 때까지 유지
  - local scope
    - 함수가 만든 scope. 함수 내부에서만 참조 가능
    - 함수가 종료될 때까지 유지
    
- variable
  - global variable: global scope에 정의된 변수
  - local variable: local scope에 정의된 변수

- 이름 검색 규칙 (LEGB Rule)
  - 파이썬에서 사용되는 식별자는 이름공간(namespace)에 저장되어 있음
  - 아래와 같은 순서로 이름을 찾아나감
    - Local scope: 지역 범위(현재 작업 중인 범위)
    - Enclosed scope: 지역 범위 한 단계 위 범위
    - Global scope: 최상단에 위치한 범위
    - Built-in scope: 모든 것을 담고 있는 범위(정의하지 않고 사용할 수 있는 모든 것)
      - ex) print()
  - 함수 내에서는 바깥 scope의 변수에 접근 가능하나, 수정은 불가

```python
# LEGB 예시(1)
print(sum(range(3))) # 3, built-in scope에 위치한 sum 탐색
sum = 10 # global scope에 sum 지정
print(sum(range(3))) # 오류 발생, global scope에 위치한 sum을 우선 탐색

# LEGB 예시(2)
a = 0
b = 1
def enclosed():
    a = 10
    c = 2
    def local(c):
        print(a, b, c)
    local(100) # 10 1 100
    print(a, b, c)
enclosed() # 10 1 2
print(a, b) # 0 1
```

- global 문: 현재 코드 블록 전체에 적용 -> 식별자를 global variable로 지정

```python
a = 0
def func():
    global a
    a = 10

print(a) # 0
func()
print(a) # 10

# global에 나열된 이름은 global 앞에 등장할 수 없음
# global에 나열된 이름은 parameter로 사용 불가
def func():
    a = 10
    global a # 오류 발생

def func(a):
    global a # 오류 발생
```

- nonlocal
  - global을 제외하고 가장 가까운 (둘러싸고 있는) scope의 변수를 연결
  - nonlocal에 나열된 이름은 parameter로 사용 불가
```python
def func1():
    x = 1
    def func2():
        nonlocal x
        x = 2
    func2()
    print(x)

func1() # 2
```

- LEGB 심화
```python
# (1)
nums = [1, 2, 3]

def a():
    nums[0] = 10
a()
print(nums) # [10, 2, 3] -> global 없이도 바뀜

# (2)
a = 3
print(id(a)) # 164...6032

def first():
    global a
    print(id(a)) #164...6032 -> 같음

first()

# (3)
def first():
    a = 4
    print(id(a)) # 210...4560

    def second():
        nonlocal a
        print(id(a)) # 210...4560 -> 같음
    second()

first()

# (4)
def first():
    a = 6
    print(id(a)) # 198...7760

    def second():
        print(id(a)) # 198...7760 -> 같음
    second()

first()

# (5)
def first():
    a = 6
    print(id(a)) # 318...9472

    def second():
        print(id(a)) # 오류 발생 (우선적으로 같은 이름공간 내에서 할당된 값을 가져오기 때문에, a = 7이 먼저 나와야 함)
        a = 7
    second()

first()
```

### 함수 응용
- map: 순회 가능한 데이터구조(iterable)의 모든 요소에 함수 적용해 map object로 반환

```python
numbers = [1, 2, 3]
result = map(str, numbers)
print(result) # <map object at 0x000...>
print(list(result)) # ['1', '2', '3']

# map의 응용
original = input() # 1 2 3 입력
print(original) # '1 2 3'
str_list = original.split()
print(str_list) # ['1', '2', '3']
map_object = map(int, str_list)
print(map_object) # <map object at 0x000...>
list_flavour = list(map_object)
print(list_flavour) # [1, 2, 3]

nums = list(map(int, input().split())) # 다 합친거
print(nums) # [1, 2, 3]
```

- filter: 순회 가능한 데이터구조(iterable)의 모든 요소에 함수 적용하고, 그 결과가 True인 것들을 filter object로 반환

```python
def ood(n):
    return n % 2
numbers = [1, 2, 3]
result = filter(odd, numbers)
print(result) # <filter object at 0x000...>
print(list(result)) # [1, 3]
```

- zip: 복수의 iterable을 모아 튜플을 원소로 하는 zip object를 반환

```python
girls = ['jane', 'ashley']
boys = ['justin', 'eric']
pair = zit(girls, boys)
print(pair) # <zip object at 0x000...>
print(list(pair)) # [('jane', 'justin'), ('ashley', 'eric')]

# 전치코드시 zip 활용
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
transposed_matrix = list(zip(*a))
print(transposed_matrix) # [(1, 4, 7), (2, 5, 8), (3, 6, 9)]

list_transposed_matrix = list(map(list, zip(*a)))
print(list_transposed_matrix) # [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
```

- lambda 함수: 표현식을 계산한 결과값을 반환하는 함수, 익명함수라고도 불림

```python
# 함수이름 = lambda parameter : 표현식
triangle_area = lambda b, h : 0.5 * b * h
print(triangle_area(5, 6)) # 15.0
```

- 재귀 함수(recursive function): 자기 자신을 호출하는 함수

```python
# 점화식 등에 사용
# 변수 사용을 줄여줄 수 있음
def factorial(n):
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n-1)
print(factorial(4)) # 24
```

## 4. 모듈
- 모듈: 다양한 기능을 하나의 파일로
- 패키지: 다양한 파일을 하나의 폴더로
- 라이브러리: 다양한 패키지를 하나의 묶음으로
- pip: 이것을 관리하는 관리자
- 가상환경: 패키지의 활용 공간

### 모듈과 패키지
- 모듈: 특정 기능을 하는 코드를 파이썬 파일 단위로 작성한 것
- 패키지: 특정 기능과 관련된 여러 모듈의 집합
```python
# 모듈과 패키지 불러오기
import module # 모듈 불러오기
from module import var, function, class # 모듈 내 변수, 함수, 클래스 불러오기
from module import * # 모듈로부터 모두 불러오기

# import module vs from module import *
# import만 사용함현 모듈 안의 함수를 사용할 때, 모듈명.함수명()으로 사용
# from을 사용하면 바로 함수명()으로 사용

from package import module # 패키지 내 모듈 불러오기
from package.module import var, function, class # 패키지 내 특정 모듈 내 변수, 함수, 클래스 불러오기
```

### 파이썬 표준 라이브러리
- 파이썬에 기본적으로 설치된 모듈과 내장 함수
  - [파이썬 표준 라이브러리 보는 곳](https://docs.python.org/ko/3/library/index.html)
- 파이썬 패키지 관리자(pip)
  - PyPI(Python Package Index)에 저장된 외부 패키지들을 설치하도록 도와주는 패키지 관리 시스템

```python
$ pip install SomePackage # 최신 버전 패키지 설치
$ pip install SomePackage==1.0.5 # 특정 버전 패키지 설치
$ pip install SomePackage>=1.0.4 # 최소 버전 패키지 설치
$ pip uninstall SomePackage # 패키지 삭제
$ pip list # 패키지 목록
$ pip show SomePackage # 특정 패키지 정보
$ pip freeze > requirements.txt # 패키지 목록 저장
$ pip install -r requirements.txt # 저장한 패키지 목록 설치
```

### 사용자 모듈과 패키지
- 패키지
  - 패키지는 여러 모듈/하위 패키지로 구조화
  - 모든 폴더에는 __init__.py를 만들어 패키지로 인식

```python
# 모듈 만들기
# calculator/tools.py
def add(num1, num2):
    return num1 + num2

# 모듈 활용하기
from calculator import tools
print(dir(tools)) # tools에 어떤 변수와 메소드를 가지고 있는지 나열
'''
['__builtins__', '__cached__', ... , 'add']
'''
print(tools.add(3, 5)) # 8
```

### 가상환경
- 가상환경
  - 파이썬 표준 라이브러리가 아닌 경우 모두 pip를 통해 설치해야함
  - 각 프로젝트마다 버전이 상이할 수 있음
  - 이러한 경우 가상환경을 만들어 프로젝트별로 독립적인 패키지를 관리할 수 있음

```python
# 가상환경 생성
$ python -m venv venv(폴더명)

# 가상환경 활성화/비활성화
$ source venv/Scripts/activate
$ source venv/Scripts/deactivate


# 예시
# A 팀원이 numpy 패키지 설치 후, B 팀원에게 패키지 파일 복사

# A 팀원 가상환경 생성 및 활성화
# A 팀원 폴더
$ python -m venv venv
$ source venv/Scripts/activate

# A 팀원이 numpy 패키지 설치 후, 패키지 목록을 텍스트 파일에 저장
# A 팀원 폴더
$ pip install numpy
$ pip freeze > requirements.txt
# B 팀원에게 텍스트 파일 전송

# B 팀원 가상환경 생성 및 활성화
# B 팀원 폴더
$ python -m venv venv
$ source venv/Scripts/activate

# B 팀원이 A 팀원에게 받은 패키지 설치
# B 팀원 폴더
# A 팀원에게 받은 텍스트 파일 붙여넣기
$ pip install -r requirements.txt
$ pip list # numpy 패키지 설치 확인
```

