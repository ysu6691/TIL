# SQL 고급

## 1. 내장 함수

### 내장 함수

SQL 내장 함수는 상수나 속성 이름을 입력 값으로 받아 단일 값을 결과로 반환한다.

모든 내장 함수는 최초에 선언될 때 유효한 입력 값을 받아야 한다.

SQL 내장 함수는 `SELECT`절과 `WHERE`절, `UPDATE`절 등에서 모두 사용 가능하다.

MYSQL에서 제공하는 내장 함수들
  |구분|함수|
  |-- | -- |
  |숫자 함수|ABS, CEIL, COS, EXP, FLOOR, LN, LOG, MOD, POWER, RAND, ROUND, SIGN, TRUNCATE|
  |문자 함수(문자 반환)|CHAR, CONCAT, LEFT, RIGHT, LOWER, UPPER, LPAD, RPAD, LTRIM, RTRIM, REPLACE, REVERSE, RIGHT, SUBSTR, TRIM|
  |문자 함수(숫자 반환)|ASCII, INSTR, LENGTH|
  |날짜/시간 함수|ADDDATE, CURRENT_DATE, DATE, DATEDIFF, DAYNAME, LAST_DAY, SYSDATE, TIME|
  |변환 함수|CAST, CONVERT, DATE_FORMAT, STR_TO_DATE|
  |정보 함수|DATABASE, SCHEMA, ROW_COUNR, USER, VERSION|
  |NULL 관련 함수|COALESCE, ISNULL, IFNULL, NULLIF|
  |집계 함수|AVG, COUNT, MAX, MIN, STD, STDDEV, SUM|
  |윈도우 함수(분석 함수)|CUME_DIST, DENSE_RANK, FIRST_VALUE, LAST_VALUE, LEAD, NTILE, RANK, ROW_NUMBER|

### 숫자 함수
|함수|설명|예시|
|--|--|--|
|ABS(숫자)|숫자의 절댓값을 계산|ABS(-4.5) -> 4.5|
|CEIL(숫자)|숫자보다 크거나 같은 최소의 정수|CEIL(4.1) -> 5|
|FLOOR(숫자)|숫자보다 작거나 같은 최소의 정수|FLOOR(4.1) -> 4|
|ROUND(숫자, m)|m번째 자리에서 숫자 반올림|ROUND(5.36, 1) -> 5.40|
|LOG(n, 숫자)|밑이 n인 숫자의 로그 값을 반환 (n이 없으면 자연로그)|LOG(10) -> 2.30259|
|POWER(숫자, n)|숫자의 n제곱 값을 반환|POWER(2, 3) -> 8|
|SQRT(숫자)|숫자의 제곱근 값을 반환(숫자는 양수)|SQRT(9.0) -> 3.0|
|SIGN(숫자)|숫자가 음수면 -1, 0이면 0, 양수면 1|SIGN(3.4) -> 1|

- 예시
  ```sql
  -- 고객별 푱균 주문 금액을 백원 단위로 반올림한 값을 구하시오.
  SELECT   custid '고객번호', ROUND(SUM(saleprice)/COUNT(*), -2) '평균금액'
  FROM     Orders
  GROUP BY custid;
  ```

### 문자 함수

s: 문자열, c: 문자, n: 정수, k: 정수
<table>
  <tr>
    <td>반환 구분</td>
    <td>함수</td>
    <td>설명</td>
    <td>예시</td>
  </tr>
  <tr>
    <td rowspan="8">문자값 반환 함수</td>
    <td>CONCAT(s1,s2)</td>
    <td>두 문자열을 연결</td>
    <td>CONCAT('가나', '다라') -> '가나 다라')
  </tr>
  <tr>
    <td>LOWER(s)</td>
    <td>문자열을 모두 소문자로 변환</td>
    <td>LOWER('ABC') -> 'abc'</td>
  </tr>
  <tr>
    <td>LPAD(s,n,c)</td>
    <td>대상 문자열의 왼쪽부터 지정한 자리수까지 지정한 문자로 채움</td>
    <td>LPAD('ABCD', 7, '*') -> '***ABCD' </td>
  </tr>
  <tr>
    <td>REPLACE(s1,s2,s3)</td>
    <td>대상 문자열의 지정한 문자를 원하는 문자로 변경</td>
    <td>REPLACE('ABAB', 'A', 'DE') -> 'DEBDEB'</td>
  </tr>
  <tr>
    <td>RPAD(s,n,c)</td>
    <td>대상 문자열의 오른쪽부터 지정한 자리수까지 지정한 문자로 채움</td>
    <td>RPAC('ABCD', 7, '*') -> 'ABCD***'</td>
  </tr>
  <tr>
    <td>SUBSTR(s,n,k)</td>
    <td>대상 문자열의 지정된 자리(n)에서부터 지정된 길이(k)만큼 잘라서 반환</td>
    <td>SUBSTR('ABCDEF', 2, 3) -> 'BCD'</td>
  </tr>
  <tr>
    <td>TRIM(c FROM s)</td>
    <td>대상 문자열의 양쪽에서 지정된 문자를 삭제<br>(문자열만 넣으면 기본값으로 공백 제거)</td>
    <td>TRIM('=' FROM '==ABC==') -> 'ABC'</td>
  </tr>
  <tr>
    <td>UPPER(s)</td>
    <td>문자열을 모두 대문자로 변환</td>
    <td>UPPER('abc') -> 'ABC'</td>
  </tr>
  <tr>
    <td rowspan="3">숫자값 반환 함수</td>
    <td>ASCII(c)</td>
    <td>알파벳 문자의 아스키 코드 값을 반환</td>
    <td>ASCII('D') -> 68</td>
  </tr>
  <tr>
    <td>LENGTH(s)</td>
    <td>문자열의 Byte 반환<br>(알파벳 1byte, 한글 3byte)</td>
    <td>LENGTH('ABCD') -> 4</td>
  </tr>
  <tr>
    <td>CHAR_LENGTH(s)</td>
    <td>문자열의 문자 수를 반환</td>
    <td>CHAR_LENGTH('가나다') -> 3</td>
  </tr>
