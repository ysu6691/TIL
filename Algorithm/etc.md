# 알고리즘 기타

## 1. 비트 연산자

### 비트 연산자 종류
- `&`: 비트 단위로 AND 연산
- `|`: 비트 단위로 OR 연산
- `<<`: 피연산자의 비트 열을 왼쪽으로 이동
- `>>`: 피연산자의 비트 열을 오른쪽으로 이동

```python
# & 연산은 각 비트를 and 연산한 뒤 결과 출력
print(bin(10)) # 0b1010
print(bin(25)) # 0b11001
print(bin(4)) # 0b100

print(10&25) # 8
print(10&4) # 0

# << 와 >>
print(bin(3)) # 0b11
print(bin(3<<2)) # 0b1100 (3을 왼쪽으로 두 칸 이동)
print(3<<2) # 12

print(bin(27)) # 0b11011
print(bin(27>>2)) # 0b110 (27을 오른쪽으로 두 칸 이동)
print(27>>2) # 6

# 1<<n = 2^n
print(1<<3) # 8
print(1<<5) # 32

# i & (1<<j): i의 j번째 비트가 1인지 검사
print(bin(10)) # 0b1010
print(bin(1<<3)) # 0b100
print(10 & (1<<3)) # 0

print(bin(10)) # 0b1010
print(bin(1<<4)) # 0b1000
print(10 & (1<<4)) # 8

if 10 & (1<<4):
    print('10의 네 번째 비트는 1이다.')
else:
    print('10의 네 번째 비트는 1이 아니다.')
```

### 비트 연산자로 부분집합 생성
- 길이가 n인 리스트의 부분집합을 생성
- 부분집합의 총 개수는 `n^2`개 [`1<<n`과 동일]
- `i`가 `n^2`을 돌 때마다, 부분집합 하나씩 생성

```python
arr = [3,1,2]

n = len(arr) # n = 3
                        
for i in range(1<<n):   
    for j in range(n): 
        if i & (1<<j):  
            print(arr[j], end=", ")
    print()
print()
```

<img src="https://user-images.githubusercontent.com/109272360/183824392-618ca730-ecb3-4f7f-8c78-cc7dda673be8.png" width="1000px" style="margin-top:20px;">

<br>
