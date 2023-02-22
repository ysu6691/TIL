# 자바 객체지향 프로그래밍

## 1. 객체지향 프로그래밍

### 객체지향 프로그래밍
- 객체: 사물과 같이 유형적인 것과 개념이나 논리와 같은 무형적인 것들
- 객체 모델링: 현실세계의 객체를 SW 객체로 설계하는 것
- 객체지향 프로그래밍: 프로그램을 수많은 '객체'라는 기본 단위로 나누고 이들의 상호작용으로 서술하는 방식
- 객체지향 프로그래밍 특징
  - 추상화
  - 다형성
  - 상속
  - 캡슐화

## 2. 함수
### 함수의 선언과 사용
```java
public class FunctionTest {
    public static void main(String[] args) {
        System.out.println("아침 먹기");
        study(); // 함수의 사용
        System.out.println("점심 먹기");
    }
    
    // 함수의 선언
    public static void study() {
        System.out.println("자바 공부");
        System.out.println("파이썬 공부");
    }
}
```

### 파라미터의 사용
```java
public class FunctionTest {
    public static void main(String[] args) {
        System.out.println("아침 먹기");
        move("학교", "셔틀"); // parameter 전달
        System.out.println("점심 먹기");
    }
  
    // argument(인자) 설정
    public static void move(String location, String method) {
        System.out.println(location + "(으)로 " + method + "(을)를 이용해 이동");
    }
}
```

### return의 사용
- `return`값이 없으면 `void`
- `return`값이 있다면 해당 타입 명시
- `return` 타입은 하나만 적용 가능

```java
public class FunctionTest {
    public static void main(String[] args) {
        System.out.println(add(1, 2)); // 3
    }
    
    public static int add(int num1, int num2) {
        return num1 + num2;
	  }
}
```

### 메소드 오버로딩
- 함수명이 같아도 받는 인자가 다르면 여러 메소드를 정의할 수 있다.
- 중복 코드에 대한 효율적인 관리가 가능하다.
- 파라미터의 개수나 순서, 타입이 달라야 한다. (이름만 다른 것은 x)
```java
public class FunctionTest2 {
    public static void main(String[] args) {
        System.out.println(add(1, 2)); // 3
        int[] nums = new int[] {1, 2, 3};
        System.out.println(add(nums)); // 3
    }
    
    public static int add(int num1, int num2) {
        return num1 + num2;
    }
    
    public static int add(int[] nums) {
        int acc = 0;
        for (int num: nums) {
          acc += num;
        }
        return acc;
    }
}
```

## 3. 클래스
- 다양한 자료형을 가질 수 있는 **사용자정의 자료형**이다.
- 클래스는 객체를 생성하는 틀의 역할을 한다.
- 각 객체가 어떤 특징(속성과 동작)을 가지고 있을지 결정한다.
- 클래스를 통해 생성된 객체를 인스턴스라고 한다.
- 클래스 내부에서 선언된 함수를 메소드라고 하며, 메소드를 이용해 객체들끼리 상호작용할 수 있다.

### 클래스 구성
- **속성(Attribute)**: 필드
- **동작(Behavior)**: 메소드
- **생성자(Constructor)**: 인스턴스를 생성하는 호출 메소드

### 클래스 선언
- 접근제한자: `public` / `default`
- 활용제한자: `final` / `abstract`
```java
[접근제한자] [활용제한자] class 클래스명 {
    속성 정의
    기능 정의
    생성자
}
```

```java
public class Person {
    String name;
    int age;
    String hobby;
    public static void getInfo(String name, int age, String hobby) {
        System.out.println("이름: " + name + ", 나이: " + age + ", 취미: " + hobby);
    }
}
```

### 클래스와 변수

- **클래스 변수**
  - 클래스 영역 선언 (static 키워드)
  - 생성시기: 클래스가 메모리에 올라 갔을 때
  - 모든 인스턴스가 공유하며 클래스에서 직접 접근 가능
  - 예시
    ```java
    public class Person {
        static int personCount; // 초기값: 0
    }
    ```

- **인스턴스 변수**
  - 클래스 영역 선언
  - 생성시기: 인스턴스가 생성되었을 때 (new)
  - 인스턴스별로 생성됨
  - 예시
    ```java
    public class Person {
        String name;
        int age;
    }
    ```

- **지역 변수**
  - 클래스 영역 이외 (메소드, 생성자 등)
  - 생성시기: 선언되었을 때 
  - 외부에서 접근이 불가하며 영역을 벗어나면 소멸

- 예시
  ```java
  // Person.java

  package classTest;

  public class Person {
      static int personCount;
      String name;
      int age;
      String hobby;
  }
  ```

  ```java
  // PersonTest.java

  package classTest;

  public class PersonTest {
      public static void main(String[] args) {
          // 인스턴스 생성
          Person p1 = new Person();
          
          // 인스턴스 속성 저장
          p1.name = "Yun";
          p1.age = 27;
          p1.hobby = "Youtube";
          
          // 인스턴스 변수 출력
          System.out.println(p1.name); // Yun
          
          // 클래스 변수 출력
          System.out.println(Person.personCount); // 0
          System.out.println(p1.personCount); // 0 (가능은 하지만 클래스에서 직접 접근할 것을 권장)
      }
  }
  ```

