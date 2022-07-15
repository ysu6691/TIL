# Git
## 1. Git 초기 설정
### 작성자 설정 / 수정
```python
$ git config --global user.name "이름"
$ git config --global user.email "이메일"
```

### 작성자 설정 확인
```python
$ git config --global --list
```



## 2. Git 기본
### git 폴더 설정
```python
$ git init
```

### 파일의 현재 상태
```python
$ git status
```

### add
```python
# 현재 디렉토리에 속한 파일/폴더 전부
$ git add .

# 특정 파일
$ git add a.txt

# 특정 폴더
$ git add a 
```

### commit
```python
$ git commit -m "메시지"
```

### log
```python
# 커밋 내용 조회
$ git log

# 커밋 내용 한 줄로 축약해서 조회
$ git log --oneline
```



## 3. Github 연결
### remote
```python
# 원격 저장소 등록
$ git remote add 저장소이름 주소

# 원격 저장소 조회
$ git remote -v

# 원격 저장소 연결 삭제
$ git remote remove 저장소이름
```

## push
```python
# 로컬 저장소에서 원격 저장소로 이동
$ git push 저장소이름 브랜치이름
```

## clone
```python
# 원격 저장소의 커밋 내역을 가져와서, 로컬 저장소를 생성 (최초 1번)
$ git clone 저장소주소

# 원하는 이름으로 생성
$ git clone 저장소주소 원하는이름
```

## pull
```python
#원격 저장소의 변경 사항을 가져와서, 로컬 저장소를 업데이트
$ git pull 저장소이름 브랜치이름
```

## collision
- 로컬 커밋 기록과 원격 커밋 기록이 상충하여 발생하는 현상
- pull 후에 수정+저장+add+commit 하여 push

