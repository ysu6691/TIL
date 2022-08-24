# 순열과 조합

## 1. 순열(Permutation)

### 순열의 구현
```python
def perm(depth):
    if depth == N:
        result.append(selection[:])
        return

    for i in range(N):
        if not check[i]:
            check[i] = 1
            selection[depth] = arr[i]
            perm(depth+1)
            check[i] = 0

arr = [1, 2, 3]
N = 3
check = [0] * N
selection = [0] * N
result = []

print(result)
'''
[[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]
'''
```

### nPr의 구현
```python
def perm(depth):
    if depth == r:
        result.append(selection[:])
        return

    for i in range(n):
        if not check[i]:
            check[i] = 1
            selection[depth] = arr[i]
            perm(depth+1)
            check[i] = 0

arr = [1, 2, 3]
n = 3
r = 2
check = [0] * n
selection = [0] * r
result = []
perm(0)

print(result)
'''
[[1, 2], [1, 3], [2, 1], [2, 3], [3, 1], [3, 2]]
'''
```

### 중복 순열의 구현
```python
def perm(depth):
    if depth == N:
        result.append(selection[:])
        return

    for i in range(N):
        selection[depth] = arr[i]
        perm(depth+1)

arr = [1, 2]
N = 2
check = [0] * N
selection = [0] * N
result = []
perm(0)

print(result)
'''
[[1, 1], [1, 2], [2, 1], [2, 2]]
'''
```

### nπr의 구현
```python
def perm(depth):
    if depth == r:
        result.append(selection[:])
        return

    for i in range(n):
        selection[depth] = arr[i]
        perm(depth+1)

arr = [1, 2, 3]
n = 3
r = 2
check = [0] * n
selection = [0] * r
result = []
perm(0)

print(result)
'''
[[1, 1], [1, 2], [1, 3], [2, 1], [2, 2], [2, 3], [3, 1], [3, 2], [3, 3]]
'''
```

## 2. 조합

### 조합(nCr)의 구현
```python
def comb(idx, sidx):
    if sidx == r:
        result.append(selection[:])
        return

    if idx == n:
        return

    selection[sidx] = arr[idx]
    comb(idx+1, sidx+1)
    comb(idx+1, sidx)

arr = [1, 2, 3, 4]
n = 4
r = 2
check = [0] * n
selection = [0] * r
result = []
comb(0, 0)

print(result)
'''
[[1, 2], [1, 3], [1, 4], [2, 3], [2, 4], [3, 4]]
'''
```

### 중복 조합(nHr)의 구현
```python
def comb(idx, sidx):
    if sidx == r:
        result.append(selection[:])
        return

    if idx == n:
        return

    selection[sidx] = arr[idx]
    comb(idx, sidx+1)
    comb(idx+1, sidx)

arr = [1, 2, 3, 4]
n = 4
r = 2
check = [0] * n
selection = [0] * r
result = []
comb(0, 0)

print(result)
'''
[[1, 1], [1, 2], [1, 3], [1, 4], [2, 2], [2, 3], [2, 4], [3, 3], [3, 4], [4, 4]]
'''
```

## 3. 부분집합
```python
def subset(idx):
    if idx == N:
        tmp = []
        for i in range(N):
            if check[i]:
                tmp.append(arr[i])
        result.append(tmp)
        return

    check[idx] = 0
    subset(idx + 1)
    check[idx] = 1
    subset(idx + 1)

arr = [1, 2, 3]
N = 3
check = [0] * N
result = []
subset(0)

print(result)
'''
[[], [3], [2], [2, 3], [1], [1, 3], [1, 2], [1, 2, 3]]
'''
```
