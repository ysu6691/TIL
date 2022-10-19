# JavaScript 기초


## 1. JavaScript 시작하기

### JavaScript란?
- HTML 문서의 콘텐츠를 **동적으로 변경**할 수 있는 언어
- Web을 위해 탄생한 언어이지만, 서버 프로그래밍, 블록체인 등 다양한 분야에서 활용이 가능한 프로그래밍 언어로 변화했다.
- 웹 브라우저는 자바스크립트를 해석하는 엔진을 가지고 있다.
- ES5 표준안에서 ES6 표준안이 제정되면서, 큰 변환가 일어났다.

### JavaScript 실행하기
- Web Browser로 실행하기
  - 첫 번째 방법:
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      ...
    </head>
    <body>
      <script>console.log('Hello, JavaScript!')</script>
    </body>
    </html>
    ```
  - 두 번째 방법:
    ```js
    // hello.js
    console.log('Hello, JavaScript!');
    ```
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      ...
    </head>
    <body>
      <script src="hello.js"></script>
      <!-- <script type="text/javascript" src="hello.js"></script> -->
      <!-- 웹 표준은 위와 같지만, HTML5에서는 type의 default값이 'text/javascript'이므로 생략가능  -->
    </body>
    </html>
    ```
  - 세 번째 방법: 
    웹 브라우저의 console에서 바로 입력 (자바스크립트 엔진이 브라우저에 있기 때문에 가능)
    ※ 웹 브라우저에서 바로 실행할 수 있는 자바스크립트 문접들을 'Vanila JavaScript'라 한다.

- Node.js로 실행하기
  - 자바스크립트 엔진이 있는 Node.js로도 자바스크립트를 실행할 수 있다.
  ```bash
  $ node hello.js
  ```

## 2. JavaScript 기초 문법

### 코드 작성법
- 세미콜론(;)
  - 자바스크립트는 세미콜론을 선택적으로 사용 가능(없으면 자동으로 삽입)

- 들여쓰기와 코드 블럭
  - 들여쓰기는 **2칸**을 사용
  - **블럭은 중괄호 내부**를 말함 -> 중괄호를 사용해 코드 블럭을 구분

- 주석
  - 한 줄 주석: `//`
  - 여러 줄 주석: `/* */`

- 스타일 가이드
  - Airbnb JavaScript Style Guide
  - Google JavaScript Style Guide
  - standardJavaScript

### 변수와 식별자
- 식별자
  - 변수를 구분할 수 있는 변수명
  - 반드시 문자, 달러($) 또는 밑줄(_)로 시작
  - 대소문자를 구분하며, 클래스명 외에는 모두 소문자로 시작
  - 예약어 사용 불가능
  - 3가지 케이스 존재
    - camelCase(=lowerCamelCase): 변수, 객체, 함수에 사용
    - PascalCase(=UpperCamelCase): 클래스, 생성자에 사용
    - UPPER_SNAKE_CASE: 상수(변경될 가능성이 없는 값)에 사용

- 선언, 할당, 초기화
  - 선언: 변수를 생성
  - 할당: 선언된 변수에 값을 저장
  - 초기화: 선언된 변수에 처음으로 값을 저장

- 변수의 선언
  - `let`: 블록 스코프 지역 변수를 선언 / 재할당 가능 & 재선언 불가능
  - `const`: 블록 스코프 읽기 전용 상수를 선언 / 재할당 불가능 & 재선언 불가능
  - `var`: 함수 스코프 변수를 선언 / 재할당 가능 % 재선언 가능
    - "호이스팅"되는 특성으로 인해 문제 발생 가능 -> **let과 const 사용 권장**
  - 변수 선언시 세 가지 키워드 중 하나를 사용하지 않으면 자동으로 `var`로 선언
  - 선언 예시
    ```js
    let number = 10 // 선언 및 초기화
    number = 20 // 재할당
    let number 30 // 재선언 불가
    ```
    ```js
    const number = 10 // 선언 및 초기화
    number = 20 // 재할당도 불가
    ```
    ```js
    var number = 10 // 선언 및 초기화
    number = 20 // 재할당
    var number = 30 // 재선언
    ```

- 호이스팅(hoisting)
  - 변수를 선언 이전에 참조할 수 있는 현상
  - 변수 선언 이전의 위치에서 접근 시 undefined를 반환
  - 변수를 선언하기 전에 접근이 가능한 것은 논리적 흐름을 깨뜨릴 수 있음
  - 하지만 아직까지 많은 기존 코드가 ES6 이전의 문법으로 작성되어 있으므로, var가 쓰이고 있음
  - 호이스팅 예시
    ```js
    console.log(name) // undefined => 선언 이전에 참조
    var name = '홍길동'

    console.log(age) // let과 const는 오류 발생
    let age = 10
    ```

