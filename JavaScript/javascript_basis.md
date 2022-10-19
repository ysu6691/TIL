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
- `if`, `else if`, `else`
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

- `switch`
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
- `while`
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

- `for`
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

- `for in`
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

- `for of`
  - 반복 가능한(iterable) 객체를 순회할 때 사용
  - 인덱스를 활용하지 않고도 바로 요소에 접근이 가능하다.
  - Array, Set, String 등에 사용한다.
  - 예시
    ```js
    const numbers = [0, 1, 2, 3]

    for (const number of numbers) {
      console.log(number) // 0, 1, 2, 3
    }
    ```

- `for in` vs `for of`
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

> **※참고: Array를 for in으로 돌면 0, 1, ... 가 나오는 이유** <br>
> Array도 사실 키와 속성들을 담고 있는 **객체**이기 때문 <br>
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
> 따라서 다음과 같이 배열을 만들 수도 있지만, **[ ]를 사용하는 것이 권장사항**이다.
> ```js 
> var myArray = new Array() // new는 생략 가능
> myArray[0] = 1
> console.log(myArray)
> // [ 1 ]
> ```

## 3. 함수

### 함수의 정의
- 함수 선언식(Function declaration)
  - 일반적인 프로그래밍 언어의 함수 정의 방식
  - 함수 선언식으로 정의한 함수는 호이스팅 발생 (변수로 평가 x)
  - 예시
    ```js
    function add(num1, num2) {
      return num1 + num2
    }

    console.log(add(1, 3)) // 4
    ```
  
- 함수 표현식(Function expression)
  - 표현식 내에서 함수를 정의하는 방식
  - 함수의 이름을 생략한 **익명 함수**를 어떤 함수 식별자에 할당해서 정의
  - 함수 표현식으로 선언한 함수는 함수 정의 전에 호출 시 에러 발생 (변수로 평가되어 변수의 scope 규칙을 따름)
  - 따라서 **선언식 대신 표현식을 권장**
  - 예시
    ```js
    const sub = function (num1, num2) {
      return num1 - num2
    }

    console.log(sub(5, 3)) // 2

    // 표현식에서 함수 이름을 명시하는 것도 가능
    // 단, 이 경우는 함수 이름은 호출에 사용 불가(디버깅 용도로 사용)
    const mySub = function namedSub(num1, num2) {
      return num1 - num2
    }
    mySub(3, 1) // 2
    namedSub(3, 1) // 오류 발생
    ```

- 함수의 인자
  - 파라미터 작성 시, 기본 파라미터 값 선언 가능
  - 파라미터보다 받은 인자의 개수가 많으면, 설정한 파라미터의 수만큼 인자 받음
  - 파라미터보다 받은 인자의 개수가 적으면, undefined로 처리
  - 예시
    ```js
    // 기본 파라미터 값 설정
    const greeting = function (name = 'Anonymous') {}
      return `Hi ${name}`
    }
    greeting() // Hi Anonymous

    // 받은 인자의 개수가 더 많은 경우
    const twoArgs = function (arg1, arg2) {
      return [arg1, arg2]
    }
    twoArgs(1, 2, 3) // [1, 2]

    // 받은 인자의 개수가 더 적은 경우
    const threeArgs = function (arg1, arg2, arg3) {
      return [arg1, arg2, arg3]
    }
    threeArts(1) // [1, undefined, undefined]
    ```

- Spread syntax(...)
  - 전개 구문
  - 배열의 경우 요소, 함수의 경우 인자로 반복 가능한 객체를 받을 수 있음
  - 예시
    ```js
    // 배열
    let a = [3, 4]
    let b = [1, 2, ...a, 5]
    console.log(b) // [ 1, 2, 3, 4, 5 ]

    // 함수
    const restOpr = function (arg1, arg2, ...restArgs) {
      return [arg1, arg2, restArgs]
    }
    restArgs(1, 2, 3) // [ 1, 2, [ 3 ] ]
    restArgs(1) // [1, undefined, [ ]]
    ```

