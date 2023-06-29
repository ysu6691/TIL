# 다트(Dart)


## 1. Hello World!

```dart
void main() {
  print("Hello World!");
}
```


## 2. 변수

### 변수 선언

```dart
void main() {
  var name = "ysu";
  print(name); // ysu
  print("My name is " + name); // My name is ysu
  print("My name is $name"); // My name is ysu
  
  var age = 10;
  print("I'll be ${age + 1} years old next year");
  // I'll be 11 years old next year
}
```

### 변수 타입

- 정수(integer)

  ```dart
  void main() {
    int number1 = 10;
    int number2 = 3;

    print(number1 + number2); // 13
    print(number1 - number2); // 7
    print(number1 * number2); // 30
    print(number1 / number2); // 3.333...335
    print(number1 % number2); // 1
  }
  ```

- 실수(double)

  ```dart
  void main() {
    double number1 = 2.0;
    double number2 = 1.5;

    print(number1 + number2); // 3.5
    print(number1 - number2); // 0.5
    print(number1 * number2); // 3
    print(number1 / number2); // 1.333...333 
    print(number1 % number2); // 0.5
  }
  ```

- 참/거짓(boolean)

  ```dart
  void main() {
    bool var1 = true;
    bool var2 = false;
  }
  ```

- 문자열(string)

  ```dart
  void main() {
    String name = "ysu";
    print("My name is " + name);
  }
  ```

- var

  모든 타입을 선언할 수 있으며, 선언 이후 타입 변경이 불가능하다.

  ```dart
  void main() {
    var name = "ysu";
    var age = 10;
    var isMale = true;

    // name = 1; -> 에러 발생 (타입 변경 불가)

    // runtimeType을 이용해 타입을 알 수 있다.
    print(name.runtimeType); // String
    print(age.runtimeType); // int
    print(isMale.runtimeType); // bool
  }
  ```

- dynamic

  모든 타입을 선언할 수 있으며, 선언 이후 타입 변경이 가능하다.

  ```dart
  void main() {
    dynamic dynamic1 = 1;
    dynamic dynamic2 = 2;

    dynamic1 = 10;
    dynamic2 = "a";

    // runtimeType을 이용해 타입을 알 수 있다.
    print(dynamic1.runtimeType); // int
    print(dynamic2.runtimeType); // String
  }
  ```

### Nullable vs Non-nullable

- Nullable: `?`을 이용해 선언 시 null이 가능한 변수임을 명시

- Non-nullable: 변수에 `!`을 붙여 null이 아님을 명시

```dart
void main() {
  // String name1 = null; -> 에러 발생
  
  // ?을 이용해 nullable로 선언
  String? name1 = null;
  print(name1); // null
  
  String name2 = "ysu";
  // !를 이용해 non-nullable이라고 명시
  print(name2!);
  // print(name1!); -> 에러 발생
}
```

### final vs const

- `final`: 변수의 값을 추후 변경 불가 / **빌드 시 값을 알고 있지 않아도 됨**

- `const`: 변수의 값을 추후 변경 불가 / **빌드 시 값을 알고 있어야 함**

```dart
void main() {
  final String final1 = "a";
  const int const1 = 10;
  
  // final과 const 모두 값 변경 불가
  // final1 = "b";
  // const2 = 7;
  
  // 타입 생략 시 var로 선언
  final final2 = 3;
  const const2 = "b";
  print(final2.runtimeType); // int
  print(const2.runtimeType); // String
  
  final DateTime now = DateTime.now(); // final은 빌드 시 값을 몰라도 됨
  // const DateTime now = DateTime.now(); // 에러 -> const는 빌드 시 값을 알아야 됨
}
```

### enum

```dart
enum Status {
  approved,
  pending,
  rejected
}

void main() {
  Status status = Status.approved;
}
```

## 3. 연산자

