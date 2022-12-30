# TypeScript 기본

## 1. 타입스크립트를 쓰는 이유

타입스크립트는 자바스크립트에 타입을 부여한 언어이다.

타입스크립트는 자바스크립트와 달리 브라우저에서 실행하려면 컴파일 과정이 필요하다.

타입스크립트를 사용하는 이유는 다음과 같다.

- 에러의 사전 방지

  코드를 작성하다 보면 기존에 개발자가 의도한 방식대로 동작할 수 있다.

  자바스크립트는 어떠한 오류나 힌트도 알려주지 않기 때문에 이러한 상황을 모르고 지나칠 것이다.

  <img src="https://user-images.githubusercontent.com/109272360/209923198-95b7abde-434f-4e9f-ab83-4228d3db9fb5.PNG" width="300px">

  하지만 타입스크립트를 사용하면 의도하지 않은 코드의 동작을 예방할 수 있다.

  <img src="https://user-images.githubusercontent.com/109272360/209923203-340af664-d76a-4156-99f4-4785b0c429b7.PNG" width="450px">

- 코드 자동 완성과 가이드

  Visual Studio Code와 같은 개발 툴의 기능을 최대로 활용할 수 있다.

  위와 같은 예시에서 함수의 결괏값을 이용해 코드를 작성해야 하는 상황이 발생할 수 있다.

  이때 개발자는 함수의 결과를 예상하고 그 타입을 `number`라고 가정한 상태에서 코드를 작성할 것이다.

  자바스크립트는 이러한 경우에도 결괏값의 타입이 `number`인 것을 인지하지 못한다.

  하지만 타입스크립트는 타입을 인지하고 그 타입에 대한 API를 미리 보기로 띄워주고, 빠르고 정확하게 코드를 작성할 수 있다.
  

## 2. 기본 타입

타입스크립트는 초기에 선언한 변수의 타입과 다른 타입의 값을 할당하면 에러를 나타낸다.

```ts
let name: string = 'ysu'
name = 1 // 에러를 알려줌
```

지정할 수 있는 타입은 다음과 같다.

### String
```ts
const name: string = 'ysu'
```

### Number
```ts
const age: number = 26
```

### Boolean
```ts
const isAdult: boolean = true
```

### Array
```ts
// 1.
const myArray1: number[] = [1, 2, 3]

// 2.
const myArray2: Array<number> = [1, 2, 3]

// 지정한 타입과 다른 타입을 추가하면 에러를 발생시킨다.
myArray1.push('a')
```

### Tuple
```ts
const myTuple: [string, number] = ['a', 1]

// 지정한 위치에 다른 타입을 넣으면 에러를 발생시킨다.
myTuple[0] = 1
```

### Any
```ts
// 모든 타입에 대해서 허용할 때 any를 사용한다.

let a: any = 1
a = 'a' // 에러 발생 x
```

### Void
```ts
// 함수가 아무것도 반환하지 않을 때 void를 사용한다.

function sayHello(): void {
  console.log(Hello)
}
```

### Never
```ts
// 함수가 항상 에러를 반환하거나 무한 루프를 돌 때 never를 사용한다.

function showError(): never {
  throw new Error()
}

function infLoop(): never {
  while (true) {
    // 무한 루프
  }
}
```

### enum
- 특정 값들의 집합을 의미하는 자료형이다.
- 숫자형 enum과 문자형 enum이 존재한다.
- 한 enum에 문자와 숫자를 혼합하여 생성할 수는 있지만 권장하지는 않는다.

- 숫자형 enum
  ```ts
  // 각 key마다 숫자를 지정한다.
  enum NumberEnum1 {
    first = 10,
    second = 20,
    third = 30,
  }

  // 초기 하나만 지정하면 나머지는 1씩 증가한다.
  enum NumberEnum2 {
    first = 10,
    second, // 11
    third // 12
  }

  // 초기 값도 지정하지 않으면 0부터 시작한다.
  enum NumberEnum3 {
    first, // 0
    second, // 1
    third // 2
  }

  // key로 value를 얻을 수 있고 value로 key를 얻을 수 있다.
  console.log(NumberEnum1.first) // 10
  console.log(NumberEnum1['first']) // 10
  console.log(NumberEnum1[10]) // first
  console.log(NumberEnum1.10) // 이 방식은 에러 발생
  ```

