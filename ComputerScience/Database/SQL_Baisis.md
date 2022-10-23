# SQL 기초

## 1. SQL 개요

### SQL 문

데이터베이스에서 데이터를 추출하여 문제를 해결하는 데 사용

실행 순서가 없는 비절차적인 언어이다.(찾는 데이터만 기술하고 어떻게 찾는지 그 절차는 기술하지 않는다.)

하지만 내부적으로 DBMS에 의하여 명령에 따른 데이터를 다루는 순서가 정해져 있다.

### SQL의 명령어

**데이터 정의어**(DDL, Data Definition Language): 테이블이나 관계의 구조를 생성

  ex) CREATE, ALTER, DROP

**데이터 조작어**(DML, Data Manipulation Language): 테이블에 데이터를 검색, 삽입, 수정, 삭제하는 데 사용

  ex) SELECT, INSERT, DELETE, UPDATE (SELECT은 질의어(query)라고도 부른다.)

**데이터 제어어**(DCL, Data Control Language): 데이터의 사용 권한을 관리하는 데 사용

  ex) GRANT, REVOKE

> SQL 문법
> 
> 1. SQL문은 세미콜론(;)과 함께 끝난다. 
> 
> 2. 대소문자의 구분은 없지만 SQL 예약어는 대문자로, 테이블이나 속성 이름은 소문자로 적는다.

## 2. 데이터 정의어(DDL)

### CREATE 문

```sql
CREATE TABLE 테이블이름(
  {속성이름 데이터타입 [제약사항]} [KEY 종류]
)
```

  ※ { }: 반복가능,  [ ]: 선택적 사용,  |: 1개 선택,  < >: 해당되는 문법 사항이 있음

- 데이터 타입
  
  | 데이터 타입                        | 설명                                |
  | ----------------------------- | --------------------------------- |
  | INTEGER<br/>INT               | 4바이트 정수형을 저장                      |
  | NUMERIC(m,d)<br/>DECIMAL(m,d) | 전체 자릿수 m, 소수점이하 자릿수 d를 가진 숫자형을 저장 |
  | CHAR(n)                       | 문자형 고정길이, 문자를 저장하고 남은 공간은 공백 처리   |
  | VARCHAR(n)                    | 문자형 가변길이 저장                       |
  | DATE                          | 날짜형, 연도, 월, 날, 시간을 저장             |

- 제약사항
  
  | 명령어      | 내용                              |
  | -------- | ------------------------------- |
  | NOT NULL | NULL 값을 허용하지 않는다.               |
  | UNIQUE   | 유일한 값을 갖는다.                     |
  | DEFAULT  | 기본값을 설정한다.                      |
  | CHECK    | 값에 대한 조건을 부여한다. (ex. age >= 18) |

- KEY 종류
  
  - PRIMARY KEY: `PRIMARY KEY 속성이름(들)`
  
  - FOREIGN KEY: `FOREIGN KEY 속성이름 REFERENCES 테이블이름(속성이름) 제약사항`
    
    - 제약사항 종류:
      
      - `ON DELETE 옵션`: 참조되고 있는 개체 삭제 시 옵션 지정
      
      - `ON UPDATE 옵션`: 참조되고 있는 개체 변경 시 옵션 지정
      
      - 옵션 종류
        
        - RESTRICT: 참조하고 있는 개체와 함께 변경/삭제 취소(제한)
        
        - NO ACTION: MYSQL에서는 RESTRICT와 동일, 기본값
        
        - CASCADE: 참조하고 있는 개체와 함께 변경/삭제
        
        - SET NULL: 외래키를 NULL로 변경

- 예시
  
  ```sql
  CREATE TABLE Book (
    bookid      INTEGER PRIMARY KEY,
    bookname    VARCHAR(40) NOT NULL,
    publisher   VARCHAR(40),
    price       INTEGER CHECK(price > 0)
  );
  
  CREATE TABLE  Customer (
    custid      INTEGER PRIMARY KEY,  
    name        VARCHAR(40),
    address     VARCHAR(50) DEFAULT 'no address',
    phone       VARCHAR(20) UNIQUE
  );
  
  CREATE TABLE Orders (
    orderid INTEGER PRIMARY KEY,
    custid  INTEGER,
    bookid  INTEGER,
    saleprice INTEGER,
    orderdate DATE,
    FOREIGN KEY (custid) REFERENCES Customer(custid),
    FOREIGN KEY (bookid) REFERENCES Book(bookid) ON DELETE CASCADE
  );
  ```

