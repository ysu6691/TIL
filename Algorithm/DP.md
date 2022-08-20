# 동적 프로그래밍 (Dynamic Programming)

### 동적 프로그래밍이란?
- 하나의 큰 문제를 여러 개의 작은 문제로 나누어서 그 결과를 해결하는 것
- 작은 문제에서의 결과를 저장해 큰 문제를 해결하는 알고리즘

### DP의 사용 조건
- 겹치는 부분 문제 (Overlapping Subproblems)
  - 동일한 작은 문제들이 반복하여 나타나는 경우

- 최적 부분 구조 (Optimal Substructure) 
  - 부분 문제의 최적 결과값을 사용해, 전체 문제의 최적 결과를 낼 수 있는 경우

### DP의 사용
1. DP로 풀 수 있는 문제인지 확인
2. 문제의 변수 파악
3. 변수 간 관계식 만들기(점화식)
4. 기저 상태 파악
5. 구현하기
  - Bottom-up 방식: 반복문 사용
  - Top-Down 방식(Memoization) - 재귀 사용

### Memoization
- 입력값에 대한 결과를 저장함으로써, 같은 입력값에 대해 함수가 한 번만 실행되도록 함

<img src="https://user-images.githubusercontent.com/109272360/185610180-d4a0d1ab-b2f2-4563-9536-05ec950e30c4.png" width="750px" style="margin-top:16px;">
<img src="https://user-images.githubusercontent.com/109272360/185610177-907f0727-bc6b-4999-8299-c7693f5186c2.png" width="750px" style="margin-top:16px;">

### 코드
ex) 피보나치 수열

```python
# 재귀 함수
def fibo(n):
    if n < 2:
        return n
    
    return fibo(n-1) + fibo(n-2)
```

```python
# memoization
def fibo(n):
    if n >= 2 and n >= len(memo):
        memo.append(fibo(n-1) + fibo(n-2))
    return memo[n]

memo = [0, 1]
```

```python
# Bottom up
def fibo(n):
    f = [0, 1]

    for i in range(2, n+1):
        f.append(f[i-1 + f[i-2]])

    return f[n]
```

