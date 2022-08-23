# 문자열(String)

## 1. 고지식한 알고리즘(Brute Force)

### 고지식한 알고리즘이란?
- 본문 문자열을 처음부터 끝까지 차례대로 순회하면서, 패턴 내의 문자들을 일일히 비교

### 알고리즘 수행 과정
<img src="https://user-images.githubusercontent.com/109272360/184523419-7e0b0a2d-e284-4adb-b6ad-fd6fbb8e7755.png" width="400px" style="margin-top: 20px;">
<img src="https://user-images.githubusercontent.com/109272360/184523421-ee39bbc5-a3e4-4d05-bf26-bea47e6c16fe.png" width="400px" style="margin-top: 20px;">
<img src="https://user-images.githubusercontent.com/109272360/184523422-3565c436-2bf7-4702-8a8e-3a380d874b7c.png" width="400px" style="margin-top: 20px;">
<img src="https://user-images.githubusercontent.com/109272360/184523423-86991b55-a7f3-4e66-9f1c-82a0618acc88.png" width="400px" style="margin-top: 20px;">
<img src="https://user-images.githubusercontent.com/109272360/184523424-f7cf19dd-1dae-44b8-9a64-98b53f1a28bb.png" width="400px" style="margin-top: 20px;">
<img src="https://user-images.githubusercontent.com/109272360/184523425-9edf061d-3802-4d0f-8a0d-efcd1349a805.png" width="400px" style="margin-top: 20px;">
<img src="https://user-images.githubusercontent.com/109272360/184523426-7cfda208-991d-4c7c-9547-ba005aeec99a.png" width="700px" style="margin-top: 20px;">

### 시간복잡도
- N = 전체 텍스트 길이, M = 찾을 패턴의 길이
- 최악의 경우 텍스트 모든 위치에서 패턴 비교: O(MN)

### 코드
```python
p = 'bbc'
t = 'abbbcd'
M = len(p)
N = len(t)

def BruteForce(p, t):
    for i in range(N):
        for j in range(M):
            if text[i] == pattern[j]:
                i += 1
            else:
                break
        else:
            print('find!')
            break
    else:
        print('none')
```

## 2. 보이어-무어 알고리즘

### 보이어-무어 알고리즘이란?
- 오른쪽에서 왼쪽으로 비교
- 최대 패턴의 길이만큼 이동 가능
- 대부분의 상용 소프트웨어에서 채택

### 알고리즘 수행 과정
- 건너뛰는 경우를 저장하는 배열을 만들기
  - 비교 문자열이 건너뛰는 거리는 `length - index - 1` 중 0을 제외한 최솟값
  - 비교 문자열 외 다른 문자는 모두 `length`만큼 건너뛰기
- 비교 문자열의 오른쪽부터 비교 후, 불일치 시 해당 문자만큼 건너뛰기

<img src="https://user-images.githubusercontent.com/109272360/184591516-4a80e666-b23d-44ca-a37b-766ddc43e641.png" width = "500px" style = "margin-top:20px;">
<img src="https://user-images.githubusercontent.com/109272360/184591517-aaddf141-581e-4b8f-897c-65b187e069c2.png" width = "900px" style = "margin-top:20px;">
<img src="https://user-images.githubusercontent.com/109272360/184591519-9743c5c4-feca-44d9-a230-826c0aca4a7c.png" width = "900px" style = "margin-top:20px;">
<img src="https://user-images.githubusercontent.com/109272360/184591521-a783e930-7a7f-47da-9387-5d89262bf23d.png" width = "900px" style = "margin-top:20px;">
<img src="https://user-images.githubusercontent.com/109272360/184591523-e1a6c880-25f8-421c-ae25-0d748c8d9361.png" width = "900px" style = "margin-top:20px;">
<img src="https://user-images.githubusercontent.com/109272360/184591524-0f4e0a61-2cd6-4cfe-b285-3148399b00fa.png" width = "900px" style = "margin-top:20px;">

### 시간복잡도
- 텍스트를 다 보지 않아도 확인 가능
- N = 전체 텍스트 길이, M = 찾을 패턴의 길이
- 평균: O(N)보다 대체로 적은 수준
- 최악의 경우: O(MN)

### 코드
```python
p = 'cdeee'
t = 'acceeddeecdeee'
M = len(p)
N = len(t)

def skip(p, char):
    for i in range(M-1, -1, -1):
        if p[i] == char:
            if i == M - 1:
                continue
            return M - i - 1
    if char in p:
        return 1
    return M

def BoyerMoore(p, t):
    i = M - 1
    j = M - 1

    while i < N:
        k = i + 0
        move = 0

        for _ in range(M):
            if t[i] != p[j]:
                move = skip(p, t[k])
                break
            i -= 1
            j -= 1

        if j == -1:
            break

        i = k + move
        j = M - 1

    if i >= N:
        return -1
    else:
        return i + 1
```