### ALTER 문

```sql
ALTER TABLE 테이블이름
  [ADD 속성이름 데이터타입]
  [DROP COLUMN 속성이름]
  [ALTER COLUMN 속성이름 데이터타입]
  [ALTER COLUMN 속성이름 [NULL | NOT NULL]]
  [ADD PRIMARY KEY(NOT NULL인 속성이름)]
  [[ADD | DROP] 제약이름]
```

- 예시
  
  ```sql
  ALTER TABLE Book ADD isbn VARCHAR(13);
  ALTER TABLE Book MODIFY isbn INTEGER;
  ALTER TABLE Book MODIFY isbn INTEGER NOT NULL;
  ALTER TABLE Book DROP COLUMN isbn;
  ```

### DROP 문

```sql
DROP TABLE 테이블이름
```

## 3. 데이터 조작어(DML)

### SELECT 문

```sql
SELECT [ALL | DISTINCT] 속성이름(들)
FROM 테이블이름(들)
[WHERE 검색조건(들)]
[GROUP BY 속성이름]
[HAVING 검색조건(들)]
[ORDER BY 속성이름 [ASC | DESC]]
```

- **SELECT 문 기초**
  
  - 예시
    
    ```sql
    -- 도서 테이블의 모든 도서 이름과 가격을 조회
    SELECT bookname, price FROM Book;
    -- 도서 테이블의 모든 도서 정보 조회
    SELECT * FROM Book;
    -- 도서 테이블에 있는 모든 출판사 검색
    SELECT DISTINCT publisher FROM Book;
    ```