- 기본 연산

  ```dart
  void main() {
    int number = 10;
    
    print(number + 2); // 12
    print(number - 2); // 8
    print(number * 2); // 20
    print(number / 2); // 5
    print(number % 3); // 1
    
    print(number++); // 10
    print(number); // 11
    
    print(++number); // 12
    print(number); // 12
    
    number += 3;
    number -= 3;
    number *= 3;
    number %= 3;
    // number /= 3; -> 에러(double 할당)
    
    int? number2 = null;
    number2 ??= 3; // 해당 변수가 null이면 오른쪽 값을 할당
    print(number2); // 3
    
    number ??= 1; 
    print(number); // number는 null이 아니므로 원래 값 출력 
  }
  ```

- 비교 연산

  ```dart
  void main() {
    int num1 = 10;
    int num2 = 20;
    
    print(num1 > num2); // false
    print(num1 < num2); // true
    print(num1 >= num2); // false
    print(num1 <= num2); // true
    print(num1 == num2); // false
    print(num1 != num2); // true
    
    print(num1 is int); // true
    print(num1 is String); // false
    print(num1 is! int); // false
  }
  ```

- 논리 연산

  ```dart
  void main() {
    bool bool1 = true;
    bool bool2 = false;
    
    print(bool1 && bool2); // false
    print(bool1 || bool2); // true
  }
  ```

## 4. Collections

### List

- List 선언

  ```dart
  void main() {
    List<String> names = ["y", "s", "u"];
    List<int> nums = [1, 2, 3];

    print(nums) // [1, 2, 3]
  }
  ```

- List 접근

  ```dart
  void main() {
    List<int> nums = [1, 2, 3];

    // 인덱스로 접근
    print(nums[0]); // 1

    // 리스트 길이
    print(nums.length); // 3

    // 삽입
    nums.add(10);
    print(nums); // [1, 2, 3, 10]

    // 삭제
    print(nums.remove(10)); // true
    print(nums.remove(100)); // false
    print(nums); // [1, 2, 3]

    // 인덱스 조회
    print(nums.indexOf(3)); // 2
    print(nums.indexOf(100)); // -1 (없는 인자면 -1 출력)

    // 인자 포함 여부
    print(nums.contains(3)); // true
    print(nums.contains(100)); // false

    // 비어 있는지 여부
    print(nums.isEmpty); // false
    print(nums.isNotEmpty); // true
  }
  ```

  ```dart
  class Person {
    String name;
    int age;

    Person(this.name, this.age);
  }

  void main() {

    Person p1 = Person("a", 10);
    Person p2 = Person("b", 12);
    Person p3 = Person("c", 15);

    List<Person> persons = [p1, p2, p3];

    // forEach (권장x -> for in 사용 권장)
    persons.forEach((person) {
      print(person.name);
    });
    // a
    // b
    // c

    // map
    Iterable<String> names = persons.map((person) {
      return person.name;
    });
    print(names); // (a, b, c)
    
    // reduce
    int totalNum = [1, 2, 3].reduce((total, n) {
      return total + n;
    });
    print(totalNum); // 6
    
    // fold
    int totalAge = persons.fold(0, (total, person) {
      return total + person.age;
    });
    print(totalAge); // 37
  }
  ```

### Map

- Map 선언

  ```dart
  void main() {
    Map<String, int> ageMap = {
      "ysu": 10,
      "xxx": 13
    };
    
    print(ageMap); // {ysu: 10, xxx: 13}
  }
  ```