- 블록 스코프와 함수 스코프
  - 블록 스코프:
    - if, for, 함수 등의 중괄호 내부를 가리킴
    - 블록 스코프를 가지는 변수는 블록 바깥에서 접근 불가
    - `let`과 `const`는 블록 스코프 지역 변수를 선언할 때 쓰이므로, 블록 스코프 밖에서는 접근이 불가
    - 예시
      ```js
      let x = 1
      if (x === 1) {
        let x = 2
        console.log(x) // 2
      }
      console.log(x) // 1
      ```

  - 함수 스코프:
    - 함수의 중괄호 내부를 가리킴
    - 함수 스코프를 가지는 변수는 함수 바깥에서 접근 불가
    - `var`는 함수 스코프 지역 변수를 선언할 때 쓰이므로, 함수 스코프 밖에서는 접근이 불가
    - 예시
      ```js
      function hello() {
        for (var i=0; i<12; i++) {
          
        }
        console.log(i)  // for 외부에서는 접근 가능
      }

      hello();  //12
      console.log(i) // 함수 외부에서는 접근 불가능(오류 발생)
      ```

### 데이터 타입
- 원시 타입과 참조 타입으로 나뉨
  - 원시 타입(Primitive type)
    - Number, String, Boolean, undefined, null, Symbol
  - 참조 타입(Reference type)
    - Objects(Array, Function, ...)

- Number
  - 정수 또는 실수형 숫자를 표현
  - NaN(Not a Number): 숫자가 아님을 나타냄
    - NaN을 반환하는 경우
      - 숫자로 변환이 불가능한 경우 (Number(undefined))
      - 결과가 허수인 경우 (Math.sqrt(-1))
      - 피연산자가 NaN (1 + NaN)
      - 정의할 수 없는 계산식 (0 * Infinity)
      - 문자열을 포함하면서 덧셈이 아닌 계산식 ('가' / 2)
  - 예시
    ```js
    const a = 13
    const b = -5
    const c = 3.14
    const d = 2.998e8
    const e = Infinity
    const f = -Infinity
    const g = NaN
    // Number.isNaN(): 주어진 값의 유형이 Number이고 값이 NaN이면 true 반환
    console.log(Number.isNaN(NaN)) // true
    console.log(Number.isNaN(1)) // false
    ```

