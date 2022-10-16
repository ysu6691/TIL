# SQL

## 1. DATABASE

### RDB
- RDB(Relational Database, 관계형 데이터베이스)
  - 데이터를 테이블, 행, 열 등으로 나누어 구조화하는 방식
  - 테이블간 관계를 설정해, 여러 데이터를 쉽게 조작 가능
    - ex) 주문 테이블에서 고객 id를 이용해, 고객 테이블 내 해당 고객 주소로 배송
  - RDB 기본 구조: 스키마, 테이블(필드, 레코드, PK)

- RDBMS(Relational Database Management System)
  - 관계형 데이터베이스를 만들고 관리하는 데 사용되는 프로그램
  - SQLite, MySQL, Oracle 등

### SQLite
- 서버가 아닌 응용 프로그램에 넣어 사용하는 비교적 가벼운 RDBMS
- 구글 안드로이드 및 임베디드 운영체제에 탑재된 데이터베이스
- 로컬에서 간단한 DB 구성을 할 수 있으며, 오픈소스 프로젝트이기 때문에 자유롭게 사용 가능

## 2. SQL 기본

### SQL이란?
- SQL(Structured Query Language)
  - RDBMS의 데이터를 관리하기 위해 설계된 프로그래밍 언어
  - 데이터베이스와 상호작용하는 수단

- SQL Commands
  - DDL(Data Definition Language): 데이터 정의 언어
    - 관계형 데이터베이스 구조(테이블, 스키마)를 정의(생성, 수정, 삭제)하기 위한 명령어
      - 키워드: `CREATE`, `DROP`, `ALTER`
  - DML(Data Manipulation Language): 데이터 조작 언어
    - 데이터를 조작(CRUD: 추가, 조회, 변경, 삭제)하기 위한 명령어
      - 키워드: `INSERT`, `SELECT`, `UPDATE`, `DELETE`
  - DCL(Data Control Language): 데이터 제어 언어
    - 데이터의 보안, 수행제어, 사용자 권한 부여 등을 정의하기 위한 명령어
      - 키워드: `GRANT`, `REVOKE`, `COMMIT`, `ROLLBACK`

- SQL Syntax
  ```SQL
  -- SELECT statement
  SELECT column_name FROM table_name;
  -- clause: SELECT column_name, FROM table_name
  ```
  - 하나의 SQL문(statement)은 **세미콜론(;)** 으로 끝남 (줄바꿈은 상관 x)
    - statement: 독립적으로 실행할 수 있는 완전한 코드 조각
    - clause: statement의 하위 단위
  - SQL 키워드는 대소문자를 구분하지 않으나, 대문자 작성을 권장 (구조 파악 용이)
  - 들여쓰기는 문법 상 고려되지 않으므로, 가독성을 위해 사용
  - 주석: 문장의 맨 앞에 `--` 사용

- SQLite 사용하기
```bash
$ sqlite3 # 시작
sqlite> .open mydb.sqlite3 # 데이터베이스 파일 열기
# 혹은 $ sqlite3 mydb.sqlite3 로 한 번에 데이터베이스 파일 열기 가능
sqlite> .mode csv # 모드를 csv로 설정
sqlite> .import users.csv users # users.csv 데이터를 users 테이블로 가져오기
sqlite> .exit # 종료
```

