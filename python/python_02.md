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
print(resule) 

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








