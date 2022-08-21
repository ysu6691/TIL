# 정렬(Sorting)

## 1. 정렬

### 정렬이란?
2개 이상의 자료를 특정 기준에 의해 오른차순(ascending) 혹은 내림차순(descending)으로 재배열하는 것
키: 자료를 정렬하는 기준이 되는 특정 값

### 정렬의 종류
- 버블 정렬(Bubble Sort)
- 카운팅 정렬(Counting Sort)
- 선택 정렬(Selection Sort)
- 퀵 정렬(Quick Sort)
- 삽입 정렬(Insertion Sort)
- 병합 정렬(Merge Sort)

## 2. 버블정렬

### 버블정렬이란?
인접한 두 개의 원소를 비교하며 자리를 계속 교환하는 방식

### 정렬 과정
- 첫 번째 원소부터 인접한 원소끼리 계속 자리를 교환하면서, 맨 마지막 자리까지 이동
- 한 단계가 끝나면 가장 큰 원소가 마지막 자리로 정렬

<div style="display:flex; flex-wrap:wrap; align-items:flex-start; justify-content:space-evenly;" width="850px">
  <img src="https://user-images.githubusercontent.com/109272360/183350036-d866bd47-a95f-4486-93ac-b7b65c9606cf.png" width="300px">
  <img src="https://user-images.githubusercontent.com/109272360/183350045-6d018166-54ef-479c-a353-aa58fa3aeb5c.png" width="300px">
  <img src="https://user-images.githubusercontent.com/109272360/183350052-77890a9d-8c64-46e0-84e3-3c7bdf69d31d.png" width="300px" style="margin-top:16px;">
  <img src="https://user-images.githubusercontent.com/109272360/183350056-70a32781-60a5-43bc-b0bb-3a651cd116b9.png" width="300px" style="margin-top:16px;">
</div>

<br>

### 시간 복잡도
O(n<sup>2</sup>)

### 코드
```python
def bubble_sort(a_list):

    # a_list의 길이: N
    for i in range(N-1, 0, -1):
        for j in range(i):
            if a_list[j] > a_list[j+1]: # 안정 정렬 (>= 일 경우, 불안정 정렬)
                a_list[j], a_list[j+1] = a_list[j+1], a_list[j]

    return a_list
```

## 3. 카운팅 정렬

### 카운팅 정렬이란?
각 항목이 몇 개씩 있는지 확인한 후, 새로운 리스트의 각 인덱스에 추가해 정렬하는 방식

### 제한 사항
- 정수나 정수로 표현할 수 있는 자료에 대해서만 적용 가능
- 집합 내의 가장 큰 정수를 미리 알고 있어야 함

### 정렬 과정
<div style="display:flex; flex-wrap:wrap; align-items:flex-start;" width="850px">
  <img src="https://user-images.githubusercontent.com/109272360/183440043-e1a28c24-4a0f-4d9b-b522-e33d5a4ca88a.png" width="500px">
  <img src="https://user-images.githubusercontent.com/109272360/183440056-69e6357e-7291-4e1e-9c2d-a8d1226667e4.png" width="500px">
  <img src="https://user-images.githubusercontent.com/109272360/183440072-06a4197e-1562-41a4-99be-5056a5fbf62a.png" width="500px" style="margin-top:16px;">
  <img src="https://user-images.githubusercontent.com/109272360/183923343-2c5b7c82-f578-44e5-8efe-03ea97e6e84e.png" width="500px" style="margin-top:16px;">
  <img src="https://user-images.githubusercontent.com/109272360/183440089-b3e3215a-e00e-43a2-84cd-751ef9a4e395.png" width="500px" style="margin-top:16px;">
  <img src="https://user-images.githubusercontent.com/109272360/183440103-71348ae6-5442-4da0-a125-2c269a181c47.png" width="500px" style="margin-top:16px;">
  <img src="https://user-images.githubusercontent.com/109272360/183440335-c7ab8b16-cf8d-4e5a-a503-f618d550de97.png" width="500px" style="margin-top:16px;">
  <img src="https://user-images.githubusercontent.com/109272360/183440340-9dd597e6-33ea-4ee1-8c65-5b2150affaed.png" width="500px" style="margin-top:16px;">
  <img src="https://user-images.githubusercontent.com/109272360/183440346-e58aea0b-578b-40ce-bf5e-50fa3e72b15a.png" width="500px" style="margin-top:16px;">
</div>

<br>

### 시간 복잡도
O(n + K)

### 코드
```python
def counting_sort(a_list):
    # a_list 내의 가장 큰 정수: M
    M = 5
    count = [0] * (M + 1)

    # a_list의 길이: N
    N = 10
    new_list = [0] * N

    for i in range(N):
        count[a_list[i]] += 1

    for i in range(M):
        count[i + 1] += count[i]

    for i in range(N - 1, -1, -1):
        new_list[count[a_list[i]] - 1] = a_list[i]
        count[a_list[i]] -= 1

    return new_list
```

## 4. 선택 정렬

### 선택 정렬이란?
주어진 자료들 중 가장 작은 값의 원소부터 차례대로 맨 앞의 원소와 교환하는 방식

### 제한 사항
- 정수나 정수로 표현할 수 있는 자료에 대해서만 적용 가능
- 집합 내의 가장 큰 정수를 미리 알고 있어야 함

### 정렬 과정
- 주어진 리스트 중에서 최솟값을 찾는다.
- 그 값을 리스트의 맨 앞에 위치한 값과 교환한다.
- 이를 반복

<img src="https://user-images.githubusercontent.com/109272360/183922142-77a68779-9d9c-4c25-84d9-1daecc9e78b4.png" width="400px" style="margin-top:20px;">
<img src="https://user-images.githubusercontent.com/109272360/183922146-25acbf98-6ada-4dec-9ad7-7e34125562e5.png" width="400px" style="margin-top:20px;">
<img src="https://user-images.githubusercontent.com/109272360/183922148-6e9dd286-973e-4ed7-bbe4-f08b3f3d8306.png" width="400px" style="margin-top:20px;">
<img src="https://user-images.githubusercontent.com/109272360/183922152-14781b40-6da1-4699-9cdd-eccf985f4747.png" width="400px" style="margin-top:20px;">

### 시간 복잡도
O(n<sup>2</sup>)

### 코드
```python
def selection_sort(a_list):
    # a_list의 길이: N
    for i in range(N-1):
        minIdx = 0
        for j in range(i, N):
            if minIdx > a_list[j]:
                minIdx = j
      a_list[minIdx], a_list[i] = a_list[i], a_list[minIdx]

    return a_list
```








## ?. 안정 정렬
- 중복된 값을 입력 순서와 동일하게 정렬하는 것
- ex1) [3, 1, 2, 3] -> [1, 2, 3, 3] (안정정렬과 불안정정렬 모두 동일한 결과)
- ex2) [(a,3), (b,1), (c,2), (d,3)] -> [(b,1), (c,2), (a,3), (d,3)] (안정정렬)

  <img src="https://user-images.githubusercontent.com/109272360/183350062-99a2a4ac-ec24-4e53-823a-de6a29520260.png" width="550px">