- **WHERE**: 조건에 맞는 검색을 할 때 사용
  
  - 연산자 종류
    
    | 술어   | 연산자                     |
    | ---- | ----------------------- |
    | 비교   | =, !=(<>), <, >, <=, >= |
    | 범위   | BETWEEN                 |
    | 집합   | IN, NOT IN              |
    | 패턴   | LIKE                    |
    | NULL | IS NULL, IS NOT NULL    |
    | 복합조건 | AND(&&), OR(            |
  
  - 와일드 문자 종류
    
    | 와일드 문자 | 의미                |
    | ------ | ----------------- |
    | +      | 문자열을 연결           |
    | %      | 0개 이상의 문자열과 일치    |
    | [ ]    | 1개의 문자와 일치        |
    | [^]    | 1개의 문자와 불일치       |
    | _      | 특정 위치의 1개의 문자와 일치 |
  
  - 예시
    
    ```sql
    -- 가격이 20000원 이상인 도서 이름 검색
    SELECT bookname FROM Book WHERE price>=20000;
    -- 출판사가 '대한미디어'가 아닌 도서 이름 검색
    SELECT bookname FROM Book WHERE publisher<>'대한미디어';
    SELECT bookname FROM Book WHERE publisher!='대한미디어';
    -- 가격이 10000원 이상 20000원 이하인 도서 이름 검색
    SELECT bookname FROM Book WHERE price BETWEEN 10000 AND 20000;
    SELECT bookname FROM Book WHERE price>=10000 AND price<=20000;
    -- 출판사가 '대한미디어' 또는 '굿스포츠'인 도서 이름 검색
    SELECT bookname FROM Book WHERE publisher IN ('대한미디어', '굿스포츠')
    SELECT bookname FROM Book WHERE publisher LIKE '대한미디어' OR publisher LIKE '굿스포츠'
    SELECT bookname FROM Book WHERE publisher='대한미디어' OR publisher='굿스포츠'
    -- 도서 이름의 왼쪽 두 번째 위치에 '구'라는 문자열을 갖는 도서 이름 검색
    SELECT bookname FROM Book WHERE bookname LIKE '_구%'
    ```
  
  > **문자열 비교 시 인용부호**
  > SQL 언어에서 문자열은 작은 따옴표(' ')를 사용한다.
  > 하지만 SELECT 문에서 AS 다음에 붙이는 별칭은 큰 따옴표(" ")를 붙일 수 있다.
  > 만약 별칭에 공백이 포함될 경우에는 반드시 큰 따옴표를 사용한다.

- **ORDER BY**: 실행 결과를 특정 순서대로 출력
  
  - ASC: 오름차순, 기본값
  
  - DESC: 내림차순
  
  - 예시
    
    ```sql
    -- 도서를 이름순으로 검색
    SELECT * FROM Book ORDER BY bookname;
    -- 도서를 가격의 내림차순으로 검색, 만약 가격이 같다면 출판사의 오름차순으로 검색
    SELECT * FROM Book ORDER BY price DESC, publisher;
    ```

- **GROUP BY**
  
  - 속성 값이 같은 튜플끼리 그룹을 만든다.
  
  - 집계 함수: 테이블의 각 열에 대해 계산을 하는 함수
  
  - 집계 함수 종류: SUM, AVG, MIX, MAX, COUNT
  
  - HAVING: 그룹으로 묶은 후 조건에 맞는 튜플만 검색한다.
  
  - 예시
    
    ```sql
    -- 집계 함수 예시
    -- 고객이 주문한 도서의 총 판매액 검색
    SELECT SUM(saleprice) FROM Orders;
    -- 2번 고객이 주문한 도서의 총 판매액을 구하는데, 열 이름을 '총매출'로 변경해서 검색
    SELECT SUM(saleprice) AS '총매출' FROM Orders WHERE custid=2;
    -- 총 도서 판매 건수를 검색
    SELECT COUNT(*) FROM Orders;
    
    -- GROUP BY 예시
    -- 고객별로 주문한 도서의 총 수량과 총 판매액 검색
    SELECT custid, COUNT(*), SUM(saleprice) FROM Orders GROUP BY custid;
    -- 가격이 8000원 이상인 도서를 구매한 고객에 대하여 고객별 주문 도서의 총 수량을 검색
    SELECT custid, COUNT(*) FROM Orders WHERE saleprice>=8000 GROUP BY custid HAVING count(*)>=2;
    ```
  
  > **주의사항**
  > 
  > 1. SELECT 절에는 GROUP BY로 묶은 속성만 나올 수 있다. (집계함수는 예외)

  >    맞는 예: `SELECT custid, SUM(saleprice) FROM Orders GROUP BY custid;`

  >    틀린 예: `SELECT bookid, SUM(saleprice) FROM Orders GROUP BY custid;`

  > 2. HAVING 절은 반드시 GROUP BY와 함께 작성해야 하고 WHERE 절보다 뒤에 나와야 한다. 그리고 검색조건에는 집계함수만 올 수 있다.

  >    맞는 예: `SELECT custid FROM Orders WHERE saleprice>=8000 GROUP BY custid HAVING count(*)>=2;`

  >    틀린 예: `SELECT custid FROM Orders HAVING count(*)>=2 WHERE saleprice>=8000 GROUP BY custid;`

- **SQL문의 실행 순서**

  ①FROM -> ②WHERE -> ③GROUP BY -> ④HAVING -> ⑤SELECT -> ⑥ORDER BY
  
  ①테이블을 불러온 후, ②조건에 맞는 튜플만 남긴다.
  
  ③그룹으로 묶은 뒤, ④조건에 맞는 그룹만 남긴다
  
  ⑤어떤 열을 출력할지 선택한 뒤, 순서대로 정렬 후 출력한다.

- **Join**
  
  - 한 테이블의 행을 다른 행의 테이블의 행에 연결하여, 두 개 이상의 테이블을 결합하는 연산
  
  - 테이블이 여러 개일 경우 열 이름이 중복될 수 있으므로, **테이블 이름.열 이름** 형식으로 작성한다.
  
  - 일반적인 조인(내부조인)과 외부조인
    
    - 일반적인 조인
      
      ```sql
      -- 양식 1
      SELECT <속성들> FROM 테이블1, 테이블2 WHERE <조인조건> AND <검색조건>
      
      -- 양식 2
      SELECT <속성들> FROM 테이블1 INNER JOIN 테이블2 ON <조인조건> WHERE <검색조건>
      ```
    
    - 외부조인
      
      ```sql
      SELECT <속성들> 
      FROM 테이블1 { LEFT | RIGHT | FULL } [OUTER]
           JOIN 테이블2 ON <조인조건>
      WHERE <검색조건>
      ```
  
  - 예시
    
    ```sql
    -- 고객과 고객의 주문에 관한 데이터를 모두 검색
    SELECT * FROM Customer, Orders WHERE Customer.custid=Orders.custid;
    -- 고객별로 주문한 모든 도서의 총 판매액을 검색
    SELECT name, SUM(saleprice) FROM Customer, Orders WHERE Customer.custid=Orders.custid GROUP BY Customer.name;
    -- 고객의 이름과 고객이 주문한 도서의 이름을 검색
    SELECT Customer.name, Book.bookname FROM Customer, Orders, Book WHERE Customer.custid=Orders.custid AND Orders.bookid=Book.bookid;
    -- 
    ```
  
  - **셀프 조인**
    
    동일 테이블 사이의 조인을 말한다. 즉 FROM절에 동일 테이블이 두 번 이상 나타난다.
    
    따라서 테이블과 컬럼 이름이 모두 동일하기 때문에, 식별을 위해 **반드시 테이블 별칭(Alias)를 사용해야 한다.**
    
    <img src="https://user-images.githubusercontent.com/109272360/197336222-e8ac6836-c3fa-4182-8636-8bf0241d2bbc.png" width="630px;">
    
    예시:

      ```sql
      -- '홍길동' 사원이 관리하는 부하사원의 이름을 검색
      SELECT staff.ename FROM Emp staff, Emp manager WHERE staff.mgr=manager.empno and manager.ename LIKE '홍길동';
      ```

### INSERT 문

```sql
INSERT INTO 테이블이름[(속성리스트)]
       VALUES (값리스트);
```

- 데이터를 삽입하는 명령어이다.

- 속성리스트는 생략해도 되지만, 생략한다면 값을 입력할 때 속성 순서대로 입력해야 한다.

- 속성리스트를 작성할 때는 항상 속성의 순서대로 입력하지 않아도 된다.

- 만약 값을 입력하지 않은 속성이 있다면, 기존에 설정해두었던 default 값으로 값이 입력된다.

- 어떠한 테이블의 데이터를 다른 테이블로 삽입하고 싶다면, SELECT 문을 적절히 활용할 수 있다.

- 예시
  
  ```sql
  -- Book 테이블에 새로운 도서 '스포츠 의학'을 삽입
  -- 책 번호: 11, 출판사: 한솔의학서적, 가격: 90000원
  INSERT INTO Book(bookid, bookname, publisher, price)
          VALUES (11, '스포츠 의학', '한솔의학서적', 90000);
  
  -- 수입도서 목록(Imported_book)을 Book 테이블에 모두 삽입
  INSERT INTO Book(bookid, bookname, price, publisher)
         SELECT bookid, bookname, price, publisher
         FROM Imported_book;
  ```

### UPDATE 문

```sql
UPDATE 테이블이름
SET    속성이름1=값1[, 속성이름2=값2, ...]
[WHERE <검색조건>];
```

- 테이블 내 데이터의 특정 속성 값을 수정하는 명령이다.

- 다른 테이블의 속성 값으로 수정하려면, SELECT 문을 적절히 활용할 수 있다.

- WHERE 절을 입력하지 않으면 모든 데이터가 수정되므로 주의해야 한다.

- MySQL에서는 UPDATE 문을 실행하기 위해서는 Safe Updates 옵션을 해제해야 한다.

  - Safe update mode: UPDATE 및 DELETE 수행 시 실수를 방지하기 위해 만든 옵션

  - [Edit] -> [Preferences] -> [SQL Editor] -> 'Safe Updates' 체크 해제 -> [OK]

- 예시

  ```sql
  -- Safe Updates 옵션 미 해제 시 다음 명령어를 실행하면 일시적으로 safe update mode 해제
  -- SET SQL_SAFE_UPDATES=0;

  -- Customer 테이블에서 고객번호가 5인 고객의 주소를 '대한민국 부산'으로 변경
  UPDATE Customer
  SET    address='대한민국 부산'
  WHERE  custid=5;

  -- Book 테이블에서 14번 도서의 출판사를 imported_book 테이블의 21번 도서의 출판사와 동일하게 변경
  UPDATE Book
  SET    publisher = (SELETE publisher
                      FROM imported_book
                      WERE bookid = '21')
  WHERE bookid = '14';
  ```

### DELETE 문

```sql
DELETE FROM 테이블이름
[WHERE 검색조건];
```

- 테이블 내 투플을 삭제하는 명령이다.

- WHERE 절을 입력하지 않으면 모든 데이터가 삭제되므로 주의해야 한다.

- 예시
  ```sql
  -- Book 테이블에서 도서번호가 11인 도서를 삭제
  DELETE FROM Book
  WHERE  bookid = '11';
  ```
