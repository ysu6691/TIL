# TypeScript 기본

- [The TypeScript Handbook](https://www.typescriptlang.org/ko/docs/handbook/intro.html)
- [TS Playground](https://www.typescriptlang.org/play)

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
  
## 2. 타입스크립트 사용법

### 타입스크립트 설치
```bash
$ npm init -y
```
```bash
$ npm install typescript
```

### 타입스크립트 컴파일
```bash
# 자바스크립트 파일 자동 생성
$ npx tsc 파일명
```

## 3. 기본 타입

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
```ts
// 배열 내에 객체 넣기
let people: {
  name: string
  age: number
}[]

people = [
  {
    name: '홍길동',
    age: 26
  },
  {
    name: '김민수',
    age: 20
  }
]
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

### Type Aliases

`type`을 이용해 새로운 타입을 정의할 수 있다.

`type`은 자바스크립트에는 없는 기능이기 때문에, 컴파일 이후에는 자바스크립트 코드에서 사라진다.

```ts
// type Aliases를 이용해 한 번의 타입 정의로 여러 번 사용할 수 있다.
type Person = {
  name: string
  age: number
}

const person1: Person = {
  name: '홍길동',
  age: 24
}
```

> **참고: 타입 추론**
>
> 위 예시들에서 변수를 선언할 때 다양한 타입을 명시적으로 표기했다.
>
> 하지만 타입스크립트는 명시적으로 타입을 표기하지 않아도 초기 선언한 타입을 기억한다.
>
> 이를 타입 추론이라고 한다.
>
> ```ts
> let name = 'ysu'
>
> name = 123 // 초기 선언한 문자열과 달라 에러 발생
> ```
>
> 따라서 명시적으로 적어주는 불필요한 작업 없이 타입 추론을 사용하는 것이 권장되는 방식이다.


## 4. 인터페이스

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

> **참고: Interface vs Type Alias**
>
> `Type Alias`와 `Interface` 모두 여러 타입의 속성으로 이루어진 새로운 타입을 정의할 수 있다.
>
> ```ts
> interface Person {
>   name: string
>   age: number
> }
> ```
> ```ts
> type Person = {
>   name: string
>   age: number
> }
> ```
> 
> 둘의 가장 큰 차이점은 **`interface`는 확장이 가능하다는 것이고 `type`은 확장이 불가능하다는 것**이다.
>
> `interface`는 `extends`를 사용해 확장이 가능한데, 다음과 같이 재선언으로 확장할 수도 있다.
>
> ```ts
> interface Person {
>   name: string
> }
> 
> interface Person {
>   age: number
> }
> 
> const person1: Person = {
>   name: 'ysu',
>   age: 26
> }
> ```
>
> 하지만 `type`은 불가능하다.
>
> ```ts
> type Person = {
>   name: string
> }
> 
> // 에러 발생
> type Person = {
>   age: number
> }
> ```
>
> 따라서 확장에 용이한 `interface`의 사용이 권장된다.

## 5. 함수

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

this가 가리키는 것을 명시하려면 다음과 같이 작성해야 한다.

```ts
function 함수명(this: 타입) {
  //
}
```

실제 예제에 적용해보면 다음과 같다.

```ts
interface User {
  name: string
  age: number
  sayHello(this: User): void
}

const user1: User = {
  name: 'ysu',
  age: 26,
  // 여기에서 this 타입을 명시해도 됨
  sayHello: function () {
    console.log(`Hello, my name is ${this.name}`)
  }
}

user1.sayHello() // Hello, my name is ysu
```

## 6. Union 타입 & Intersection 타입

### Union 타입

`|` 연산자를 이용해 타입을 여러 개 연결하는 방식을 유니온 타입 정의 방식이라고 한다.

```ts
function myFunc(text: string | number) {
  // ...
}
```

유니온 타입을 사용할 때 주의할 점이 있다.

```ts
interface Person {
  name: string
  age: number
}

interface Developer {
  name: string
  skill: string
}

function introduce(someone: Person | Developer) {
  console.log(someone.name) // 정상 동작
  console.log(someone.age) // 에러 발생
  console.log(someone.skill) // 에러 발생
}
```

위 코드에서 `introduce` 함수의 `someone` 인자는 두 개의 타입을 가질 수 있다.

하지만 `age`와 `skill`은 두 타입 중 하나의 타입만 가질 수 있는 속성이므로 에러가 발생한다.

따라서 `introduce` 함수 안에서는 두 타입의 공통 속성인 `name`에만 접근할 수 있다.

### Intersection 타입

`&` 연산자를 이용해 여러 개의 타입 정의를 하나로 합치는 방식을 인터섹션 타입 정의 방식이라고 한다.

```ts
interface Person {
  name: string
  age: number
}

interface Developer {
  name: string
  skill: string
}

const myUser: Person & Developer = {
  name: 'ysu',
  age: 26,
  skill: 'typescript'
}
```

## 7. Generics

재사용성이 높은 요소를 만들 때 자주 활용되는 특성이다.

한가지 타입보다 여러 가지 타입에서 동작하는 요소를 생성하는데 사용된다.

### 함수와의 사용

```ts
function getSize(arr: number[] | string[] | boolean[]): number {
  return arr.length
}

const arr1 = [1, 2, 3]
console.log(getSize(arr1))

const arr2 = ['a', 'b', 'c']
console.log(getSize(arr2))

const arr3 = [true, false]
console.log(getSize(arr3))

// ...
```

위 코드에서는 유니온 타입을 사용해 `getSize` 함수의 인자로 여러 타입을 지정했다.

만약 다른 타입을 추가하고 싶을 때는 해당 타입을 추가로 또 적어줘야 할 것이다.

이는 제네릭을 이용해 함수 사용 시 그 타입을 지정하도록 할 수 있다.

```ts
// T: 타입파라미터 (주로 T를 사용)
// 전달받은 타입을 어떤 요소에 사용할지 결정
function getSize<T>(arr: T[]): number {
  return arr.length
}

// 각 배열마다 타입을 명시
const arr1 = [1, 2, 3]
console.log(getSize<number>(arr1))

const arr2 = ['a', 'b', 'c']
console.log(getSize<string>(arr2))

const arr3 = [true, false]
console.log(getSize<boolean>(arr3))

// getSize(arr1)과 같이 사용해도 타입 추론에 의해 에러가 나지 않기 때문에 이렇게도 흔히 사용한다.
// 만약 복잡한 코드에서 타입 추정이 되지 않는다면 위와 같이 타입을 명시하는 것이 좋다.
```

### 인터페이스와의 사용

```ts
interface Mobile<T> {
  name: string
  price: number
  option: T
}

const m1: Mobile<null> = {
  name: 'm1',
  price: 100,
  option: null
}

const m2: Mobile<object> = {
  name: 'm2',
  price: 120,
  option: {
    color: 'red'
  }
}
// 다음과 같이 객체 형태로 타입 파라미터를 지정할 수도 있다.
// option의 value에 들어가는 객체의 key 하나씩 타입을 지정한다.
// const m2: Mobile<{color: string}> = {
// ...
// }
```

### 제네릭 제약 조건: extends

```ts
interface User {
  name: string
  age: number
}

interface Book {
  price: number
}

// data에 에러 발생
function showName(data): string {
  return data.name
}

const myUser: User = {
  name: 'ysu',
  age: 26
}

const myBook: Book = {
  price: 100
}

console.log(showName(myUser)) // ysu
console.log(showName(myBook)) // undefined
```

위 코드에서 `showName`의 인자인 `data`의 타입을 지정하지 않아 에러가 발생했다.

`any`를 사용할 수도 있지만 이럴 경우 `name` 속성을 가지고 있지 않은 타입이 `showName` 함수를 실행하는 것을 막지 못하게 된다.

따라서 이러한 경우에도 제네릭을 사용할 수 있다.

```ts
// ...
function showName<T>(data: T): string {
  return data.name
}
// ...
```

위와 같이 코드를 수정하면 이번에는 `name`에 에러가 발생한다.

이는 어떤 타입이 올지 모르는데 `name` 속성을 사용하고 있기 때문이다.

이러한 상황에서는 `extends`를 사용해 `T` 타입이 `name` 속성을 가지고 있게 설정할 수 있다.

```ts
interface User {
  name: string
  age: number
}

interface Book {
  price: number
}

// T 타입이 문자열 타입의 name 속성을 가지고 있도록 설정
function showName<T extends {name: string}>(data: T): string {
  return data.name
}

const myUser: User = {
  name: 'ysu',
  age: 26
}

const myBook: Book = {
  price: 100
}

console.log(showName(myUser)) // ysu
console.log(showName(myBook)) // 에러 발생
```

위와 같이 `extends`를 사용하면 문자열 타입의 `name` 속성이 없는 요소는 `showName` 함수 사용시 에러가 발생한다.

이처럼 `extends`를 사용해 제네릭 사용 시 **제약 조건**을 걸어 에러를 발생시킬 수 있다.


> 참고1: `any`의 사용
> 
> 제네릭은 어떤 타입이 올지 모르는 경우 여러 타입을 허용하기 위해 사용했다.
>
> 사실은 `any`를 사용해도 이러한 기능을 더 간편하게 사용할 수는 있다.
>
> 하지만 `any`는 타입에 대한 어떤 힌트나 에러도 알려주지 않으므로, 타입스크립트가 가진 장점을 살릴 수 없다는 문제가 있다.