### Arrow Function
- 화살표 함수(Arrow Fuction)
  - 함수를 비교적 간결하게 정의할 수 있는 문법
    1. function 키워드 생략 가능
    2. 함수의 파라미터가 하나뿐이라면 ( )도 생략 가능
    3. 함수의 내용이 한 줄이라면 { }와 return도 생략 가능
  - 화살표 함수는 항상 익명 함수
  - == 함수 표현식에서만 사용 가능
  - 예시
    ```js
    const arrow1 = function (x) {
      return x
    }

    // 1. function 키워드 삭제
    const arrow2 = (x) => {return x}

    // 2. 파라미터가 1개일 경우에만 ( ) 생략 가능
    const arrow3 = x => {return x}

    // 3. 함수 바디가 return을 포함한 표현식 1개일 경우에 { } & return 생략 가능
    const arrow4 = x => x

    // 응용1. 인자가 없다면 ( ) 대신 _ 로 표현 가능
    const noArgs = _ => 'No args'

    // 응용2. object를 return 하는 경우에는 return을 명시적으로 적는다.
    const returnObject1 = () => { return { key: 'value' } }

    // 응용3: return을 적지 않으려면 괄호를 붙여야 한다.
    const returnObject2 = () => ( {key: 'value'} )
    ```

### 즉시 실행 함수(IIFE)
- IIFE: Immediately Invoked Function Expression
  - 선언과 동시에 실행되는 함수
  - 함수의 선언 끝에 소괄호를 추가하여 선언되자 마자 실행
  - 선언과 동시에 실행되기 때문에 같은 함수를 다시 호출할 수 없음
  - **일회성 함수**이므로, 익명함수로 사용하는 것이 일반적
  - 예시
    ```js
    (function(num) { return num ** 2 })(2); // 8

    (num => num ** 3)(2); // 8
    ```

## 4. 배열(Array)

### 배열
- 키와 속성들을 담고 있는 객체
- 순서를 보장
- 주로 대괄호를 이용하여 생성
- 0을 포함한 양의 정수 인덱스로 접근 가능
- 배열의 길이는 `array.length` 형태로 접근
  - 배열의 마지막 원소는 `array.length - 1`로 접근
  
### 배열 메소드 기초
| 메소드 | 설명 | 비고 |
|--|--|--|
| reverse |원본 배열의 요소들의 순서를 반대로 정렬||
|push & pop|배열의 가장 뒤의 요소를 추가 또는 제거||
|unshift & shift|배열의 가장 앞의 요소를 추가 또는 제거||
|includes|배열에 특정 값이 존재하는지 판별 후 참/거짓 반환||
|indexOf|배열에 특정 값이 존재하는지 판별 후 인덱스 반환|요소가 없을 경우 -1 반환|
|join|배열의 모든 요소를 구분자를 이용하여 연결|구분자 생략 시 쉼표 기준|

```js
// reverse
const numbers = [1, 2, 3]
numbers.reverse()
console.log(numbers) // [ 3, 2, 1 ]

// push & pop
numbers.push(100)
console.log(numbers) // [ 3, 2, 1, 100]

numbers.pop() // 100 반환
console.log(numbers) // [ 3, 2, 1 ]

// unshift & shift
numbers.unshift(100)
console.log(numbers) // [ 100, 3, 2, 1 ]

numbers.shift() // 100 반환
console.log(numbers) // [ 3, 2, 1 ]

// includes
console.log(numbers.includes(1)) // true

// indexOf
console.log(numbers.indexOf(2)) // 1

// join
console.log(numbers.join()) // 3,2,1
console.log(numbers.join('')) // 321
```