### 메소드
- 객체가 할 수 있는 행동을 함수형으로 정의
- **camelCase**로 작성하는 것이 관례
- 메소드 선언
  ```java
  // 접근제한자: public / protected / default / private
  [접근제한자] [활용제한자] 반환값 메소드이름([매개변수들]) {
      행위들
  }

  // 예시
  public static void main(String [] args ) {
      ...
  }
  ```

- 메소드 호출
  ```java
  // 클래스 예시
  public class Person {
      public void info() {
          ...
      }

      public static void hello() {
          ...
      }
  }
  ```
  - `static`이 메소드에 선언되어 있지 않은 경우: `인스턴스.메소드명()`으로 호출
    ```java
    Person p = new Person();
    p.info();
    ```

  - `static`이 메소드에 선언되어 있는 경우: `클래스명.메소드명()`으로 호출
    ```java
    Person.hello();
    ```

## 4. JVM 메모리 구조

### JVM 메모리 구조

JVM이 관리하는 메모리 공간은 크게 3가지 영역으로 나눌 수 있다.

- **메소드(스태틱) 영역**: 모든 쓰레드가 공유하는 메모리 영역이다. 클래스, 인터페이스, 메소드, 필드, static 변수 등의 바이트 코드가 보관된다.
- **힙 영역**: 모든 쓰레드가 공유하며 동적할당자 `new` 키워드로 생성된 객체(인스턴스)와 배열이 할당되는 영역이다. GC에 의해 자동 초기화가 진행된다.
- **스택 영역**: 지역 변수, 매개 변수(=파라미터), 연산 중 발생하는 임시 데이터가 할당되는 영역으로 초기화가 진행되지 않는다.


JVM 메모리 구조의 동작 원리를 간단히 살펴보면 다음과 같이 나타낼 수 있다.

<img src="https://user-images.githubusercontent.com/109272360/215331596-55ce80c6-fcce-431c-a062-d36ffe7da95b.png" width="450px" style="margin: 16px 0">
<img src="https://user-images.githubusercontent.com/109272360/215331605-3c06f7fe-6ed3-4158-aa98-1165b5f24ce5.png" width="850px" style="margin: 16px 0">
<img src="https://user-images.githubusercontent.com/109272360/215331615-3e53b84a-e5c2-498a-9578-1a1bc4c28330.png" width="850px" style="margin: 16px 0">
<img src="https://user-images.githubusercontent.com/109272360/215331623-34fdbec3-fad0-48b1-9dd4-af7f5be6554c.png" width="850px" style="margin: 16px 0">


### Garbage Collection
- 자바는 메모리 관리를 개발자 대신 **GC**(Garbage Collection)가 관리
- 힙 영역 (메소드 영역 포함)에 생성된 메모리 관리 담당
- 더 이상 사용되지 않는 객체들을 점검하여 제거
- JVM에 의해서 자동적으로 실행

### static

- static과 non-static
  - static: 클래스 멤버
  - non-static: 객체 멤버

- 로딩 시점
  - static: 클래스 로딩 시
  - non-static: 객체 생성 시

- 메모리상의 차이
  - static: 클래스 당 하나의 메모리 공간만 할당
  - non-static: 인스턴스 당 메모리가 별도로 할당

- 문법적 특징
  - static: 클래스 이름으로 접근
  - non-static: 객체 생성 후 접근

- 접근
  - static 영역에서는 non-static 영역에 직접 접근이 불가능 (non-static 멤버는 객체가 생성되어야 메모리가 할당됨)
    ```java
    public class Main {
        String str = "문장";

        public static void main(String[] args) {
            System.out.println(str); // 에러
        }
    }
    ```

  - non-static 영역에서는 static 영역에 대한 접근이 가능
    ```java
    public class Main {
        static String str = "문장";

        public void print() {
            System.out.println(str); // 문장
        }
    }
    ```

  - 예시
    ```java
    package practice;

    public class Student {
        private String name;
        private int age;
        private String major;

        public String getName() {
            return this.name;
        }
    }
    ```
    - **static을 지정한 경우**
      ```java
      package practice;

      public class StudentTest {
        
          // static을 이용해 인스턴스 생성 없이도 접근 가능하도록 설정
          static Student[] students = new Student[10];
          static int size = 0;
        
          Student getStudent(String name) {
              for (int i=0; i<size; i++) {
                  if (name.equals(students[i].getName())) {
                      return students[i]
                  }
              }
          }
          return null;
      }
      ```
    - **static을 지정하지 않은 경우 (1): 파라미터의 이용**
      ```java
      package practice;

      public class StudentTest {
        
          Student[] students = new Student[10];
          int size = 0;

          // 파라미터를 이용해 인스턴스와 변수 이용
          Student getStudent(String name, Student[] students, int size) {
              for (int i=0; i<size; i++) {
                  if (name.equals(students[i].getName())) {
                      return students[i];
                  }
              }
              return null;
          }
      }
      ```
    - **static을 지정하지 않은 경우 (2): 인스턴스의 생성**
      ```java
	  Student[] students = new Student[10];
	  int size = 0;

      Student getStudent(String name) {
          // StudentTest 인스턴스 생성 후 속성 지정
          StudentTest stt = new StudentTest();
          Student[] students = stt.students;
          int size = stt.size;
        
          for (int i=0; i<size; i++) {
              if (name.equals(students[i].getName())) {
                  return students[i];
              }
          }
          return null;
      }
      ```

