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
```

## 2. 순서가 있는 데이터 구조

### 문자열(String Type)
- 변경 불가능한 immutable
- 작은 따옴표(')나 큰 따옴표(")를 활용하여 표기

```python
s.find(x) # x의 첫 번째 위치를 반환. 없으면, -1을 반환
s.index(x) # x의 첫 번째 위치를 반환. 없으면, 오류 발생
s.isalpha() # 알파벳 문자 여부(단순 알파벳이 아닌 유니코드 상 letter)
s.isupper() # 대문자 여부
s.islower() # 소문자 여부
s.istitle() # 타이틀 형식 여부

s = 'abc'
print(s.find('a')) # 0
print(s.find('d')) # -1
print(s.index('a')) # 0
print(s.index('d')) # ValueError: substring not found
print(s.isalpha()) # True
print('ㄱㄴㄷ'.isalpha()) # True
print('Ab'.isupper()) # False
print('ab'.islower()) # True
print('Title Title!'.istitle()) # True
```