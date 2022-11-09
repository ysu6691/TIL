# Git
## 1. Git 초기 설정
### 작성자 설정 / 수정
```python
$ git config --global user.name "이름"
$ git config --global user.email "이메일"
```

### 작성자 설정 확인
```bash
$ git config --global --list
```



## 2. Git 기본
### git 폴더 설정
```bash
$ git init
```

### 파일의 현재 상태
```bash
$ git status
```

### add
```bash
# 현재 디렉토리에 속한 파일/폴더 전부
$ git add .

# 특정 파일
$ git add a.txt

# 특정 폴더
$ git add a 

# add 삭제
$ git reset HEAD 파일/폴더 # 특정 파일/폴더 add 삭제
$ git reset HEAD # 모든 파일/폴더 add 삭제
```

### commit
```bash
# commit
$ git commit -m "메시지"

# 마지막 1개의 commit 취소
$ git reset HEAD^

# 마지막 n개의 commit 취소
$ git reset HEAD~n

# commit message 변경
$ git commit --amend
```

### log
```bash
# 커밋 내용 조회
$ git log

# 커밋 내용 한 줄로 축약해서 조회
$ git log --oneline
```



## 3. Github 연결
### remote
```bash
# 원격 저장소 등록
$ git remote add 저장소이름 주소

# 원격 저장소 조회
$ git remote -v

# 원격 저장소 연결 삭제
$ git remote remove 저장소이름
```

### push
```bash
# 로컬 저장소에서 원격 저장소로 이동
$ git push 저장소이름 브랜치이름
```

### clone
```bash
# 원격 저장소의 커밋 내역을 가져와서, 로컬 저장소를 생성 (최초 1번)
$ git clone 저장소주소

# 원하는 이름으로 생성
$ git clone 저장소주소 원하는이름
```

### pull
```bash
# 원격 저장소의 변경 사항을 가져와서, 로컬 저장소를 업데이트
$ git pull 저장소이름 브랜치이름
```

### collision
```bash
# 로컬 커밋 기록과 원격 커밋 기록이 상충하여 발생하는 현상
# pull 후에 수정+저장+add+commit 하여 push
```

## 4. Example
```bash
# Day1 : 집(HOME)에서 깃허브(hub)로 push
# Day2 : 회사(company)에서 clone 생성 후 수정해서 깃허브로 push
# Day3 : 집에서 push 했던 기존 파일 수정한 뒤, git에서 수정된 파일 pull (collision) -> 해결 후 깃허브로 push

---

# Day1. 집에서 깃허브로 push

$ git init
$ git add.
$ git commit -m 'Day1 : 집에서 작성'
$ git remote add origin 'https://github.com/ysu6691'
$ git push origin master

---

# Day2. 회사(company)에서 pull & 수정 후 깃허브로 push

$ git clone 'https://github.com/ysu6691'

$ git add.
$ git commit -m 'Day2 : 회사에서 수정'
$ git push origin master

---

# Day3. 집에서 push 했던 기존 파일 수정한 뒤, git에서 수정된 파일 pull (collision) -> 해결 후 깃허브로 push

$ git add.
$ git commit -m 'Day3 : 집에서 재수정'
$ git pull origin master

# collision 발생
# 팀원과 상의 후 재수정

$ git add.
$ git commit -m 'Day3 : collision 해결'
$ git push origin master
```