- 문자형 enum
  ```ts
  // 각 key마다 문자열을 지정한다.
  enum StringEnum1 {
    first = '가',
    second = '나',
    third = '다'
  }

  // 문자형 enum일 때 아무 값도 지정하지 않으면 에러가 발생한다.
  enum StringEnum2 {
    first = '가'
    second,
    third,
  }

  // key로는 value를 얻을 수 있지만 value로는 key를 얻을 수 없다.
  console.log(StringEnum1.first) // 가
  console.log(StringEnum1['first']) // 가
  console.log(StringEnum1['가']) // 에러 발생
  console.log(StringEnum1.'가') // 에러 발생
  ```

### Null, Undefined
```ts
const a: null = null
const b: undefined = undefined
```

## 3. 인터페이스

인터페이스는 상호 간에 정의한 약속 혹은 규칙을 의미한다.

```ts
const User :object = {
  name: 'ysu',
  age: '26',
}

User.name = 1
console.log(User.name) // 1
```

위 코드에서 객체의 key에 접근하려 하면 에러를 발생시킨다.

또한 초기에 문자열로 지정한 `name`을 숫자로 바꿔도 에러 없이 바뀌어 버린다.

이는 각 key의 타입을 지정하지 않았기 때문이다.

따라서 **인터페이스**를 이용해 객체, 함수, 클래스 등을 구현하면 초기 지정한 타입을 쉽게 유지할 수 있다.

### 객체 구현
```ts
interface User {
  name: string
  age: number
}

const myUser: User = {
  name: 'ysu',
  age: 26
}

console.log(myUser.name) // ysu
myUser.age = 'a' // 에러 발생
```

### 옵션 속성
```ts
interface User {
  name: string
  age: number
  // ?를 이용해 받아도 되고 받지 않아도 되는 key인 것을 알려준다.
  gender?: string
}

// gender 없이도 객체가 잘 생성된다.
const myUser: User = {
  name: 'ysu',
  age: 26
}

// 이후에 추가 가능
myUser.gender = 'male'
console.log(myUser.gender)
```

### 객체의 Read only 속성
```ts
interface User {
  name: string
  age: number
  // 변경이 불가능한 key인 것을 알려준다.
  readonly gender: string
}

const myUser: User = {
  name: 'ysu',
  age: 26,
  gender: 'male'
}

myUser.gender = 'female' // 에러 발생
```

### 정의하지 않은 속성 사용
```ts
interface User {
  name: string
  age: number
  // [아무 문자열: key 타입]: value 타입
  [propName: number]: string
  // 모든 타입의 value 저장하기
  // [propName: string]: any
}

const myUser: User = {
  name: 'ysu',
  age: 26,
  // number: string 쌍의 요소를 계속 추가 가능
  1: 'A',
  2: 'B'
}
```
```ts
// type 별칭을 이용해 value를 제한할 수 있다.
type Score = 'A' | 'B' | 'C' | 'F'

interface User {
  name: string
  age: number
  // A, B, C, F에 해당하는 값만 value로 받을 수 있다.
  [grade: number]: Score
}

const myUser: User = {
  name: 'ysu',
  age: 26,
  1: 'A', 
  2: 'a' // 에러 발생
}
```

### 함수의 정의
```ts
interface Add {
  // (인자들 타입 작성): return 타입 작성
  (num1: number, num2: number): number
}

const add: Add = function (x, y) {
  return x + y
  // console.log(x + y) -> 에러 발생
  // Add 인터페이스의 return 타입을 void로 해야 함
}

console.log(add(10, 20))
console.log(add('10', 20)) // 에러 발생
```

### 클래스 정의
```ts
interface Car {
  color: string
  wheels: number
  start(): void
}

class Bmw implements Car {
  color
  constructor (c: string) {
    this.color = c
  }
  wheels = 4
  start() {
    console.log('Go')
  }
}

const myCar = new Bmw('red')
console.log(myCar.color) // red
myCar.start() // Go
```