## 5. 생성자

### 생성자

**인스턴스가 생성될 때 최초 한 번 수행되는 함수**이다.

`new` 키워드와 함께 호출

힙 영역에 객체 생성 후 객체의 번지가 return

생성자명은 클래스명과 동일하게 작성 (PascalCase로 작성)

반환타입이 없음

```java
public class Dog {
    public Dog() {
        ...
    }
}
```

### 기본 생성자와 파라미터가 있는 생성자

- 기본(디폴트) 생성자
  - 클래스 내에 생성자가 하나도 정의되어 있지 않을 경우 JVM이 자동으로 제공하는 생성자
  - `클래스명() {}` 형식으로 자동 생성
  - 예시
    ```java
    class Dog {
        String name;
        int age;
        // Dog() {} 있는 것과 같음
    }

    public class Main {
        public static void main(String[] args) {
            Dog dog1 = new Dog();
            dog1.name = "ysu";
            dog1.age = 3;
            System.out.println(dog1.name); // ysu
        }
    }
    ```

- 파라미터가 있는 생성자
  - 생성자 호출 시 값을 넘겨주어야 함
  - JVM에서 기본 생성자를 추가하지 않음
    ```java
    class Dog {
        String name;
        int age;

        Dog(String n, int a) {
            name = n;
            age = a;
        }
    }

    public class Main {
        public static void main(String[] args) {
            Dog dog1 = new Dog("ysu", 3);
            System.out.println(dog1.name); // ysu
        }
    }
    ```

- 생성자 오버로딩
  - 클래스 내에 메소드 이름이 같고 매개변수의 타입 또는 개수가 다른 것
  - 예시
    ```java
    class Dog {
        String name;
        int age;

        Dog(String n, int a) {
            name = n;
            age = a;
        }

        Dog(String n) {
            name = n;
        }

        Dog() {
        }
    }

    public class Main {
        public static void main(String[] args) {
            Dog dog1 = new Dog("ysu", 3);
            System.out.println(dog1.name); // ysu
            
            Dog dog2 = new Dog("ysu");
            System.out.println(dog2.name); // ysu
            
            Dog dog3 = new Dog();
            System.out.println(dog3.name); // null
        }
    }
    ```

### this

객체 자신을 가리킴

this를 이용하여 자신의 멤버 접근 가능: `this.멤버변수`

객체에 대한 참조이므로 static 영역에서 this 사용 불가

- `this([인자들])`: 생성자 호출
  - this 생성자 호출 시 제한사항
    - 생성자 내에서만 호출이 가능
    - 생성자 내에서 첫번째 구문에 위치해야 함
  - 예시
    ```java
    class Dog {
        String name;
        int age;

        // new Dog()로 호출하면 기본 필드 적용
        Dog() {
            this("ysu", 3);
        }

        Dog(String name, int age) {
            this.name = name;
            this.age = age;
        }

    }

    public class Main {
        public static void main(String[] args) {
            Dog dog1 = new Dog();
            System.out.println(dog1.name); // ysu
            
            Dog dog2 = new Dog("ysu", 3);
            System.out.println(dog2.name); // ysu
        }
    }
    ```

> **this는 객체의 필드를 식별할 수 있게 도와준다.**
> 
> ```java
> class Dog {
>     String name;
>     int age;
>   
>     // name과 age를 구별하지 못해 에러 발생
>     Dog(String name, int age) {
>         name = name;
>         age = age;
>     }
> }
> ```
> 
> ```java
> class Dog {
>     String name;
>     int age;
> 
>     // name과 age를 구별 가능
>     Dog(String name, int age) {
>         this.name = name;
>         this.age = age;
>     }
> }
> ```

## 6. 접근 제한자

### 패키지

프로그램의 많은 클래스를 관리하기 위해서 패키지를 이용한다.

클래스와 관련 있는 인터페이스들을 모아두기 위한 이름 공간이다.

패키지의 구분은 `.`을 이용한다.

ex) `com.project_이름.module_이름`

### import

다른 패키지에 있는 클래스를 사용하기 위해서는 `import` 과정이 필요하다.

`import` 뒤에 패키지 이름과 클래스 이름을 모두 입력해 특정 클래스를 가져오거나, `*`을 이용해 패키지 내 모든 클래스를 가져올 수 있다.

(`*`은 하위 클래스만 가져오고 하위 패키지는 가져오지 않음)

- `import package_name.class_name;`

- `import package_name.*;`

- 예시
  ```java
  // service 패키지에서 dto 패키지의 Person 클래스를 import 하기

  package com.project.service;

  import com.project.dto.Person;

  public class PersonService {
      Person p;
  }
  ```

