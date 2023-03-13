# Spring 기본

## 1. 스프링이란?

### 제어 역전(IoC)

자바에서 객체를 사용하려면 사용하려는 객체를 선언한 뒤 사용해야 한다.

```java
@RestController
public class NoDIController {

    // 구현체 직접 생성
    private MyService service = new MyServiceImpl();

    @GetMapping("/hello")
    public String getHello() {
        return service.getHello();
    }
}
```

위 코드는 객체를 생성하고 사용하는 일련의 작업을 개발자가 직접 제어하는 구조이다.

하지만 **제어 역전(IoC: Inversion of Control)**을 특징으로 하는 스프링은 사용할 객체를 직접 생성하지 않고 **객체의 생명주기 관리를 "스프링 컨테이너" 또는 "IoC 컨테이너"에 위임**한다.

이러한 제어 역전을 통해 의존성 주입(DI), 관점 지향 프로그래밍(AOP) 등이 가능해진다.

즉, 스프링을 사용하면 객체의 제어권을 컨테이너로 넘기기 때문에 개발자는 비즈니스 로직을 작성하는 데 더 집중할 수 있다.

### 의존성 주입(DI)

**의존성 주입(DI: Dependency Injection)**이란 제어 역전의 방법 중 하나로, **사용할 객체를 외부 컨테이너로부터 주입받아 사용**하는 방식을 의미한다.

스프링에서 의존성을 주입받는 방법은 세 가지가 있다.

- 생성자를 통한 의존성 주입

  ```java
  @RestController
  public class DIController {

      MyService myService;

      @Autowired
      public DIController(MyService myService) {
          this.myService = myService;
      }

  }
  ```

- 필드 객체 선언을 통한 의존성 주입

  ```java
  @RestController
  public class DIContainer {

      @Autowired
      private MyService myService;

  }
  ```

- setter 메서드를 통한 의존성 주입

  ```java
  @RestController
  public class SetterInjectionController {

      MyService myService;

      @Autowired
      public void setMyService(MysService myService) {
        this.myService = myService;
      }

  }
  ```

이 세 가지 방식 중 스프링 공식 문서에서는 **생성자를 통한 의존성 주입 방식**을 권장한다.

다른 방식과는 다르게 생성자를 통한 의존성 주입 방식은 레퍼런스 객체 없이는 객체를 초기화할 수 없게 설계할 수 있기 때문이다.