# 완전 탐색(Exhaustive Search)

## 1. DFS & BFS

### DFS
- DFS(Depth First Search): 깊이 우선 탐색
- `스택(stack)`을 활용해 구현
<br>
<img src="https://user-images.githubusercontent.com/109272360/184883260-45d53a9e-7451-4f58-8e68-57482d781a8c.png" width="400px" style="margin-top:10px;">

### DFS 구현
```python
# 스택 이용
# V: Vertex 개수, E: Edge 개수
V, E = map(int, input().split())

# 인접 행렬 생성
adj_matrix = [[0]*(V+1) for _ in range(V+1)]

for _ in range(E):
    # 연결 정보 저장
    start, end = map(int, input().split())
    adj_matrix[start][end] = 1
    adj_matrix[end][start] = 1 # 양방향 허용

stack = [1]
visited = []

while stack:
    current = stack.pop()
    if current not in visited:
        visited.append(current)

    for destination in range(V+1):
        if adj_matrix[current][destination] and destination not in visited:
            stack.append(destination)
```

```python
# 재귀 함수 이용
# 12x12 이상의 행렬에서 사용 시 에러 발생 가능
def dfs(n):
    if n not in visited:
        visited.append(n)

    for destination in range(V+1):
        if adj_matrix[n][destination] and destination not in visited:
            dfs(destination)

V, E = map(int, input().split())

adj_matrix = [[0]*(V+1) for _ in range(V+1)]

for _ in range(E):
    start, end = map(int, input().split())
    adj_matrix[start][end] = 1
    adj_matrix[end][start] = 1

visited = []

dfs(1) # 스택 이용할 때와 결과 다르게 나오는 점 유의!
```

### BFS
- BFS(Breadth First Search): 너비 우선 탐색
- `큐(Queue)`를 이용해 구현
<br>
<img src="https://user-images.githubusercontent.com/109272360/184883267-03226e04-ec31-479f-8ea2-dd68fd1953c1.png" width="400px" style="margin-top:10px;">


### BFS 구현
```python
V, E = map(int, input().split())

# 인접 행렬 생성
adj_matrix = [[0]*(V+1) for _ in range(V+1)]

for _ in range(E):
    # 연결 정보 저장
    start, end = map(int, input().split())
    adj_matrix[start][end] = 1
    adj_matrix[end][start] = 1 # 양방향 허용

queue = [1]
visited = []

while queue:
    current = queue.pop(0)
    if current not in visited:
        visited.append(current)

    for destination in range(V+1):
        if adj_matrix[current][destination] and destination not in visited:
            queue.append(destination)
```