### 캡슐화

객체의 속성(필드)와 행위(메소드)를 하나로 묶은 뒤, 실제 구현 내용 일부를 외부에 감추어 은닉한다.

**외부에서 호출 및 읽기/쓰기가 가능한 데이터/메소드**와 **외부에서 호출 및 읽기/쓰기가 불가능한 데이터/메소드**를 지정할 수 있다.

### 접근 제한자(access modifier)

클래스, 멤버 변수(필드), 멤버 메소드 등의 선언부에서 **접근 허용 범위를 지정**하는 역할의 키워드이다.

- 접근 제한자의 종류
  - `public`
    - 모든 위치에서 접근 가능
    - 외부 클래스, 내부 클래스, 멤버변수, 메소드 사용 가능
  - `protected`
    - 같은 패키지에서 접근 가능
    - 다른 패키지에서 접근이 불가능하지만, 다른 패키지의 클래스와 상속관계가 있을 경우 접근 가능
    - 내부 클래스, 멤버변수, 메소드 사용 가능
  - `(default)`
    - 같은 패키지에서만 접근 가능
    - 접근제한자가 선언이 안 되었을 경우 기본 적용
    - 외부 클래스, 내부 클래스, 멤버변수, 메소드 사용 가능
  - `private`
    - 자신 클래스에서만 접근이 허용
    - 내부 클래스, 멤버변수, 메소드 사용 가능

|수식어|클래스 내부|동일 패키지|(다른 패키지의) 하위 클래스|다른 패키지|
|--|--|--|--|--|
|private|○||||
|(default)|○|○|||
|protected|○|○|○||
|public|○|○|○|○|

- 접근자(getter)와 설정자(setter)
  - 접근제한에 의해 접근할 수 없는 변수의 경우, 접근하기 위한 메소드(접근자와 설정자)를 public으로 선언하여 사용
  - 예시
    ```java
    package access;

    public class Person {
        // private으로 접근 제한
        private String name;

        // getter: 접근 가능한 메소드
        public String getName() {
            return this.name;
        }

        // setter: 수정 가능한 메소드
        public void setName(String name) {
            this.name = name;
        }

    }
    ```
    ```java
    package access;

    public class Main {
        public static void main(String[] args) {
            Person p1 = new Person();
            p1.name = "ysu"; // 에러 발생 (바로 접근 불가)
            p1.setName("ysu");
            System.out.println(p1.getName());
        }
    }
    ```

> **getter와 setter를 사용하는 이유**
>
> `private`을 이용해 선언한 속성은 getter & setter를 이용해 접근해야 한다.
>
> 이는 `public`으로 선언한 속성을 `객체.속성`으로 접근하는 것과 동일하게 동작한다.
>
> 하지만 getter & setter를 이용하면 다음과 같이 **속성에 제한을 두거나 원하는 방식으로 접근하게 해 속성의 신뢰성을 높일 수 있다.**
>
> 또한 getter or setter 중 하나만 가능한 속성을 두어 읽기 전용/쓰기 전용으로 설정할 수도 있다.
> 
> ```java
> package modifier;
> 
> public class Car {
>     String color;
>     private int speed;
> 	
>     // getter에도 원하는 결과를 return하도록 로직을 짤 수 있다.
>     public int getSpeed() {
>         return this.speed;
>     }
> 	
>     // 속성에 제한 두기
>     public void setSpeed(int speed) {
>         if (speed <= 250 && speed >= 0) {
>             this.speed = speed;
>         } else {
>             System.out.println("속도 제한");
>         }
>     }
> }
> ```
> ```java
> package modifier;
> 
> public class CarTest {
>     public static void main(String[] args) {
>         Car c = new Car();
>         c.color = "Red";
>         c.setSpeed(100);
>         System.out.println(c.getSpeed()); // 100
>         c.setSpeed(300); // 속도 제한
>     }
> }
> ```

> **참고: boolean 속성의 getter**
>
> boolean 타입은 getter에 `get` 대신 `is`가 붙는다.
>
> ```java
> package modifier;
> 
> public class Person {
>     private boolean isMale;
> 
>     public boolean isMale() {
>         return isMale;
>     }
> 
>     public void setMale(boolean isMale) {
>         this.isMale = isMale;
>     }
> }
> ```
> ```java
> package modifier;
> 
> public class PersonTest {
>     public static void main(String[] args) {
>         Person p = new Person();
>         p.setMale(true);
>         System.out.println(p.isMale()); // true
>     }
> }
> ```

- 그 외 제한자 예시
  - `static`: 클래스 레벨의 요소 설정
  - `final`: 요소를 더이상 수정할 수 없게 함
  - `abstract`: 추상 메소드 및 추상 클래스 작성

### 싱글턴 패턴(Singleton Pattern)

싱글턴 패턴에서는 생성자가 여러 차례 호출되더라도 **실제로 생성되는 객체는 하나**이다.

최초 생성 이후에 호출된 생성자는 최초 생성자가 생성한 객체를 리턴한다.

