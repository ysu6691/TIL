# IoC 컨테이너와 DI

## 1. IoC 컨테이너: 빈 팩토리와 애플리케이션 컨텍스트

### IoC 컨테이너란?

스프링 애플리케이션은 객체의 생성과 관계설정 등의 작업을 애플리케이션 코드 대신 독립된 컨테이너가 담당한다.

이를 컨테이너가 코드 대신 오브젝트에 대한 제어권을 갖고 있다고 해서 **제어 역전(IoC, Inversion of Control)**이라고 한다.

스프링에서 IoC를 담당하는 컨테이너를 **빈 팩토리** 또는 **애플리케이션 컨텍스트**라고 부른다.

- **빈 팩토리**
  - **의존성 주입(DI, Dependency Injection)**을 위한 객체의 생성과 객체 사이의 런타임 관계를 설정
  - `BeanFactory` 인터페이스로 정의된다.

- **애플리케이션 컨텍스트**
  - 빈 팩토리에 엔터프라이즈 애플리케이션을 개발하는 데 필요한 컨테이너 기능을 추가한 것
  - `ApplicationContext` 인터페이스로 정의된다. (`BeanFactory` 인터페이스를 상속하고 있음)


**실제로 스프링 컨테이너 또는 IoC 컨테이너는 ApplicationContext 인터페이스를 구현한 클래스의 객체를 뜻한다.**

스프링 애플리케이션은 최소한 하나 이상의 IoC 컨테이너, 즉 애플리케이션 컨텍스트 객체를 갖고 있다.

### IoC 컨테이너를 이용해 애플리케이션 만들기

IoC 컨테이너를 사용하는 애플리케이션을 만드는 방법과 그 동작원리에 대해 살펴보면 다음과 같다.

```java
// IoC 컨테이너 만들기
// StaticApplicationContext는 ApplicationContext 인터페이스를 구현한 클래스이다.
StaticApplicationContext ac = new StaticApplicationContext();
```

이렇게 만들어진 컨테이너가 본격적인 IoC 컨테이너로 동작하기 위해서는 두 가지가 필요하다.

- **POJO 클래스**

  먼저, 애플리케이션의 핵심 코드를 담고 있는 POJO(Plain Old Java Object) 클래스를 준비해야 한다.

  POJO는 유연한 확장성을 고려하고 자신의 기능에 충실하게 작성되어야 한다.

  서로의 인터페이스에만 의존하며, 어떤 구체적인 객체를 사용하게 될지는 관심을 갖지 않는다.

  <img src="https://user-images.githubusercontent.com/109272360/230944859-05bd1713-8139-44b9-b288-7906a0f556f8.png" width="400px">

  ```java
  public class Hello {
      private String name;
      private Printer printer;
	
      public Hello() {
      }

      public Hello(String name, Printer printer) {
          this.name = name;
          this.printer = printer;
      }

      public String sayHello() {
          return "Hello " + name;
      }
	
      // DI에 의해 Printer 타입의 오브젝트에게 출력 작업을 위임한다.
      public void print() {
          this.printer.print(this.sayHello());
      }
  }
  ```

  ```java
  public interface Printer {
      void print(String message);
  }
  ```

  ```java
  public class StringPrinter implements Printer {
      private StringBuffer buffer = new StringBuffer();

      // buffer를 변경한다.
      public void print(String message) {
          this.buffer.append(message);
      }

      public String toString() {
          return this.buffer.toString();
      }
  }
  ```



- **설정 메타정보**

  만든 POJO 클래스들 중에 애플리케이션에서 사용할 것들을 고르고, 이를 IoC 컨테이너가 제어할 수 있도록 적절한 메타정보를 만들어 제공해야 한다.

  스프링 컨테이너가 관리하는 객체를 **빈(Bean)**이라고 하며, IoC 컨테이너가 필요로 하는 설정 메타정보는 이 빈을 어떻게 만들고 어떻게 동작하게 할 것인가에 관한 정보이다.

  애플리케이션 컨텍스트는 `BeanDefinition` 인터페이스로 구현된 객체를 사용해 IoC와 DI 작업을 수행한다.

  설정 메타정보를 작성한 원본 자료를 읽어와 `BeanDefinition` 객체로 변환하는 `BeanDefinitionReader`만 있으면, 설정 메타정보는 어떤 형식으로든 작성될 수 있다.

  따라서 스프링의 메타정보는 특정한 파일 포맷이나 형식에 제한되지 않으며, xml, 애노테이션, 프로퍼티 파일 등 여러 방식으로 표현될 수 있다.

  그 뒤 스프링 IoC 컨테이너는 이를 참고해서 빈 객체를 생성하고 프로퍼티나 생성자를 통해 DI 작업을 수행한다.

  이렇게 DI로 연결되는 객체들이 모여서 하나의 애플리케이션을 구성하고 동작하게 된다.

  <img src="https://user-images.githubusercontent.com/109272360/230946785-d524ae2c-da74-4855-926c-81197f89c43f.png" width="600px">

이제 IoC 컨테이너에 빈을 등록하는 테스트 코드를 작성해볼 수 있다.