- Map 접근

  ```dart
  void main() {
    Map<String, int> myMap = {};
    
    // 맵 요소 추가
    myMap.addAll({"a": 1, "b": 2});
    print(myMap);
    
    // key로 접근
    print(myMap["a"]); // 1
    print(myMap["d"]); // null
    
    // 새로운 key, value 추가
    myMap["c"] = 3;
    print(myMap); // {a: 1, b: 2, c: 3}
    
    // key 제거
    print(myMap.remove("c")); // 3
    print(myMap.remove("d")); // null
    print(myMap); // {a: 1, b: 2}
    
    // key, value 조회
    print(myMap.keys); // (a, b)
    print(myMap.values); // (1, 2)
    print(myMap.keys.runtimeType); // LinkedHashMapKeyIterable<String>
    
    // Map 길이 조회
    print(myMap.length); // 2
    
    // key, value 포함하는지 조회
    print(myMap.containsKey("a")); // true
    print(myMap.containsValue(3)); // false
    
    // Map 비어 있는지 조회
    print(myMap.isEmpty); // false
    print(myMap.isNotEmpty); // true

    // key, value 쌍 조회 (entries)
    myMap.entries.forEach((entry) {
      print("key: ${entry.key}, value: ${entry.value}");
    });
    // key: a, value: 1
    // key: b, value: 2
  }
  ```

  ```dart

  ```

### Set

- Set 선언

  ```dart
  void main() {
    Set<int> ages = {1, 2, 3};
    print(ages); // {1, 2, 3}
  }
  ```

- Set 접근

  ```dart
  void main() {
    Set<int> ages = {1, 2, 3};
    
    // 원소 추가
    ages.add(4);
    print(ages); // {1, 2, 3, 4}
    
    // 원소 제거
    print(ages.remove(4)); // true
    print(ages.remove(100)); // false
    print(ages); // {1, 2, 3}
    
    // 길이 조회
    print(ages.length); // 3
    
    // Set 비어 있는지 조회
    print(ages.isEmpty); // false
    print(ages.isNotEmpty); // true
    
    // 원소 포함하는지 조회
    print(ages.contains(1)); // true
  }
  ```

## 5. 조건문

### if

```dart
void main() {
  int num = 10;
  
  if (num < 5) {
    print("a");
  } else if (num < 15) {
    print("b");
  } else {
    print("c");
  }  
  // b
}
```

### switch

```dart
void main() {
  int num = 10;

  switch (num % 3) {
    case 0:
      print("나머지: 0");
      break;
    
    case 1:
      print("나머지: 1");
      break;
        
    default:
      print("나머지: 2");
  }
  // 나머지: 1
}
```

## 6. 반복문

### for

```dart
void main() {

  for (int i=0; i<=3; i++) {
    print(i);
  }
  // 0
  // 1
  // 2
  // 3

  List<int> nums = [1, 2, 3];
  for (int num in nums) {
    print(num);
  }
  // 1
  // 2
  // 3
}
```

### while

```dart
void main() {
  int cnt = 0;
  while (cnt < 3) {
    cnt += 1
    print(cnt)
  }
  // 1
  // 2
}
```

### do while

```dart
void main() {
  int cnt = 0;
  do {
    cnt += 1;
    print(cnt);
  } while (cnt < 3);
  // 1
  // 2
  // 3
}
```

## 7. 함수

### 함수의 사용

```dart
void main() {
  int addNums(int a, int b) {
    return a + b;
  }
  
  print(addNums(1, 2));
}
```

### Optional Parameter

```dart
void main() {
  int addNums(int a, [int? b, int c = 20]) {
    if (b != null) {
      return a + b + c;
    }
    return a + c;
  }
  
  print(addNums(1)); // 21
  print(addNums(1, 2)); // 23
  print(addNums(1, 2, 3)); // 6
}
```

### Named Parameter

중괄호 내부에 입력받을 파라미터를 적어 named parameter를 명시할 수 있다.

기본적으로 optional이므로, null을 허용하거나 값을 미리 설정해야 한다.

`required` 키워드를 사용해 필수로 넣어야 하는 파라미터를 명시할 수 있다.

```dart
void main() {
  int addNums({int a = 10, int? b}) {
    if (b == null) {
      return a;
    }
    return a + b;
  }
  
  print(addNums(a: 1, b: 3)); // 4
}
```

```dart
void main() {
  int addNums({required int a, int b = 10}) {
    return a + b;
  }
  
  print(addNums(a: 1)); // 3
}
```

### Arrow Function

```dart
void main() {
  int addNums(int a, int b) => a + b;
  
  print(addNums(1, 2)); // 3
}
```