### 배열 메소드 심화
- Array Helper Methods
  - 배열을 순회하며 특정 로직을 수행하는 메소드
  - 메소드 호출 시 인자로 **callback 함수**를 받는 것이 특징
    - callback 함수: 어떤 함수의 내부에서 실행될 목적으로 인자로 넘겨받는 함수
    - 다양한 함수 형태로 사용 가능하다.
  |메소드|설명|비고|
  |--|--|--|
  |forEach|배열의 각 요소에 대해 콜백 함수를 한 번씩 실행|반환 값 없음|
  |map|콜백 함수의 반환 값을 요소로 하는 새로운 배열 반환||
  |filter|콜백 함수의 반환 값이 참인 요소들만 모아서 새로운 배열을 반환||
  |reduce|콜백 함수의 반환 값들을 하나의 값에 누적 후 반환||
  |find|콜백 함수의 반환 값이 참이면 해당 요소를 반환||
  |some|배열의 요소 중 하나라도 판별 함수를 통과하면 참을 반환||
  |every|배열의 모든 요소가 판별 함수를 통과하면 참을 반환||

- `forEach`
  - `array.forEach(callback(element[, index[, array]]))`
    - 참고: [ ]는 선택 사항이다.
  - 인자로 주어지는 콜백 함수를 배열의 각 요소에 대해 한 번씩 실행
  - 예시
    ```js
    const colors = ['red', 'blue', 'green']

    colors.forEach( (color) => {
      console.log(color)
    })
    /*
    red
    blue
    green
    */

    colors.forEach( (color, i, myArray) => {
      console.log(color, i, myArray)
    })
    /*
    red 0 [ 'red', 'blue', 'green' ]
    blue 1 [ 'red', 'blue', 'green' ]
    green 2 [ 'red', 'blue', 'green' ]
    */
    ```

- `map`
  - `array.map(callback(element[, index[, array]]))`
  - 콜백 함수의 반환 값을 요소로 하는 새로운 배열 반환
  - 기존 배열 전체를 다른 형태로 바꿀 때 유용
  - 예시
    ```js
    const numbers = [1, 2, 3]

    const doubleFunc = numbers.map((number) => {
      return number * 2
    })
    console.log(doubleFunc) // [ 2, 4, 6 ]
    ```

- `filter`
  - `array.filter(callback(element[, index[, array]]))`
  - 콜백 함수의 반환 값이 참인 요소들만 모아서 새로운 배열 반환
  - 기존 배열의 요소들을 필터링할 때 유용
  - 예시
    ```js
    const numbers = [0, 1, 2]

    const positiveNumbers = numbers.filter((number) => {
      return number > 0
    })
    console.log(positiveNumbers) [ 1, 2 ]
    ```

- `reduce`
  - `array.reduce(callback(acc, element[, index[, array]]))[, initialValue]`
  - 콜백 함수의 반환 값들을 하나의 값에 누적 후 반환
  - 배열을 하나의 값으로 계산하는 동작이 필요할 때 사용
  - initialValue: 최초 callback 함수 호출 시 acc에 할당되는 값, 기본값은 배열의 첫 번째 값이다.
    - 빈 배열의 경우, initialValue를 제공하지 않으면 에러 발생
  - 예시
    ```js
    const numbers = [1, 2, 3]

    const sumNumbers = numbers.reduce((total, x) => {
      return total + x
    })
    console.log(sumNumbers) // 6
    console.log(sumNumbers / numbers.length) // 2, 평균값 구하기
    ```

- `find`
  - `array.find(callback(element[, index[, array]]))`
  - 콜백 함수의 반환 값이 참이면 해당 요소를 반환
  - 찾는 값이 배열에 없으면 undefined 반환
  - 예시
    ```js
    const numbers = [1, 2, 3]

    const findNumber = numbers.find((number) => {
      return number === 2
    })
    console.log(findNumber) // 2
    ```

- `some`
  - `array.some(callback(element[, index[, array]]))`
  - 배열의 요소 중 하나라도 판별 함수를 통과하면 참을 반환
  - 빈 배열은 항상 false 반환
  - 예시
    ```js
    const numbers = [1, 2, 3]

    const someNumber = numbers.some((number) => {
      return number === 2
    })
    console.log(someNumber) // true
    ```