```java
package practice;

public class Manager {
    private static Manager manager = new Manager();

    private Manager() {}

    public static Manager getManager() {
        return manager;
    }
}
```

### 연습해보기

- 이름, 나이, 전공 속성을 갖는 `Student` 클래스를 생성한다.

- 새로운 학생을 추가하고, 정보를 조회하고, 전공을 변경하는 메소드를 제공하는 `Manager` 클래스를 생성한다.

- 사용자에게 입력받는 `StudentTest` 클래스를 생성한다.

```java
package practice;

public class Student {
    private String name;
    private int age;
    private String major;
	
    Student(){
		
    }
	
    Student(String name, int age, String major) {
        this.name = name;
        this.age = age;
        this.major = major;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getMajor() {
        return major;
    }

    public void setMajor(String major) {
        this.major = major;
    }
}
```

```java
package practice;

// 싱글턴 패턴 적용
public class StudentManager {
    private Student[] students = new Student[100];
    private int size = 0; // 현재 Student 배열의 크기

    private static StudentManager manager = new StudentManager();

    private StudentManager() {
    }

    static StudentManager getManager() {
		    return manager;
    }

    // 새로운 학생 추가
    void addStudent(Student s) {
        // students 배열에 새로운 인스턴스 추가하고 size 증가
		    students[size++] = s;
    }

    // 이름으로 학생을 찾아 전공을 변경
    void changeMajor(String name, String major) {
        Student s = getStudent(name);
        if (s != null) {
            s.setMajor(major);
        }
    }

    // 이름으로 학생 정보 조회
    Student getStudent(String name) {
        for (int i = 0; i < size; i++) {
            if (name.equals(students[i].getName())) {
                return students[i];
            }
        }
        // 조건에 맞는 학생이 없는 경우 null 리턴
        return null;
    }
}
```

```java
package practice;

import java.util.Scanner;

public class StudentTest {

    static Student[] students = new Student[10];
    static int size = 0;

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);
        int sel;

        do {
            System.out.println("번호를 입력해주세요.");
            System.out.println("1. 학생 추가");
            System.out.println("2. 학생 조회");
            System.out.println("3. 전공 변경");
            System.out.println("0. 종료");
            sel = sc.nextInt();
            if (sel == 1) {
                // 학생 추가
                System.out.println("학생을 추가합니다.");
                System.out.println("이름: ");
                String name = sc.next();
                System.out.println("나이: ");
                int age = sc.nextInt();
                System.out.println("전공: ");
                String major = sc.next();

                // 학생 인스턴스를 만드는 방법
                // 1. 인스턴스를 만들고 setter를 이용해 속성을 변경한다.
                Student st = new Student();
                st.setName(name);
                st.setAge(age);
                st.setMajor(major);

                // 2. 속성이 정해진 인스턴스를 생성한다.
                Student st = new Student(name, age, major);
                sm.addStudent(st);

            } else if (sel == 2) {
                // 학생 조회
                System.out.println("학생을 조회합니다.");
                System.out.println("이름: ");
                String name = sc.next();
                Student st = sm.getStudent(name);
                if (st == null) {
                    System.out.println("학생을 찾지 못했습니다.");
            } else {
                    System.out.println("조회한 학생의 정보는");
                    System.out.println(st.getName());
                    System.out.println(st.getAge());
                    System.out.println(st.getMajor());
            }

            } else if (sel == 3) {
                // 전공 변경
                System.out.println("전공을 변경합니다.");
                System.out.println("이름: ");
                String name = sc.next();
                System.out.println("전공: ");
                String major = sc.next();
                sm.changeMajor(name, major);
            }
        } while (sel != 0);
    }
}
```

## 7. 상속

**상속**이란 어떤 클래스의 특성을 그대로 갖는 새로운 클래스를 정의한 것을 의미한다.

상위 클래스(=부모 클래스, super class)는 여러 개의 하위 클래스(=자식 클래스, sub class)를 가질 수 있다.

`extend`를 이용해 상속을 사용할 수 있다.

부모의 생성자와 초기화 블록은 상속되지 않는다.

다중 상속을 허용하지 않는다. (최대 하나의 부모 클래스만 가질 수 있다.)

모든 클래스는 기본적으로 Object 클래스를 상속받는다. (`extends Object`가 생략되어 있음)

```java
package inheritance;

public class Person {
	String name;
	int age;
	
	public void eat() {
		System.out.println("음식을 먹는다.");
	}
}
```

```java
package inheritance;

// Person 클래스를 상속받는 Student 클래스 생성
public class Student extends Person {
	String major;
	
	public void study() {
		System.out.println("공부를 한다.");
	}
}

```

```java
package inheritance;

public class StudentTest {
	public static void main(String[] args) {
		Student s = new Student();
        // Person의 속성에 접근 가능
		s.name = "ysu";
		System.out.println(s.name);
	}
}
```

### super

인스턴스가 생성될 때 해당 클래스는 부모 클래스의 생성자를 호출한다.

이는 `super()`가 숨겨져 있기 때문인데, `super()`는 **자식 클래스의 생성자에서 부모 클래스의 생성자를 호출**하는 메소드이다.