## 8. Typedef

```dart
typedef Operation = int Function(int x, int y);

int add(int x, int y) => x + y;

int substract(int x, int y) => x - y;

int calculate(int x, int y, Operation operation) => operation(x, y);

void main() {
  print(calculate(5, 3, add)); // 8
  print(calculate(5, 3, substract)); // 2
}
```

## 9. Class

### 클래스 기본

```dart
class Person {
  String name;
  int age;
  static double? avgHeight;

  // constructor
  Person(String name, int age)
      : this.name = name, // this 생략 가능
        this.age = age; // this 생략 가능

  void sayName() {
    print("My name is ${this.name}"); // this 생략 가능
  }

  void sayAvgHeight() {
    print("Average height is ${avgHeight}cm");
  }
}

void main() {
  // new는 생략 가능
  Person person = new Person("ysu", 20);
  person.sayName(); // My name is ysu
  print(person.age); // 20

  // static
  Person.avgHeight = 170;
  person.sayAvgHeight(); // Average height is 170cm
}
```

### Constructor

```dart
class Person {
  String name;
  int age;

  // 더 간단한 형식으로 생성자를 작성할 수 있다.
  Person(this.name, this.age);

  // Named constructors
  // 이름을 가진 생성자를 이용해 생성할 수도 있다.
  Person.byMap(
    Map input,
  )   : name = input["name"],
        age = input["age"];
}

void main() {
  Person p1 = Person("ysu", 20);
  Person p2 = Person.byMap({"name": "ysu", "age": 20});
}
```

### Getter & Setter

```dart
class Person {
  // private으로 선언
  String _name;

  Person(String name) : _name = name;

  // Getter
  String get name {
    return _name;
  }
  // 화살표 함수 사용
  // String get name => _name;

  // Setter
  set name(String name) {
    _name = name;
  }
}

void main() {
  Person p1 = Person("ysu");

  // get
  print(p1.name); // ysu
  // print(p1._name); -> 다른 클래스에서 접근 불가

  // set
  p1.name = "xxx";
  print(p1.name); // xxx
}
```

### 상속

```dart
class Person {
  String name;

  Person(this.name);

  void sayName() {
    print("저는 $name 입니다.");
  }
}

class Student extends Person {
  // super를 이용해 생성자 작성
  Student(super.name);
}

void main() {
  Person p1 = Person("ysu");
  Student s1 = Student("xxx");

  p1.sayName(); // 저는 ysu 입니다.
  s1.sayName(); // 저는 xxx 입니다.
}
```

### Overriding

```dart
class Person {
  String name;

  Person(this.name);

  void sayName() {
    print("저는 $name 입니다.");
  }
}

class Student extends Person {
  Student(super.name);

  // 데코레이터 작성
  @override
  void sayName() {
    print("저는 $name 학생입니다.");
  }
}

void main() {
  Person p1 = Person("ysu");
  Student s1 = Student("xxx");

  p1.sayName(); // 저는 ysu 입니다.
  s1.sayName(); // 저는 xxx 학생입니다.
}
```

### interface

다트의 인터페이스는 모두 클래스로 구현 가능하다.

```dart
// 인터페이스
class Animal {
  // 인터페이스는 생성자가 없어, 변수 할당을 미루거나 미리 정의해야한다.
  late String name;
  void bark() {}
}

class Dog implements Animal {
  @override
  String name;

  Dog(this.name);

  @override
  void bark() {
    print("멍멍");
  }
}

void main() {
  Dog dog1 = Dog("바둑이");
  dog1.bark(); // 멍멍
}
```

### Cascade

```dart
class Person {
  String name;
  
  Person(this.name);
  
  void sayHello(){
    print("Hello");
  }
  
  void sayName(){
    print("My name is $name");
  }
 
}

void main() {
  Person person = Person("ysu");
  person
    ..sayHello()
    ..sayName();
  
  // Hello
  // My name is ysu
}
```