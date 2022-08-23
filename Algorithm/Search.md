# 검색(Search)

## 1. 검색

### 검색이란?
- 저장되어 있는 자료 중에서 원하는 항목을 찾는 작업
- 목적하는 탐색 키를 가진 항목을 찾는 것
  - 탐색 키(search key): 자료를 구별하여 인식할 수 있는 키

### 검색의 종류
- 순차 검색(sequential search)
- 이진 검색(binary search)
- 해쉬(hash)
- 선형 검색(linear search)

## 2. 순차 검색

### 순차검색이란?
- 일렬로 되어 있는 자료를 순서대로 검색하는 방법
- 가장 간단하고 직관적
- 2가지 경우 존재
  - 정렬되어 있지 않은 경우
  - 정렬되어 있는 경우

### 정렬되어 있지 않은 경우
- 검색 과정
  - 첫 번째 원소부터 순서대로 검색 대상과 키 값이 같은 원소가 있는지 비교
  - 키 값이 동일한 원소를 찾으면 그 원소의 인덱스를 반환
  - 자료구조의 마지막에 이를 때까지 검색 대상을 찾지 못하면 검색 실패

  <img src="https://user-images.githubusercontent.com/109272360/183797306-4327b76d-2682-4380-9b05-657c2771da24.png" width="500px" style="margin-top: 20px; margin-bottom: 20px;">
  <img src="https://user-images.githubusercontent.com/109272360/183797310-8bb5e8ce-a0d3-49e0-b162-799bd361c908.png" width="500px" style="margin-top: 20px; margin-bottom: 20px;">

- 비교회수
  - 찾고자 하는 원소의 순서에 따라 비교회수가 결정
  - 평균 비교 회수: (1/n) * (1+2+3+...+n) = (n+1)/2
  - 시간 복잡되: O(n)

- 코드
```python
def sequentialSearch(a_list, n, key):
    # n: a_list의 길이
    # key: 찾고자 하는 원소 값
    for i in range(N):
        if a_list[i] == key:
            print('find!')
            break
    else:
        print('none')
```


### 정렬되어 있는 경우
- 검색 과정
  - 자료가 오름차순으로 정렬된 상태인 경우, 순차적으로 검색하다가 원소의 값이 키 값보다 크면 찾는 원소가 없다는 것이므로 검색 종료

  <img src="https://user-images.githubusercontent.com/109272360/183797312-9fdc1bb6-529a-4513-a48a-704905838877.png" width="500px" style="margin-top: 20px; margin-bottom: 20px;">
  <img src="https://user-images.githubusercontent.com/109272360/183797316-18a1e043-2dd0-4ed4-bc87-e4fe60f0d5a7.png" width="500px" style="margin-top: 20px; margin-bottom: 20px;">

- 비교 회수
  - 찾고자 하는 원소의 순서에 따라 비교회수가 결정
  - 정렬되어 있으므로, 검색 실패를 반환하는 경우 평균 비교 회수가 반으로 줄어듦
  - 시간 복잡도: O(n)

- 코드
```python
def sequentialSearch(a_list, n, key):
	  # n: a_list의 길이
    # key: 찾고자 하는 원소 값
    for i in range(N):
        if a_list[i] == key:
            print('find!')
            break
        elif a_list[i] > key:
            print('none')
            break
    else:
        print('finish') 
```

## 3. 이진 검색

### 이진 검색이란?
- 자료의 가운데에 있는 항목의 키 값과 비교하여 다음 검색의 위치를 결정하고 검색을 계속 진행하는 방법
- 따라서 자료가 정렬된 상태여야 함

### 검색 과정
- 자료의 중앙에 있는 원소를 골라, 찾고자 하는 목표 값과 비교
- 목표 값이 더 직으면 자료의 왼쪽 반에 대해 새로 검색
- 목표 값이 더 크면 자료의 오른쪽 반에 대해 새로 검색
- 이를 반복

<img src="https://user-images.githubusercontent.com/109272360/183845540-bd0c8ed3-4787-43f1-b721-12a0c0ce1848.png" width="600px" style="margin-top:20px;">
<img src="https://user-images.githubusercontent.com/109272360/183845549-24e259f0-4ace-428d-9e82-33d40db0405d.png" width="600px" style="margin-top:20px;">


### 코드
- 검색 범위의 시작점과 종료점을 이용하여 검색 반복 수행
- 만약 자료에 삽입이나 삭제가 발생하면, 항상 정렬 상태로 유지하는 추가 작업 필요

```python
def binarySearch(a_list, n, key):
	# n: a_list의 길이
    # key: 찾고자 하는 원소 값
	start = 0
	end = n - 1
	while start <= end:
		middle = (start + end) // 2
		if a_list[middle] > key:
			end = middle - 1
		elif a_list[middle] < key:
			start = middle + 1
		else:
			return True
	return False
```
