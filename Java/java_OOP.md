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

        public static moid main(String[] args) {
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
>   ```java
>   class Dog {
>   	String name;
>   	int age;
>   
>      // name과 age를 구별하지 못해 에러 발생
>   	Dog(String name, int age) {
>   		name = name;
>   		age = age;
>   	}
>   }
>   ```
> 
>   ```java
>   class Dog {
>   	String name;
>   	int age;
>   
>      // name과 age를 구별 가능
>   	Dog(String name, int age) {
>   		this.name = name;
>   		this.age = age;
>   	}
>   }
>   ```

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
> `객체.속성`으로 접근하는 것과 getter를 이용해 접근하는 것은 똑같이 동작한다.
> 
> 
> 
> 
> 
> 

- 그 외 제한자 예시
  - `static`: 클래스 레벨의 요소 설정
  - `final`: 요소를 더이상 수정할 수 없게 함
  - `abstract`: 추상 메소드 및 추상 클래스 작성

