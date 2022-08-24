# 1. 스택(Stack)

### 1) 스택의 특징

스택은 **자료를 한 쪽 끝에서만 넣고 뺄 수 있는 자료구조**이다. 

즉, 가장 마지막에 삽입된 데이터가 가장 먼저 삭제되는 구조를 가지고 있다. 

따라서 **후입선출(LIFO: Last In First Out) 구조**라고도 한다.

<img src="https://user-images.githubusercontent.com/109272360/186186685-02763fc0-cc2c-4bdb-bb63-a6c786bd8cd0.png" width="350px" style="margin-top:16px;">


### 2) 스택의 연산

- `push` : 스택의 맨 위(뒤)에 데이터를 삽입하는 연산
- `pop` : 스택의 맨 위(뒤)에서 데이터를 꺼내는 연산
- `peek` : 스택의 맨 위 데이터를 보는 연산 (꺼내지 x)
- `size` : 스택에 들어간 데이터의 개수를 확인하는 연산
- `isEmpty` : 스택이 비었는지 확인하는 연산 >비어있으면 True 반환
- `top` : 가장 마지막에 삽입한 데이터를 삭제하지 않고 return

### 3) 스택의 구현

```python
class Stack:
    def __init__(self):
        self.stack = []

    def push(self, data):
        self.stack.append(data)

    def pop(self):
        if self.isEmpty():
            print('스택이 비었습니다.')
        else:
            return self.stack.pop()

    def peek(self):
        if self.isEmpty():
            print('스택이 비었습니다.')
        else:
            return self.stack[-1]

    def isEmpty(self):
        return not bool(self.stack)

    def size(self):
        return len(self.stack)
```

### 4) 스택의 활용

- 웹 브라우저 방문 기록 (뒤로가기)
- 실행 취소 (undo)
- DFS 구현
- 시스템 스택


---

# 2. 큐(Queue)

### 1) 큐의 특징

큐는 **자료를 한 쪽 끝(rear)에서 넣고 다른 한 쪽 끝(front)에서 빼는 자료구조**이다. 

즉, 가장 처음에 삽입된 데이터가 가장 먼저 삭제되는 구조를 가지고 있다. 

따라서 **선입선출(FIFO: First In First Out) 구조**라고도 한다.

<img src="https://user-images.githubusercontent.com/109272360/186186708-288c54a5-cabe-4946-8048-bd862a6fce84.png" width="500px" style="margin-top:16px;">

데이터의 **앞부분을 Front, 뒷부분을 Rear** 라고 부른다.

데이터는 Rear 로 들어와서 Front 로 나간다.

### 2) 큐의 연산

- `enqueue` : 큐의 맨 뒤에 데이터를 삽입하는 연산
- `dequeue` : 큐의 맨 앞에서 데이터를 꺼내는 연산 (큐가 비어있는지 먼저 확인한 후에 실행)
- `peek` : 큐의 맨 뒤 데이터를 보는 연산 (꺼내지 x)
- `size` : 큐에 들어간 데이터의 개수를 확인하는 연산
- `isEmpty` : 큐가 비었는지 확인하는 연산

### 3) 큐의 구현

```python
class Queue:
    def __init__(self):
        self.queue = []

    def enqueue(self, data):
        self.queue.append(data)

    def dequeue(self):
        if self.isEmpty():
            print('큐가 비었습니다.')
        else:
            return self.queue.pop(0)

    def peek(self):
        if self.isEmpty():
            print('큐가 비었습니다.')
        else:
            return self.queue[0]

    def isEmpty(self):
        return not bool(self.queue)

    def size(self):
        return len(self.queue)
```

### 4) 큐의 활용

- 프린터의 인쇄 대기열
- 은행 업무
- 콜센터 고객 대기시간
- BFS 구현
---

# 3. 덱(Deque)

### 1) 덱의 특징

**덱(Double-ended queue)**은 **스택과 큐의 연산을 모두 지원하는 자료구조**이다.

**양쪽에서 모두 삽입과 삭제**가 가능하다.

입력 또는 출력을 한 쪽에서만 가능하도록 제한하는 덱도 있다.

- 입력 제한 덱(scroll): 입력이 한쪽 끝으로만 가능하도록 제한한 덱
- 출력 제한 덱(shelf): 출력이 한쪽 끝으로만 가능하도록 제한한 덱

<img src="https://user-images.githubusercontent.com/109272360/186186749-14243ec1-2e38-4c57-80c4-f6d8215f8e69.png" width="500px" style="margin-top:16px;">

### 2) 덱의 연산

- `append` : 덱의 오른쪽에서 데이터를 삽입
- `appendleft` : 덱의 왼쪽에서 데이터를 삽입
- `pop` : 덱의 오른쪽에서 데이터를 반환 및 제거
- `popleft` : 덱의 왼쪽에서 데이터를 반환 및 제거

### 3) 덱의 구현

```python
class Deque:
    def __init__(self):
        self.deque = []

    def append(self, data):
        self.deque.append(data)

    def appendleft(self, data):
        self.deque.insert(0, data)

    def pop(self):
        if self.isEmpty():
            print('덱이 비었습니다.')
        else:
            return self.deque.pop()

    def popleft(self):
        if self.isEmpty():
            print('덱이 비었습니다.')
        else:
            return self.deque.pop(0)

    def isEmpty(self):
        return not bool(self.deque)
```

```python
# 모듈로 간단하게 활용 가능
from collections import deque

my_deque = deque([1, 2, 3])
```

### 4) 덱의 활용

- 데이터를 앞, 뒤쪽에서 모두 삽입/삭제하는 과정이 필요한 경우
- 덱 모듈을 이용해 데이터를 앞에서 꺼내는 경우, 큐에서 pop(0)을 할 때보다 빠름
  - 덱에서 popleft() 사용 시: O(1) 
  - 큐에서 pop(0) 사용 시: O(N) → 뒤의 모든 데이터를 앞으로 당겨야 하므로