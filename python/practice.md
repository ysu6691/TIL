# Python Practice

## 1. 문자열
```python
s = 'abc'
print(s.find('a'))
print(s.find('z'))
print(s.index('a'))
print(s.index('z'))
print(s.isalpha())
print('123'.isalpha())
print('Ab'.isupper())
print('ab'.islower())
print('Title title'.istitle())

print('aaabc'.replace('a', 'z'))
print('aaabc'.replace('a', 'z', 2))
s = 'hI! Everyone'
print(s.capitalize())
print(s.title())
print(s.upper())
print(s.lower())
print(s.swapcase())
```


## 2. 리스트
```python
a = ['a', 'b', 'c']
a.insert(0, 1)
a.insert(100, 10)

a = ['a', 'b', 'c']
a.extend(['d', 'e'])
a.extend('xyz')
a.extend(123)
a += ['end']

a = ['a', 'b', 'c']
a.remove('a')
a.remove()
a.remove('z')

a = ['a', 'b', 'c']
a.pop(1)
a.pop()
a.pop(100)

a = ['a', 'b', 'c']
a.index(2)
a.index(100)

a = ['a', 'b', 'c']
a.count('a')
a.count('z')

a = [1, 4, 2, 3]
result = a.reverse()
print(result, a)

a = [1, 4, 2, 3]
result = a.sort()
print(result, a)

a = [1, 4, 2, 3]
result = sorted(a)
print(result, a)
```


## 3. 튜플
```python
a = (1, 4, 3, 2)
print(a.index(2))
print(a.count(2))

a.sort()
b = sorted(a)
print(b)

a = (1, 4, 3, 2)
a.extend([5])
a += 5
a += (6, 7)
```

- 시퀀스형 연산자
```python
a = [1, 2]
print(id(a))
a *= 3
print(id(a))

a = (1, 2)
print(id(a))
a *= 3
print(id(a))
```


## 4. 셋
```python
a = {1, 2, 3}
a.add(4)
a.add('a')
a.add([4, 5])

a = {1, 2, 3}
a.update(4)
a.update('a')
a.update([4, 5])

a = {1, 2, 3}
a.remove(1)
a.remove(4)

a = {1, 2, 3}
a.discard(1)
a.discard(4)

a = {1, 2, 3}
a.pop()
a.pop(1)
```


## 5. 딕셔너리
```python
a = {'apple': '사과', 'banana': '바나나'}
b = a.pop()
print(a, b)

b = a.pop('apple')
print(a, b)

b = a.pop('바나나')
print(a, b)

b = a.pop('pineapple', 0)
print(a, b)

a = {'apple': '사과', 'banana': '바나나'}
a.update(apple = '풋사과')
```


## 6. range와 슬라이싱
```python
a = list(range(5, 1, -1))
print(a)

a = [1, 2, 3, 4, 5]
print(a[-1:-3])
print(a[-1:-3:-1])
print(a[-1:0:-1])
print(a[-2::-1])
```


