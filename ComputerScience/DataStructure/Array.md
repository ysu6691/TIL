# 배열(Array)

## 2차원 배열

### 2차원 배열의 선언
  <img src="https://user-images.githubusercontent.com/109272360/183786361-a53433b8-16b4-477e-a955-3cd9101e0aa7.png" width="300px" style="margin-top:20px; margin-bottom:20px">

  ```python
  # 크기가 NxM 이고 0으로 초기화된 2차원 배열
  arr = [[0] * M for _ in range(N)]

  # 공백 있는 input일 때
  '''
  3
  1 2 3
  4 5 6
  7 8 9
  '''
  N = int(input())
  arr = [list(map(int, input().split())) for _ in range(N)]

  # 공백 없는 input일 때
  '''
  3
  123
  456
  789
  '''
  N = int(input())
  arr = [list(map(int, input())) for _ in range(N)]
  ```

### 2차원 배열의 접근
- 행 우선 순회
  <img src="https://user-images.githubusercontent.com/109272360/183786364-4ebef0af-3d15-43e4-b719-7dcbb0c93f88.png" width="230px" style="margin-top:20px; margin-bottom:20px">

  ```python
  for i in range(n):
      for j in range(m):
          Array[i][j]  # 필요한 연산 수행
  ```

- 열 우선 순회
  <img src="https://user-images.githubusercontent.com/109272360/183786365-2e7c51d5-32c7-4b65-849b-c6f180c1f406.png" width="230px" style="margin-top:20px; margin-bottom:20px">
  ```python
  for j in range(m):
      for i in range(n):
          Array[i][j] # 필요한 연산 수행
  ```
- 지그재그 순회
  <img src="https://user-images.githubusercontent.com/109272360/183786367-e4923709-4fd3-4980-97b5-3479c4655844.png" width="230px" style="margin-top:20px; margin-bottom:20px">
  ```python
  for i in range(n):
      for j in range(m):
          Array[i][j + (m-1-2*j) * (i%2)]
          # 필요한 연산 수행
  ```
- 델타를 이용한 2차 배열 탐색
  - 2차 배열의 한 좌표에서 4방향의 인접 배열 요소를 탐색하는 방법
  ```python
  # N x N 배열
  # 상하좌우 한 칸씩 탐색
  di[] = [0, 0, -1, 1] # 좌우
  dj[] = [-1, 1, 0, 0] # 상하
  
  for i in range(N):
      for j in range(N):
          for k in range(4):
              ni = i + di[k]
              nj = j + dj[k]
              if 0 <= ni < N and 0 <= nj < N:
                  Array[ni][nj] # 필요한 연산 수행
                  
  # 상하좌우 d칸 만큼 탐색
  di[] = [0, 0, -1, 1] # 좌우
  dj[] = [-1, 1, 0, 0] # 상하
  
  for i in range(N):
      for j in range(N):
          for k in range(4):
            for d in range(1, 3):
                ni = i + di[k] * d
                nj = j + dj[k] * d
                if 0 <= ni < N and 0 <= nj < N:
                    Array[ni][nj] # 필요한 연산 수행
  ```
- 전치 행렬
  ```python
  for i in range(N):
      for j in range(N):
          if i < j:
              Array[i][j], Array[j][i] = Array[j][i], Array[i][j]
  ```











