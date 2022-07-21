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