### 인터페이스 확장
```ts
interface Person {
  name: string
}

interface Developer extends Person {
  skill: string
}

const p1: Developer = {
  name: 'ysu',
  skill: 'typescript'
}
```

```ts
// 여러 개 확장도 가능
interface Car {
  color: string
  wheels: number
}

interface Toy {
  name: string
}

interface ToyCar extends Car, Toy {
  price: number
}
```

## 4. 함수

### 함수의 정의

- 함수는 인자의 타입과 반환 값의 타입을 정할 수 있다.

  ```ts
  function add(num1: number, num2: number): number {
    return num1 + num2
  }
  ```

- 아무 것도 반환하지 않을 때는 `void`를 사용한다.
  ```ts
  function add(num1: number, num2: number): void {
    console.log(num1 + num2)
  }
  ```

### 함수의 인자

- 함수를 호출할 때는 지정한 함수의 인자를 필수로 넘겨주어야 한다.

  ```ts
  function add(num1: number, num2: number): number {
    return num1 + num2
  }

  console.log(add(1)) // 에러 발생
  console.log(add(1, 2)) // 3
  console.log(add(1, 2, 3)) // 에러 발생
  ```

- 받아도 되고 안 받아도 되는 인자는 `?`를 사용한다.
  ```ts
  function add(num1: number, num2?: number): number {
    return num1 + num2
  }
  console.log(add(1, 2)) // 30
  console.log(add(1, 2, 3)) // 에러 발생
  console.log(add(1)) // NaN (에러 없음)
  ```

- 매개변수 여러 타입 지정하기
  ```ts
  function add (num1: number, num2: number | undefined): number {
    if (num2 !== undefined) {
      return num1 + num2
    } else {
      return num1
    }
  }

  console.log(add(1, 2)) // 3
  console.log(add(1, undefined)) // 1
  ```

- 매개변수 초기화는 ES6 문법과 동일하다
  ```ts
  function add(num1: number, num2 = 10): number {
    return num1 + num2
  }

  console.log(add(1, undefined)) // 11
  console.log(add(1)) // 11
  console.log(add(1, 2)) // 3
  ```

- Rest 문법이 적용된 매개변수
  ```ts
  function add(...nums: number[]): number {
    return nums.reduce((result, num) => {return result + num}, 0)
  }
  
  console.log(add(1)) // 1
  console.log(add(1, 2, 3, 4)) // 10
  ```

- 매개변수를 특정 값으로 지정
  ```ts
  function myFunc (myParam: 1|'a') {
    return myParam
  }

  console.log(myFunc(1)) // 1
  console.log(myFunc('a')) // a
  console.log(myFunc(2)) // 에러
  ```

### 인터페이스와의 사용

```ts
interface User {
  name: string
  age: number
}

function join(name: string, age: number): User {
  return {
    name,
    age
  }
}

const user1: User = join("홍길동", 20)
const user2: User = join("김민수", "31") // 에러 발생

console.log(user1)
// {
//   "name": "홍길동",
//   "age": 20
// }
```

위 예시에서 사용자가 나이를 문자열로 입력하려 한다면 에러가 발생한다.

나이를 문자열로 입력한 경우 `나이는 숫자로 입력해주세요.`를 리턴해보자.

```ts
interface User {
  name: string
  age: number
}

function join(name: string, age: number | string): User | string {
  if (typeof age === 'number') {
    return {
      name,
      age
    }
  } else {
    return '나이는 숫자로 입력해주세요.'
  }
}

const user1: User = join("홍길동", 20) // 에러 발생
const user2: string = join("김민수", "31") // 에러 발생
```

위의 코드에서 `user1`과 `user2` 모두 에러가 발생한다.

이는 `join` 함수가 `User`와 `string` 타입을 모두 반환할 수 있는 함수이기 때문이다.

따라서 이러한 경우 **오버로드**를 사용한다.

```ts
interface User {
  name: string
  age: number
}

// 오버로드
function join(name: string, age: number): User
function join(name: string, age: string): string
function join(name: string, age: number | string): User | string {
  if (typeof age === 'number') {
    return {
      name,
      age
    }
  } else {
    return '나이는 숫자로 입력해주세요.'
  }
}

const user1: User = join("홍길동", 20)
const user2: string = join("김민수", "31")
```

### this