- SQLite Data Types
  - SQlite는 다른 모든 SQL 데이터베이스 엔진과 달리 동적 타입 시스템을 사용
    - 컬럼에 저장된 값에 따라 데이터 타입이 결정
    - 따라서 테이블을 생성할 때 컬럼에 대해 특정 데이터 타입을 선언하지 않아도 됨
    - 하지만 다른 데이터베이스 엔진과의 호환성 문제가 있기 때문에, **데이터 타입을 지정하는 것을 권장**

  - Data Type 종류
    - `NULL`: 정보가 없거나 알 수 없음을 의미 / 따옴표 없이 NULL만 있는 경우
    - `INTEGER`: 정수 / 따옴표와 소수점 또는 지수가 없는 경우
    - `REAL`: 실수 / 따옴표 없이 소수점 또는 지수가 있는 경우
    - `TEXT`: 문자 데이터 / 따옴표로 묶여있는 경우
    - `BLOB`: 저장된 데이터 덩어리(이미지 등 멀티미디어 파일)
    - 별도의 Boolean type이 없음 -> 0과 1로 저장
    - [Date and Time Functions](https://www.sqlite.org/lang_datefunc.html)

  - 타입 선호도(Type Affinity): 다른 데이터베이스 엔진 간의 호환성을 최대화
    |타입|호환 예시|
    |--|--|
    |INTEGER|INT, INTEGER, TINYINT, BIGINT, BIG INT, INT2 등|
    |TEXT|CHARACTER(20), VARCHAR(255), TEXT, CLOB 등|
    |BLOB|BLOB|
    |REAL|REAL, DOUBLE, FLOAT 등|
    |NUMERIC|NEMERIC, DECIMAL(10,5), BOOLEAN, DATE, DATETIME 등|

- SQLite Constraints
  - 입력하는 자료에 대해 제약을 정함
  - 데이터 무결성: 데이터의 정확성, 일관성을 보장
  - Constraints 종류
    - `NOT NULL`: 컬럼이 NULL 값을 허용하지 않도록 지정 (사용하지 않을 경우, NULL값 허용)
    - `UNIQUE`: 컬럼의 모든 값이 서로 구별되거나 고유한 값이 되도록 함 (NULL값 허용)
    - `PRIMARY KEY`: 테이블에서 행의 고유성을 식별하는 데 사용되는 컬럼, 암시적으로 NOT NULL 제약 조건이 포함되어 있음 (**INTEGER 타입에만 사용 가능**)
    - `AUTOINCREMENT`: 사용되지 않은 값이나 이전에 삭제된 행의 값을 재사용하는 것을 방지, `PRIMARY KEY` 다음에 작성 시, rowid를 재사용 x
    - `DEFAULT '내용'`: 아무 값도 넣지 않으면, 해당 값을 `내용`으로 자동 지정

## 3. DDL

### CREATE
- 테이블 생성
  - 생성할 필드의 데이터 타입과 제약조건 설정
  - 제약조건을 설정하지 않으면, NULL값을 허용
  - 필드 생성 시, id 컬럼은 따로 정의하지 않으면 자동으로 `rowid`라는 컬럼으로 생성
    - `PRIMARY KEY` 키워드를 가진 컬럼을 rowid 컬럼의 별칭으로 사용(rowid 이름으로도 여전히 액세스 가능)
    - 데이터가 최대 값에 도달하면, 사용되지 않는 정수를 찾아 사용 (그런 정수가 없으면, 에러 발생)
    - 최대 값은 2^64개
  - 데이터 타입과 제약조건은 RDBMS에 따라 다름

```SQL
-- 양식
CREATE TABLE table_name (
  column_1 data_type constraints,
  column_1 data_type constraints,
);

CREATE TABLE contacts (
  name TEXT NOT NULL,
  age INTEGER NOT NULL,
  email TEXT NOT NULL UNIQUE
);
```

### ALTER
- 기존 테이블의 구조를 수정
```SQL
-- 1. Rename a table
ALTER TABLE table_name RENAME TO new_table_name;
ALTER TABLE contacts RENAME TO new_contacts;

-- 2. Rename a column
ALTER TABLE table_name RENAME COLUMN column_name TO new_column_name;
ALTER TABLE new_contacts RENAME COLUMN name TO last_name;

-- 3. Add a new column to a table
ALTER TABLE table_name ADD COLUMN column_definition;
ALTER TABLE new_contacts ADD COLUMN address TEXT NOT NULL;
-- 기존에 테이블 내 데이터가 있을 경우, 새롭게 추가되는 컬럼에 값이 없기 때문에 오류 발생
-- DEFAULT 제약 조건을 사용해 해결 가능
ALTER TABLE new_contacts ADD COLUMN address TEXT NOT NULL DEFAULT 'no address';

-- 4. Delete a column
ALTER TABLE table_name DROP COLUMN column_name;
ALTER TABLE new_contacts DROP COLUMN address;
-- 삭제 불가능한 경우:
-- 컬럼이 다른 부분에서 참조되는 경우, PK인 경우, UNIQUE 제약 조건이 있는 경우
```

### DROP
- 데이터베이스에서 테이블을 제거
  - 존재하지 않는 데이터 삭제 시 오류 발생
  - 한 번에 하나의 테이블만 삭제할 수 있음

```SQL
-- 양식
DROP TABLE table_name;

DROP TABLE new_contacts;
```

## 4. DML

### INSERT
- 새 행을 테이블에 삽입 (CREATE)
- 각 컬럼에 해당하는 값을 직접 지정
- 만약 컬럼을 생략하는 경우, 모든 컬럼에 대한 값을 지정해야 함 (컬럼을 포함하는 것을 권장)
- 만약 컬럼의 제약 조건이 `NOT NULL`인 경우, 필수로 값 넣어줘야 함
  
```SQL
-- 양식
INSERT INTO table_name (column1, column2, ...) VALUES (value1, value2, ...);
INSERT INTO table_name VALUES(value1, value2, ...); -- 모든 컬럼에 대한 값 지정

INSERT INTO classmates (name, age, address) VALUES ('홍길동', '23', '대전');
INSERT INTO classmates VALUES ('홍길동', '23', '대전');
-- 여러 행 삽입
INSERT INTO classmates 
VALUES
  ('홍길동', '23', '대전'),
  ('가나다', '24', '서울'),
  ('김이박', '25', '경기');
```

### SELECT
- 특정 테이블에서 데이터를 조회하기 위해 사용 (READ)
- 다양한 절(clause)과 함께 사용 가능
  - `ORDER BY`: 결과를 정렬해서 출력, `FROM` 절 뒤에 위치 [`ASC`: 오름차순(기본 값), `DESC`: 내림차순, `NULL`은 다른 값보다 작은 것으로 간주]
  - `DISTINCT`: 조회 결과에서 중복된 행을 제거, `SELECT` 바로 뒤에 위치
  - `WHERE`: 특정 검색 조건을 지정, `FROM` 절 뒤에 위치, `UPDATE`, `DELETE` 문에서도 사용
  - `LIMIT`: 반환되는 행 수를 제한, `OFFSET`과 함께 사용 시, 특정 위치에서부터 데이터 조회
  - `GROUP BY`: 선택된 컬럼 값을 기준으로 데이터(행)들의 공통 값을 묶어서 결과로 나타냄, `FROM` 절 뒤에 작성(`WHERE`절이 포함된 경우, `WHERE`절 뒤에 작성)

- operators
  - `=`, `<> or !=`, `<`, `>`, `<=`, `>=`
  - Boolean 데이터 타입을 제공하지 않으므로, 1은 TRUE를, 0은 FALSE를 의미
  - `ALL`, `AND`, `ANY`, `BETWEEN`, `IN`, `LIKE`, `NOT`, `OR`

- wildcard
  - `*`: 모든 데이터
  - `%`: 0개 이상의 문자가 올 수 있음을 의미 -> 아무 문자도 없어도 됨
  - `_`: 단일(1개) 문자가 있음을 의미

- Aggregate function: 집계 함수
  - 값 집합의 최대값, 최소값, 평균, 합계 및 개수를 계산
  - `GROUP BY` 절과 종종 사용
  - `AVG()`, `COUNT()`, `MAX()`, `MIN()`, `SUM()`
  - [Bulit-in Aggregate Functions](https://www.sqlite.org/lang_aggfunc.html)

```SQL
-- 양식
SELECT column1, column2 FROM table_name;
-- 모든 정보 조회
SELECT * FROM table_name;

-- ORDER BY
SELECT first_name, age FROM users ORDER BY age
SELECT first_name, age FROM users ORDER BY age DESC;
-- 나이를 기준으로 오름차순으로 정렬하되, 같은 나이면 계좌 잔고를 기준으로 내림차순으로 정렬
SELECT first_name, age, balance FROM users ORDER BY age ASC, balance DESC;

-- DISTINCT
SELECT DISTINCT country FROM users;
SELECT DISTINCT country FROM users ORDER BY country;
-- 두 개 이상의 컬럼을 적는 경우, 각 컬럼의 중복을 따로 보지 않고,
-- 두 컬럼을 하나의 집합으로 보고 중복을 제거 (두 컬럼 모두 중복인 경우만 제거)
SELECT DISTINCT first_name, country FROM users;

-- WHERE
SELECT first_name, age FROM users WHERE age >= 30;
SELECT first_name, age, balance FROM users WHERE age >= 30 and balance > 100000;
-- 이름에 '호'가 포함되는 사람들의 이름 조회
SELECT first_name FROM users WHERE first_name LIKE '%호%';
-- 이름이 '준'으로 끝나는 사람들의 이름 조회
SELECT first_name FROM users WHERE first_name LIKE '%준';
-- 서울 지역 전화번호를 가진 사람들의 전화번호 조회
SELECT phone FROM users WHERE phone LIKE '02-%';
-- 나이가 20대인 사람들의 이름과 나이 조회
SELECT first_name, age FROM users WHERE age LIKE '2_';
-- 나이가 20대가 아닌 사람들의 이름과 나이 조회
SELECT first_name, age FROM users WHERE age NOT LIKE '2_';
-- 경기도 혹은 강원도에 사는 사람들의 이름과 지역 조회 (2가지 방법)
SELECT first_name, country FROM users WHERE country IN ('경기도', '강원도');
SELECT first_name, country FROM users WHERE country = '경기도' OR country = '강원도';
-- 경기도 혹은 강원도에 살지 않는 사람들의 이름과 지역 조회
SELECT first_name, country FROM users WHERE country NOT IN ('경기도', '강원도');
-- 나이가 20살 이상, 30살 이하인 사람들의 이름과 나이 조회(2가지 방법)
SELECT first_name, age FROM users WHERE age BETWEEN 20 AND 30;
SELECT first_name, age FROM users WHERE age >= 20 AND age <= 30;
-- 나이가 20살 이상, 30살 이하가 아닌 사람들의 이름과 나이 조회(2가지 방법)
SELECT first_name, age FROM users WHERE age NOT BETWEEN 20 AND 30;
SELECT first_name, age FROM users WHERE age < 20 OR age > 30;

-- LIMIT
-- 첫 번째부터 열 번째 데이터까지 rowid와 이름 조회
SELECT rowid, first_name FROM users LIMIT 10;
-- 계좌 잔고가 가장 많은 10명의 이름과 계좌 잔고 조회
SELECT first_name, balance FROM users ORDER BY balance DESC LIMIT 10;
-- 11번째부터 20번째 데이터의 rowid와 이름 조회
SELECT rowid, first_name FROM users LIMIT 10 OFFSET 10;

-- Group BY
-- 각 지역별로 묶어서 조회
SELECT country FROM users GROUP BY country;
-- 테이블 내 전체 행 수 조회
SELECT COUNT(*) FROM users;
-- 지역 별로 몇 명씩 살고 있는지 조회 (그룹이 나누어 졌으므로, COUNT(*)는 그룹 별 데이터 개수)
SELECT country, COUNT(*) FROM users GROUP BY country;
-- 컬럼명을 바꾸기
SELECT country, COUNT(*) AS number_of_people FROM users GROUP BY country;
-- 인원이 가장 많은 성씨 순으로 조회
SELECT last_name, COUNT(*) FROM users GROUP BY last_name ORDER BY COUNT(*) DESC;
-- 각 지역별 평균 나이 조회
SELECT country, AVG(age) FROM users GROUP BY country;
```

### UPDATE
- 테이블에 있는 기존 행의 데이터를 수정
- `SET` 절과 `WHERE` 절을 이용해, 컬럼과 행을 지정
  - `WHERE` 절을 생략할 경우, 테이블의 모든 행에 있는 데이터를 업데이트
- 선택적으로 `ORDER BY` 및 `LIMIT` 절을 사용해 행 수를 지정 가능

```SQL
-- 양식
UPDATE table_name
SET column_1 = new_value_1,
    column_2 = new_value_2
WHERE
    search_condition;

UPDATE classmates
SET name='김철수',
    address='제주도'
WHERE rowid = 2;
```

### DELETE
- 테이블에서 행을 제거
- `WHERE` 절에 검색 조건을 추가하여 제거할 행을 식별
  - `WHERE` 절을 생략할 경우, 테이블의 모든 행을 삭제
- 선택적으로 `ORDER BY` 및 `LIMIT` 절을 사용해 행 수를 지정 가능

```SQL
--양식
DELETE FROM table_name WHERE search_condition;

DELETE FROM classmates WHERE rowid = 5;
-- 이름에 '영'이 포함되는 데이터 삭제
DELETE FROM classmates WHERE name LIKE '%영%';
-- 테이블의 모든 데이터 삭제
DELETE FROM classmates;
```