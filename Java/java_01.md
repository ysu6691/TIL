# Java 기초

## 1. Java 기본

### Java 동작 원리
<img src="https://user-images.githubusercontent.com/109272360/189694947-41c4bb1a-a02d-4652-89e6-ef7d7f70aba9.png" width="450px" style="margin-top:10px;">

- JRE(Java Runtime Environment)
  - 컴파일된 java 프로그램을 실행하는데 필요한 패키지이다.
  - JVM과 자바 클래스 라이브러리 등으로 구성되어 있다.

- JDK(Java Development Kit)
  - java를 사용하기 위해 필요한 모든 기능을 갖춘 java용 SDK(Software Development Kit)이다.
  - JRE 뿐 아니라 컴파일러(javac)와 같은 여러 도구로 구성되어 있어, 프로그램 실행, 생성 및 컴파일이 가능하다.

- 자바 가상 머신(JVM, Java Virtual Machine)
  - OS에 종속받지 않고 CPU가 Java bytecode를 인식, 실행할 수 있게 하는 주체

- Bytecode
  - JVM이 이해할 수 있는 언어로 변환된 자바 소스코드
  - 자바 컴파일러에 의해 변환된 코드 명령어 크기가 1바이트라서, 자바 바이트 코드로 불린다.
  - 바이트코드는 다시 실시간 번역기 또는 JVM 내 JIT 컴파일러에 의해 바이너리 코드(CPU가 이해할 수 있는 형태)로 변환된다.

- JIT(Just-In-Time compliation)
  - 기본적으로는 프로그램 실행 중에 컴파일을 하는 동적 컴파일러(특징: 느림)이다.
  - 하지만 기계어로 변환된 코드 중 자주 쓰이는 코드를 캐시에 저장해, 런타임에 다시 기계어로 변환하지 않고 사용한다.
  - 따라서 미리 컴파일된 코드를 사용하는 정적 컴파일과 같은 효과를 낼 수 있어, 다른 동적 컴파일러보다 좋은 성능을 갖는다.

- Main method
  - `public static void main(String [] args) {}`
  - java application 실행 시 가장 먼저 호출되는 부분

- 주석
  - `//` : 줄 주석
  - `/* */` : 범위 내용 주석
  - `/** */` : Documentation API를 위한 주석

- 출력문
  - print: 엔터 입력시 개행문자 포함 x (한 줄 출력)
  - println: 엔터 입력시 개행문자도 함께 입력
  - printf: 형식을 고려할 때 사용
    - %d: 정수
    - %f: 실수
    - %c: 문자
    - %s: 문자열
    - %b: bool 형식
  ```java
  public class intro_PrintTest {
      public static void main(String[] args) {
        
          // print
          System.out.print("Hello World");
          
          // println
          System.out.println("Hello world");
          System.out.println("Hello world");
          
          // printf
          System.out.printf("%d \n", 10); // 10진수
          System.out.printf("%o \n", 10); // 8진수
          System.out.printf("%X \n", 10); // 16진수 (10 이상은 알파벳 대문자로 표현)
          System.out.printf("%x \n", 10); // 16진수 (10 이상은 알파벳 소문자로 표현)
          
          System.out.printf("%4d \n", 10); // 4칸을 확보한 뒤 오른쪽 정렬
          System.out.printf("%-4d \n", 10); // 4칸을 확보한 뒤 왼쪽 정렬
          System.out.printf("%04d \n", 10); // 4칸을 확보한 뒤 오른쪽 정렬 (빈칸은 0으로 채우기)
          
          System.out.printf("%f \n", 10.1); // 실수
          System.out.printf("%.2f \n", 10.05); // 실수(소수점 둘째자리까지, 셋째자리에서 반올림)
          
          System.out.printf("%s \n", "안녕하세요"); // 문자열은 큰 따옴표 사용
          System.out.printf("%c \n", 'ㅇ'); // 문자는 작은 따옴표 사용

          System.out.printf("%b \n", 10); // bool 형식
          
          /*
          Hello WorldHello world
          Hello world
          10 
          12 
          A 
          a 
            10 
          10   
          0010 
          10.100000 
          10.05 
          안녕하세요 
          ㅇ
          true 
          */
      }
  }

  ```

### 변수와 자료형
- 선언
  - `자료형 변수명`
  - ex) int age;  String name;

- 저장(할당)
  - `변수명 = 저장할 값`
  - ex) age = 30;  name = "java";

- 초기화: 선언과 저장을 동시에
  - `자료형 변수명 = 저장할 값`
  - ex) int age = 30;

```java
public static void main(String[] args) {
    int a; // 선언
    a = 10; // 저장
    System.out.println(a); // 10
    
    int b = 20; // 초기화
    System.out.println(b); // 20
    
    int c = a;
    System.out.println(c); // 10
    c = b;
    System.out.println(c); // 20
    // d = b; -> 오류 발생 (d 변수를 선언하지 않음)

    System.out.printf("변수 c의 값은 %d", c);
    // 변수 c의 값은 20
}
```