</table>

- 예시
  ```sql
  -- 도서제목에 야구가 포함된 도서를 농구로 변경한 후 도서 목록 검색
  SELECT bookid, REPLACE(bookname, '야구', '농구') bookname, publisher, price
  FROM   Book;

  -- 굿스포츠에서 출판한 도서의 제목과 제목의 문자 수, 바이트 수를 검색
  SELECT bookname, CHAR_LENGTH(bookname), LENGTH(bookname)
  FROM   Book
  WHERE  publisher='굿스포츠';

  -- 고객 중에서 같은 성을 가진 사람이 몇 명이나 되는지 성별 인원수를 검색
  SELECT   SUBSTR(name, 1, 1) '성', COUNT(*) '인원'
  FROM     Customer
  GROUP BY SUBSTR(name, 1, 1)
  ```

### 날짜/시간 함수

|함수|반환형|설명|예시|
|--|--|--|--|
|STR_TO_DATE(string, format)|DATE|문자열(STRING) 데이터를 날짜형(DATE)으로 반환|STR_TO_DATE('2019-02-14', '%Y-%m-%d') -> 2019-02-14|
|DATE_FORMAT(date, format)|STRING|날짜형(DATE) 데이터를 문자열(VARCHAR)로 반환|DATE_FORMAT('2019-02-14', '%Y-%m-%d') -> '2019-02-14'|
|ADDDATE(date, interval)|DATE|DATE 형의 날짜에서 INTERVAL 지정한 시간만큼 더함|ADDDATE('2019-02-14', INTERVAL 10 DAY) -> 2019-02-24|
|DATE(date)|DATE|DATE 형의 날짜 부분을 반환|DATE('2003-12-31 01:02:03') -> 2003-12-31|
|DATEDIFF(date1, date2)|INTEGER|두 DATE 형의 날짜 차이를 반환|DATEDIFF('2019-02-14', '2019-02-04') -> 10|
|SYSDATE|DATE|DBMS 시스템상의 오늘 날짜를 반환|SYSDATE() -> 2019-06-30 21:47:01|

- 날짜와 시간 부분을 나타내는 format의 주요 지정자(specifier)

  |인자|설명|
  |--|--|
  |%w|요일 순서(0~6, Sunday=0)|
  |%W|요일(Sunday~Saturday)|
  |%a|요일의 약자(Sun~Sat)|
  |%d|1달 중 날짜(00~31)|
  |%j|1년 중 날짜(001~366)|
  |%h|12시간(01~12)|
  |%H|24시간(00~23)|
  |%i|분(0~59)|
  |%m|월 순서(01~12, January=01)|
  |%b|월 이름 약어(Jan~Dec)|
  |%M|월 이름(January~December)|
  |%s|초(0~59)|
  |%Y|4자리 연도|
  |%y|4자리 연도의 마지막 2자리|

- 예시
  ```sql
  -- 주문일로부터 10일 후 매출을 확정한다면, 각 주문의 확정일자를 검색
  SELECT orderid '주문번호', orderdate '주문일', ADDDATE(orderdate, INTERVAL 10 DAY) '확정'
  FROM   Orders;

  -- 2014년 7월 7일에 주문받은 도서의 주문번호, 주문일, 고객번호, 도서번호를 모두 검색 (주문일은 %Y-%m-%d 형태)
  SELECT orderid '주문번호', STR_TO_DATE(orderdate, '%Y-%m-%d') '주문일', custid '고객번호', bookid '도서번호'
  FROM   Orders
  WHERE  orderdate=DATE_FORMAT('20140707', '%Y%m%d');
  ```

### NULL 값 처리

NULL 값은 아직 지정되지 않은 값으로, '0', ''(빈 문자), ' '(공백)과는 다른 값이다.

비교 연산자로 비교가 불가능하며, NULL 값의 연산을 수행하면 결과 역시 NULL 값으로 반환된다.

집계 함수를 계산할 때 NULL이 포함된 행은 집계에서 빠진다.

NULL 값인지 확인할 때는 `=`연산자가 아닌 `IS NULL` 또는 `IS NOT NULL`을 사용한다.

`IFNULL` 함수는 NULL 값을 다른 값으로 대치하여 연산하거나 다른 값으로 출력하는 함수이다. `IFNULL(속성, 값)` 형태로 사용하며, 속성 값이 NULL이면 '값'으로 대치한다.

- 예시
  ```sql
  -- 가격이 NULL 값인 모든 도서 검색
  SELECT * FROM Book WHERE price IS NULL;

  -- 고객의 이름과 전화번호를 검색 (단, 전화번호가 없는 고객은 '연락처없음'으로 표시)
  SELECT name, IFNULL(phone, '연락처없음') FROM Customer;
  ```

### 변수를 활용한 행번호 출력

- 사용자 정의 변수 선언 및 초기화
  ```sql
  SET @변수이름 := 대입값;
  SELECT @변수이름 := 대입값;
  ```

- 변수를 활용한 행번호 출력
  ```sql
  -- 고객 목록에서 고객번호, 이름, 전화번호를 앞의 두 명만 검색
  SET @seq:=0;
  SELECT (@seq:=@seq+1) '순번', custid, name, phone
  FROM   Customer
  WHERE  @seq<2;
  ```