이때 그 부모 클래스의 생성자도 부모 클래스의 생성자를 호출하기 때문에, 결국 조상 클래스의 모든 생성자를 호출하게 된다.

```java
package inheritance;

public class Person {
	String name;
	int age;
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
	public void eat() {
		System.out.println("음식을 먹는다.");
	}
}
```

```java
package inheritance;

public class Student extends Person {
	String major;
	
	public Student(String name, int age, String major) {
		super(name, age);
		this.major = major;
	}
	
	public void study() {
		System.out.println("공부를 한다.");
	}
}
```

```java
package inheritance;

public class StudentTest {
	public static void main(String[] args) {
		Student s = new Student("홍길동", 20, "컴퓨터공학과");
		System.out.println(s.name);
	}
}
```

여기서 `super`는 조상 클래스를 가리키기 때문에, `super` 키워드를 이용해 조상의 속성, 메소드에 접근할 수 있다.

```java
package inheritance;

public class Student extends Person {
	String major;
	
	public Student(String name, int age, String major) {
		super(name, age);
		this.major = major;
	}
	
	public void study() {
        super.eat(); // 음식을 먹는다.
		System.out.println("공부를 한다.");
	}
}
```

### 오버라이딩(재정의, overriding)

상위 클래스에 선언된 메소드를 자식 클래스에서 재정의하는 것을 뜻한다.

메소드의 이름, 반환형, 매개변수가 동일해야 한다.

하위 클래스의 접근제어자 범위가 상위 클래스보다 크거나 같아야 한다.

(부모에서 `public`을 사용했다면 자식에서 `private`을 사용할 수 없다.)

조상보다 더 큰 예외를 던질 수 없다.

```java
package inheritance;

public class Person {
	String name;
	int age;
	
	public void eat() {
		System.out.println("음식을 먹는다.");
	}
}
```

```java
package inheritance;

public class Student extends Person {
	String major;
	
    // 메소드 오버라이딩
    // @Override => annotation를 이용해 오버라이딩을 명시할 수도 있다.
	public void eat() {
		System.out.println("학생이 음식을 먹는다.");
	}
}
```

```java
package inheritance;

public class StudentTest {
	public static void main(String[] args) {
		Student s = new Student();
		s.eat(); // 학생이 음식을 먹는다.
	}
}
```

### final

해당 선언이 최종 상태로, 결코 수정될 수 없다.

- final 클래스: 상속 금지
- final 메소드: overriding 금지
- final 변수: 더 이상 값을 바꿀 수 없음 (상수화, 관례상 대문자 snake_case를 사용)

## 8. 다형성

상속 관계에 있을 때 조상 클래스의 타입으로 자식 클래스 객체를 참조할 수 있다.

```java
package polymorphism;

public class Person {
	String name;
	int age;

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
}
```

```java
package polymorphism;

// Person을 상속받음
public class Student extends Person {
	String major;
	
	public Student(String name, int age, String major) {
		super(name, age);
		this.major = major;
	}
}
```

```java
package polymorphism;

public class MainTest {
	public static void main(String[] args) {
		// 조상 클래스의 타입으로 자식 클래스를 참조할 수 있다.
		Person p = new Student("홍길동", 20, "컴퓨터공학과");
		Object obj = new Person("김민수", 23);
		
		// 자식 클래스는 조상 클래스를 참조할 수 없다. 
		// Student s = new Person("ysu", 27);

        // 하위 클래스의 속성에 접근할 수 없다.
        // p.major -> 에러 발생 
	}
}
```

- 다형성의 활용

  - 다른 타입의 객체를 다루는 배열

    배열은 정해진 타입을 담기 때문에 객체 타입마다 각각 배열을 생성해주어야 한다.

    하지만 조상을 배열의 타입으로 지정하면, 하위 모든 클래스는 해당 배열에 들어갈 수 있다.

    ```java
    // Person 클래스를 담는 배열에는 자식 클래스인 Student 클래스도 들어갈 수 있다.
    Person[] persons = new Person[3];
    persons[0] = new Person();
    persons[1] = new Student();
    // persons[2] = new Object(); => 불가능

    // Object[] objs = new Object[3];
    // 모든 클래스를 담을 수 있다.
    ```

  - 매개변수의 다형성
    
    메소드가 호출되기 위해서는 인자와 파라미터의 타입이 같아야 한다.

    따라서 객체의 타입마다 각각 다른 메소드를 생성해야 하는 불편함이 있다.

    이때 조상을 파라미터로 처리하면 하위 모든 클래스는 해당 메소드를 사용 가능하다.

    ```java
    package polymorphism;

    public class MainTest {
        // 파라미터: Person 타입
        public static void getName(Person p) {
            System.out.println(p.name);
        }

        public static void main(String[] args) {
            Person p = new Person("홍길동", 20);
            Student s = new Student("김민수", 23, "컴퓨터공학과");

            getName(p); // 홍길동
            getName(s); // 김민수
        }
    }

    // println 메소드는 인자로 Object를 받기 때문에, 모든 클래스를 넣을 수 있다.
    public void println(Object x) {
        String s = String.valueOf(x);
        ...
    }
    ```

