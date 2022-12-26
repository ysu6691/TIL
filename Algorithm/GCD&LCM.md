# GCD & LCM

## 1. GCD(Greatest Common Divisor): 최대공약수

### GCD 구현

최대공약수를 구현하기 위해서는 유클리드 호제법을 이용하는 것이 가장 효율적이다.

> 유클리드 호제법
>
> 숫자`a`, `b`의 최대공약수는 `a를 b로 나눈 나머지`와 `b`의 최대공약수와 같다.

즉 `b`를 `a`로, `a를 b로 나눈 나머지`를 `b`로 대체하면서 0이 될 때까지 반복하면 최대공약수를 구할 수 있다.

```python
# 최대공약수
def gcd(a, b):
    while b > 0:
        a, b = b, a % b
    return a
```

`a`, `b` 중 더 큰 값이 어떤 것인지는 상관이 없다.

만약 `b`가 더 큰 값이여도 `a % b = a`이므로 `a`와 `b`가 교체되기 때문이다.

### 여러 수의 GCD 구하기

수가 여러 개가 있는 경우에는 최대공약수를 갱신하면서 구해주면 된다.

숫자 `a`, `b`, `c`가 있을 때 최대공약수는 `a와 b의 최대공약수`와 `c`의 최대공약수와 같다.

따라서 `gcd(gcd(a, b), c)`로 구할 수 있다.

```python
# 최대공약수
def gcd(a, b):
    while b > 0:
        a, b = b, a % b
    return a

# 여러 수의 최대공약수
def solution(nums):
    current_gcd = 0
    for num in nums:
        current_gcd = gcd(current_gcd, num)
    return current_gcd
```

## 2. LCM(Least Common Multiple): 최소공배수

### LCM 구현

숫자 `a`, `b`의 최소공배수는 `a * b / a와 b의 최대공약수`로 구할 수 있다.

```python
# 일단 최대공약수가 필요하다.
def gcd(a, b):
    while b > 0:
        a, b = b, a % b
    return a

# 최대공약수를 이용해 최소공배수 구현
def lcm(a, b):
    return a * b // gcd(a, b)
```

### 여러 수의 LCM 구하기

최소공배수 또한 여러 수의 최대공약수를 구하는 것과 동일하다.

숫자 `a`, `b`, `c`가 있을 때 최소공배수는 `a와 b의 최소공배수`와 `c`의 최소공배수와 같다.

따라서 `lcm(lcm(a, b), c)`로 구할 수 있다.

```python
# 최대공약수
def gcd(a, b):
    while b > 0:
        a, b = b, a % b
    return a

# 최소공배수
def lcm(a, b):
    return a * b // gcd(a, b)

# 여러 수의 최소공배수
def solution(nums):
    current_lcm = nums[0]
    for num in nums:
        current_lcm = lcm(current_lcm, num)
    return current_lcm
```