- `every`
  - `array.every(callback(element[, index[, array]]))`
  - 배열의 모든 요소가 판별 함수를 통과하면 참을 반환
  - 빈 배열은 항상 true 반환
  - 예시
    ```js
    const numbers = [1, 2, 3]

    const everyNumber = numbers.every((number) => {
      return number === 2
    })
    console.log(everyNumber) // false
    ```

## 5. 객체(Object)

### 객체
- 객체는 속성의 집합이며, 중괄호 내부에 key와 value의 쌍으로 표현
- key는 문자열 타입만 가능(띄어쓰기 등의 구분자가 있으면 따옴표로 묶어서 표현)
- value는 모든 타입(함수 포함) 가능
- 객체 요소 접근은 점 또는 대괄호로 가능
  - 대괄호 접근 시 따옴표 사용
  - key 이름에 띄어쓰기 등의 구분자가 있으면 대괄호 접근만 가능
- 예시
  ```js
  const myInfo = {
    myName: '홍길동',
    age: 15,
    'phone number': '010-1234-5678'
  }

  console.log(myInfo.myName) // 홍길동
  console.log(myInfo['myName']) // 홍길동
  console.log(myInfo['phone number']) // 010-1234-5678
  console.log(myInfo.'phone number') // 오류 발생 -> 대괄호 사용
  ```

### 객체 관련 문법 (ES6 도입)
- 속성명 축약
  - 객체를 정의할 때 KEY와 할당하는 변수의 이름이 같으면 축약 가능
  - 예시
    ```js
    const books = ['book1', 'book2']
    const magazines = ['mag1', 'mag2']

    /*
    const bookShop = {
      books: books,
      magazines: magazines,
    }
    */

    const bookShop = {
      books,
      magazines,
    }
    console.log(bookShop)
    ```

- 메소드명 축약
  - 메소드 선언 시 function 키워드 생략 가능
  - 예시
    ```js
    /*
    const obj = {
      greeting: function () {
        console.log('Hi!')
      }
    }
    */

    const obj = {
      greeting() {
        console.log('Hi!')
      }
    }

    obj.greeting() // Hi!
    ```

- 계산된 속성
  - 객체를 정의할 때 key의 이름을 표현식을 이용하여 동적으로 생성 가능
  - 예시
    ```js
    const key = 'country'
    const value = ['한국', '미국']

    const myObj = {
      [key]: value,
    }

    console.log(myObj)
    ```

- 구조 분해 할당
  - 배열 또는 객체를 분해하여 속성을 변수에 쉽게 할당
  - 예시
    ```js
    const userInfo = {
      userName: '홍길동',
      userId: 1234,
      email: 'asdf@google.com'
    }

    const { userName } = userInfo // 하나씩 저장
    const { userId, email } = userInfo // 여러개 저장

    console.log(userName, userId, email)
    // 홍길동 1234 asdf@google.com
    ```

- Spread syntax (...)
  - 배열과 마찬가지로 전개구문을 사용해 객체 내부에서 객체 전개 가능
  - 예시
    ```js
    const obj = {b: 2, c: 3}
    const newObj = {a: 1, ...obj, d: 4}

    console.log(newObj)
    // { a: 1, b: 2, c: 3, d: 4 }
    ```

- JSON 변환
  - JSON과 Object는 유사한 구조를 가지고 있지만, Object는 객체 타입이고 JSON은 문자열임
  - 따라서 서로 변환 작업이 필요
  - 예시
    ```js
    const jsonData = {
      coffee: 'Americano',
      iceCream: 'Mint Choco',
    }

    // Object -> json
    const objToJson = JSON.stringify(jsonData)
    console.log(objToJson)  // {"coffee":"Americano","iceCream":"Mint Choco"}
    console.log(typeof objToJson)  // string


    // json -> Object
    const jsonToObj = JSON.parse(objToJson)  // { coffee: 'Americano', iceCream: 'Mint Choco' }
    console.log(jsonToObj)
    console.log(typeof jsonToObj)  // object
    console.log(jsonToObj.coffee)  // Americano
    ```