- 다형성과 참조형 객체의 형변환

  - 묵시적 캐스팅 (child -> super)
    ```java
    Person p = new Person();
    Object obj = p;
    // Object obj = (Object)p 에서 괄호가 생략됨
    ```

  - 명시적 캐스팅 (super -> child)
    ```java
    Person p = new Student("ysu", 27, "컴퓨터공학과");
    Student s = (Student)p; // 괄호 안에 클래스 명시
    // Student s = p; -> 괄호 없이 형변환 불가

    System.out.println(p.major); // 에러
	System.out.println(s.major); // 컴퓨터공학과
    // p와 s는 같은 주소값을 갖지만 접근할 수 있는 내용이 다르다.
    ```

- `instanceof`

  실제 메모리에 있는 객체가 특정 클래스의 타입인지 확인해 boolean으로 return 한다.

  이를 이용하면 조상 클래스를 자손 클래스로 바꿀 수 있는지 확인할 수 있다.

  `인스턴스 instanceof 클래스명`: 인스턴스가 해당 클래스로 형변환 가능하면 true, 아니면 false 반환

  ```java
  package polymorphism;

  public class MainTest {
  	  public static void main(String[] args) {
  		  Person p1 = new Person();
  		  Person p2 = new Student();

		  // 실행 x
		  if (p1 instanceof Student) {
			  Student s = (Student)p1;
		  }
		
		  // 실행 o
		  if (p2 instanceof Student) {
			  Student s = (Student)p2;
		  }
	  }
  }
  ```

- 참조 변수의 레벨에 따른 객체의 멤버 연결

  - 부모, 자식 클래스의 멤버 변수가 중복되는 경우

    => 변수가 참조하는 타입에 따라 결정

  - 부모, 자식 클래스의 메소드가 중복되는 경우(override 되었을 때)

    => 무조건 자식 클래스의 메소드가 호출 (virtual method invocation)

  ```java
  package polymorphism;

  class SuperClass {
    String x = "super";

    public void method() {
      System.out.println("super class method");
    }
  }

  class SubClass extends SuperClass {
    String x = "sub";

    @Override
    public void method() {
      System.out.println("sub class method");
    }
  }

  public class MainTest {

    public static void main(String[] args) {
      SubClass subClass = new SubClass();
      System.out.println(superClass.x); // sub
      superClass.method(); // sub class method

      SuperClass superClass = subClass;
      System.out.println(superClass.x); // super
      superClass.method(); // sub class method
    }
  }
  ```

## 9. 추상 클래스

아래와 같은 상황에서 `Chef` 클래스의 `cook` 메소드는 하위 클래스에서만 override 되어 사용된다.

```java
package abstractclass;

class Chef {
	String name;
	
  // 하위 클래스에서만 사용되는 메소드
	public void cook() {
		System.out.println("요리하기");
	}
}

class KFoodChef extends Chef{
  // override
	public void cook() {
		System.out.println("한식 요리");
	}
}

class JFoodChef extends Chef{
  // override
	public void cook() {
		System.out.println("일식 요리");
	}
}

public class ChefTest {
	
	public static void main(String[] args) {
    // chef로 구성된 배열 생성
		Chef[] chefs = new Chef[2];
		chefs[0] = new KFoodChef();
		chefs[1] = new JFoodChef();
		
		for (Chef chef : chefs) {
			chef.cook();
			// 한식 요리
			// 일식 요리
		}
	}
}
```

`Chef` 클래스의 `cook` 메소드는 하위 클래스에서만 사용되기 때문에, `Chef` 클래스에서의 구현이 무의미하다.

하지만 `Chef` 클래스에서 `cook` 메소드를 지운다면 `instanceof` 키워드를 사용해야 하는 번거로움이 발생한다.

```java
package abstractclass;

class Chef {
	String name;
}

class KFoodChef extends Chef{
  // override되지 않은 일반 메소드
	public void cook() {
		System.out.println("한식 요리");
	}
}

class JFoodChef extends Chef{
  // override되지 않은 일반 메소드
	public void cook() {
		System.out.println("일식 요리");
	}
}

public class ChefTest {
	
	public static void main(String[] args) {
    // chef로 구성된 배열 생성
		Chef[] chefs = new Chef[2];
		chefs[0] = new KFoodChef();
		chefs[1] = new JFoodChef();
		
		for (Chef chef : chefs) {
      // Chef 클래스는 cook 메소드가 없기 때문에 cook() 사용 불가
			if (chef instanceof KFoodChef) {
        KFoodChef k = (KFoodChef)chef;
        k.cook();
        // ((KFoodChef)chef).cook(); 으로 한 번에도 가능
      } else if (chef instanceof JFoodChef) {
        JFoodChef j = (JFoodChef)chef;
        j.cook();
      }
		}
	}
}
```

따라서 이러한 경우 `abstract` 키워드를 이용해 **추상 클래스**를 정의할 수 있다.

클래스에 `abstract`가 붙으면 해당 클래스의 객체를 생성할 수 없다.

(익명클래스 문법을 사용하면 객체 생성 가능)