- String
  - 문자열을 표현하는 자료형
  - 작은 따옴표 또는 큰 따옴표 모두 가능
  - 덧셈을 통해 문자열 붙일 수 있음
  - Template Literal로 문자열 사이에 변수 삽입 가능
    - ES6+ 부터 지원
    - 작은 따옴표(') 대신 Backtick(`)을 이용
    - $를 이용하여 변수를 문자열 사이에 삽입 가능
    - 여러 줄에 걸쳐 문자열 작성 가능
  - 예시
    ```js
    const a = '가'
    const b = '나'
    console.log(a+b) // 가나

    const c = '가'
    const d = 1
    console.log(c+d) // 가1 -> 암묵적 형변환

    const word1 = `안녕
    하세요`
    console.log(word1)
    // 안녕
    // 하세요

    const age = 10
    const message = `제 나이는 ${age}세 입니다.`
    ```

- Empty Value
  - 값이 존재하지 않음을 표현하는 값으로 두 가지가 존재 -> 설계 실수
  - null: 변수에 값이 없음을 의도적으로 표현할 때 사용
  - undefined: 변수 선언 이후 직접 값을 할당하지 않으면 자동으로 할당
  ```js
  let a = null
  console.log(a) // null
  console.log(typeof a) // "object"

  let b
  console.log(b) // undefined
  console.log(typeof b) // "undefined"
  ```

- Boolean
  - true와 false
  - 조건문 또는 반복문에서 boolean이 아닌 데이터 타입은 자동 형변환 규칙에 의해 true/false로 변환
  - 자동 형변환
    | 데이터타입 | false | true |
    |--|--|--|
    | undefined | 항상 거짓 | x |
    | null | 항상 거짓 | x |
    | Number | 0, -0, NaN | 나머지 모든 경우 |
    | String | 빈 문자열 | 나머지 모든 경우 |
    | Object | x | 항상 참 |

### 연산자
- 할당 연산자
  - `=`, `+=`, `-=`, `*=`, `/=` 등

- 산술 연산자
  - `+`, `-`, `**`, `%`, `++`, `--`

- 비교 연산자
  - `==`, `!=`, `===`, `!==`, `>`, `<`, `>=`, `<=` 
  - `==`와 `===`의 차이
    - `==`(동등 연산자): 두 피연산자가 같은 값으로 평가되는지 비교(암묵적 타입 변환 수행) -> **사용 지양**
    - `===`(일치 연산자): 두 피연산자의 값과 타입이 모두 같은지 비교(엄격한 비교) -> **주로 사용**
  - 문자열은 유니코드 값을 사용하여 표준 사전 순서를 기반으로 비교

- 논리 연산자
  - `&&`(and), `||`(or), `!`(not)
  - 단축 평가 지원

- 삼항 연산자
  - `condition ? val1 : val2`
  - `condition`이 참이면 `val1` 반환, 거짓이면 `val2` 반환
  - 예시
    ```js
    true ? 1 : 2 // 1
    
    const result = Math.PI > 4 ? 'Yes' : 'No'
    console.log(result) // No
    ```

### 조건문
- if, else if, else
  - 조건은 소괄호, 실행할 코드는 중괄호 안에 작성
  - 예시
    ```js
    let age = 10

    if (age > 19) {
      console.log('성인')
    } else if (age > 10) {
      console.log('청소년')
    } else {
      console.log('유아')
    }

    // 유아
    ```

- switch
  - 표현식의 결과값과 case 문의 값을 비교
  - break, default 문 선택적 사용 가능 / break문이 없으면, 모든 실행문이 실행됨
  - 가독성 향상 및 유지보수 측면에서 if문 대신 switch 문을 사용할 수 있음
  - 예시
    ```js
    const myId = 'manager'

    switch (myId) {
      case 'admin': {
        console.log('관리자님 환영합니다!')
        break
      }
      case 'manager': {
        console.log('매니저님 환영합니다!')
        break
      }
      // 위 케이스에 break가 없다면, default 문도 실행
      default: {
        console.log(`${myId}님 환영합니다!`)
      }
    }

    // 매니저님 환영합니다!
    ```

### 반복문
- while
  - 조건은 소괄호, 실행할 코드는 중괄호 안에 작성
  - 예시
    ```js
    let i = 0

    while (i < 3) {
      console.log(i)
      i += 1
    }

    // 0
    // 1
    // 2
    ```

- for
  - 소괄호 안에 초기문, 조건문, 증감문을 작성하고, 중괄호 안에 실행할 코드 작성
  - 초기문에서 변수를 선언할 때는 `let`사용(`const`를 사용하면 변수의 값이 바뀌므로 오류 발생)
  - 예시
    ```js
    // i++ 대신 i += 1도 가능
    for (let i = 0; i < 3, i++) {
      console.log(i)
    }

    // 0
    // 1
    // 2
    ```

- for in
  - 객체의 속성을 순회할 때 사용
  - 배열도 순회 가능하지만, 인덱스 순으로 순회한다는 보장이 없으므로 권장하지 않음
  - 변수 선언 시 `const`사용해도 오류 발생 x(매 반복 시 해당 변수를 새로 정의하기 때문)
  - 예시
    ```js
    const fruits = {a:'apple', b:'banana'}
    
    for (const fruit in fruits) {
      console.log(key) // a, b
      console.log(fruits[key]) // apple, banana
    }
    ```

- for of
  - 반복 가능한(iterable) 객체를 순회할 때 사용
  - Array, Set, String 등
  - 예시
    ```js
    const numbers = [0, 1, 2, 3]

    for (const number of numbers) {
      console.log(number) // 0, 1, 2, 3
    }
    ```

- for in vs for of
  - for in: 객체 순회에 적합하다. (속성 이름을 통해 반복)
  - for of: iterable 순회에 적합하다. (속성 값을 통해 반복)
  - 예시
    ```js
    // array
    const fruits = ['사과', '바나나']

    for (let fruit in fruits) {
      console.log(fruit) // 0, 1
    }

    for (let fruit of fruits) {
      console.log(fruit) // 사과, 바나나
    }

    // object
    const capitals = {
      Korea: '서울',
      France: '파리'
    }

    for (let capital in capitals) {
      console.log(capital) // Korea, France
    }

    for (let capital of capitals) {
      console.log(capital) // 오류 발생
    }
    ```

> 참고) Array를 for in으로 돌면 0, 1, ... 가 나오는 이유
> Array도 사실 키와 속성들을 담고 있는 객체이기 때문
> Array는 인덱스를 키로 갖으며 length 프로퍼티를 갖는 특수한 객체
> ```js
> Object.getOwnPropertyDescriptors([1, 2, 3])
> /*
> { 
>   '0': { value: 1, writable: true, enumerable: true, configurable: true },
>   '1': { value: 2, writable: true, enumerable: true, configurable: true },
>   '2': { value: 3, writable: true, enumerable: true, configurable: true },
>   length: { value: 3, writable: true, enumerable: false, configurable: false }
> } 
> */
> ```
> 따라서 다음과 같이 배열을 만들 수도 있지만, [ ]를 사용하는 것이 권장사항이다.
> ```js 
> var myArray = new Array() // new는 생략 가능
> myArray[0] = 1
> console.log(myArray)
> // [ 1 ]
> ```
 
  