메소드에 `abstract`가 붙으면 해당 메소드를 직접 실행할 수 없다.

`abstract` 메소드를 포함하는 클래스는 반드시 추상 클래스가 되어야 한다.

또한 `abstract` 메소드는 하위 클래스에서 반드시 override 되어야 한다.

(`abstract` 메소드를 override 하지 않으면 해당 클래스도 추상 클래스가 된다.)

즉, 추상 클래스를 이용하면 **구현의 강제를 통해 프로그램의 안정성이 향상**된다.

```java
package abstractclass;

// 추상 클래스
abstract class Chef {
	String name;

  // 추상 메소드
  // 메소드의 선언부만 남기고 구현부는 세미콜론으로 대체한다.
	public abstract void cook();
}

class KFoodChef extends Chef {
	// override
	public void cook() {
		System.out.println("한식 요리");
	}
}

class JFoodChef extends Chef {
	// override
	public void cook() {
		System.out.println("일식 요리");
	}
}

public class ChefTest {

	public static void main(String[] args) {
		// chef로 구성된 배열 생성
		Chef[] chefs = new Chef[2];
		chefs[0] = new KFoodChef();
		chefs[1] = new JFoodChef();
		
		// 추상 클래스는 인스턴스를 생성할 수 없다.
		// Chef chef = new Chef(); -> 에러 발생

    // 상위 클래스 타입으로 자식을 참조할 수는 있다.
    // Chef chef = new KFoodChef();

    // 익명클래스 문법을 사용하면 Chef 클래스의 인스턴스를 생성할 수 있다.
		// Chef chef = new Chef() {
		// 	@Override
		// 	public void cook() {
		// 		System.out.println("요리하기");
		// 	}
		// };

		for (Chef chef : chefs) {
			chef.cook();
		}
	}
}
```

## 10. interface

완벽히 추상화된 객체로, `interface` 키워드를 이용하여 선언한다.

**선언되는 변수는 모두 상수로 적용**되며, **선언되는 메소드는 모두 추상 메소드로 적용**된다.

`extends`를 이용하여 상속이 가능하며, 다중 상속 또한 가능하다.

(구현부가 없기 때문에 부모 인터페이스끼리 충돌나지 않음)

추상클래스와 동일하게 **객체 생성이 불가능**하다.

클래스가 인터페이스를 상속할 경우에는 `implements` 키워드를 이용한다.

인터페이스를 상속받는 하위 클래스는 추상 메소드를 반드시 오버라이딩(재정의) 해야 한다.

(구현하지 않을 경우 추상 클래스로 표시)

클래스와 동일하게 **다형성**이 적용된다.

```java
package interface1;

interface MyInterface1 {
	public static final int MEMBER1 = 10;
	int MEMBER2 = 10;
	// 그냥 선언해도 public static final이 생략된 형태로 적용
	
	public abstract void method1(int param);
	void method2(int param);
	// 그냥 선언해도 public abstract가 생략된 형태로 적용
}

interface MyInterface2 {
	
}

// extends를 이용해 상속 가능
// 다중 상속도 가능
interface MyInterface3 extends MyInterface1, MyInterface2 {
	
}

// 클래스는 implements를 이용해 인터페이스를 상속받음
class MyClass1 implements MyInterface1 {
	
	@Override
	public void method1(int params) {
		
	}
	
	@Override
	public void method2(int params) {
		
	}
	
}

//만약 내부에서 메소드를 override하지 않는다면 abstract 사용 필수
abstract class MyClass2 implements MyInterface2 {
	
}

public class MainTest {
	public static void main(String[] args) {
		// 인터페이스는 인스턴스 생성이 불가
		// MyInterface1 m = new MyInterface1();
		
		// 다형성 적용
		MyInterface1 myInterface1 = new MyClass1();
	}
}
```

### 인터페이스의 사용 이유

구현을 강제해 표준화 처리를 할 수 있으며, 인터페이스를 통한 간접적인 클래스 사용으로 손쉬운 모듈 교체가 가능하다.

서로 상속 관계가 없는 클래스들에게 인터페이스를 통한 관계 부여로 다형성을 확장할 수 있다.

모듈 간 독립적인 프로그래밍이 가능해 개발 기간을 단축시킬 수 있다.

```java
package interface2;

public class Student {
	String name;
	int age;
	String major;
}
```

```java
package interface2;

public interface IStudentManager {
	void add(Student s);
	Student[] getList();
	Student searchByName(String name);
	boolean remove(String name);
}
```

```java
package interface2;

// 인터페이스를 상속받아 강제성 부여
public class StudentManager implements IStudentManager {

	@Override
	public void add(Student s) {
		// TODO Auto-generated method stub
		
	}

	@Override
	public Student[] getList() {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	public Student searchByName(String name) {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	public boolean remove(String name) {
		// TODO Auto-generated method stub
		return false;
	}
	
}
```

```java
package interface2;

public class MainTest {
	public static void main(String[] args) {
    // 인터페이스를 타입으로 지정해 해당 인터페이스를 상속받는 다양한 클래스를 지정 가능 
		IStudentManager sm = new StudentManager();
	}
}
```