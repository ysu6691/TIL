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

### Java 출력
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

## 2. 자료형, 변수, 연산자

### 자료형
|타입|자료형|크기|범위|
|---|---|---|---|
|논리형|boolean||True / False|
|문자형|char|2byte|0 ~ 65,535|
|정수형|byte|1byte|-128 ~ 127|
|정수형|short|2byte|-32,768 ~ 32,767|
|정수형|int|4byte|-2,147,483,648 ~ -2,147,483,647|
|정수형|long|8byte|-9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807|
|실수형|float|4byte|-3.402932347e+38~+3.40292347e+38|
|실수형|double|8byte|	-179769313486231570e+308~1.79769313486231570e+308|

- 자료형 범위 비교
byte < char = short < int < long < float < double

- 데이터 형변환
  - 암묵적(Implicit Casting)
    - 범위가 넓은 데이터 형에 좁은 데이터 형을 대입하는 것
    - ex) byte b = 100;  int i = b;
  - 명시적(Explicit Casting)
    - 범위가 좁은 데이터 형에 넓은 데이터 형을 대입하는 것
    - ex1) int i = 100;  byte b = i; (x)
    - ex2) int i = 100;  byte b = (byte) i; (o)

  ```java
	public static void main(String[] args) {
      // 암묵적 형변환
      short sa = 32767;
      int c = sa;
      System.out.println(c); // 32767
      
      // 명시적 형변환
      int d = 100;
      short sb = (short) d;
      System.out.println(sb); // 100
      
      // 명시적 형변환 시 범위를 벗어날 경우, 값 손실 발생
      int e = 128;
      byte sc = (byte) e;
      System.out.println(sc); // -128
      
      double f = 1.6;
      int ia = (int) f;
      System.out.println(ia); // 1 (소수점 버림)
	}
  ```

### 변수
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

    int d = 'A';
    System.out.println(d); // 65 
    // int로 되어있는 변수에 문자열을 넣으면, 아스키 코드로 변환되어 저장

    int a = 10;
		int b = a;
		System.out.println(a); // 10
		System.out.println(b); // 10
		a = 12;
		System.out.println(a); // 12
		System.out.println(b); // 10 (b는 값 변화 x)

    System.out.printf("변수 c의 값은 %d", c);
    // 변수 c의 값은 20
    System.out.printf("a: %d, b: %d", a, b);
    // a: 0, b: 1
}
```

### 연산자
- 연산자 종류
  |종류|연산 기호|우선 순위|
  |--|--|--|
  |증감 연산자|++, --|1순위|
  |산술 연산자|+, -, *, /, %|2순위|
  |시프트 연산자|>>, <<, >>>|3순위|
  |비교 연산자|>, <, >=, <=, ==, !=|4순위|
  |비트 연산자|&, &#124;, ^, ~|~만 1순위, 나머지는 5순위|
  |논리 연산자|%%, &#124;&#124;, !|!만 1순위, 나머지는 6순위|
  |조건(삼항) 연산자|?, :|7순위|
  |대입 연산자|=, *=, /=, %=, +=, -=|8순위|

  - 증감 연산자
    - 피연산자에 저장된 값을 1 증가 또는 1 감소시킨다.
    - 피연산자의 왼쪽에 위치하면 전위형(prefix), 오른쪽에 위치하면 후위형(postfix)라고 한다.
    - 전위형은 연산을 수행한 후 피연산자의 값을 변화시키고, 후위형은 피연산자의 값을 변화시킨 뒤 연산을 수행한다.

  - 조건(삼항) 연산자
    - `조건식 ? 식1 : 식2`
		- 조건식이 참일 경우 식1 수행
		- 조건식이 거짓일 경우 식2 수행

  ```java
  public static void main(String[] args) {
      // 증감 연산자
      int a = 5;
      System.out.println(a++); // 5 (출력을 하고 a에 1을 더함)
      System.out.println(a); // 6
      System.out.println(++a); // 7 (a에 1을 더하고 출력)
      
      int b = a++; // b = a, a = a + 1
      int c = ++a; // a = a + 1, c = a
      System.out.println(b); // 7
      System.out.println(c); // 9


      // 산술 연산자
      int a = 10;
      int b = 6;
      
      // 정수와 정수의 연산 = 정수
      System.out.println(a+b); // 16
      System.out.println(a/b); // 1
      
      // 정수와 실수의 연산 = 실수
      System.out.println(a/(double)b); // 1.666...
      System.out.println((double)a/b); // 1.666...
      System.out.println((double)(a/b)); // 1.0


      // 논리 연산자
      int a = 10;
      int b = 20;
      
      System.out.println(a > 5 && b > 5); // true
      System.out.println(a < 5 && b > 5); // false
      System.out.println(a < 5 || b > 5); // true
      System.out.println(a < 5 || b < 5); // false
      System.out.println(!(a < 5 || b < 5)); // true


      // 삼항 연산자
      int a = 2;
      int b = 3;
      System.out.println(a % 2 == 0 ? "짝" : "홀"); // 짝
      System.out.println(b % 2 == 0 ? "짝" : "홀"); // 홀
  }
  ```

## 3. 제어문

### 조건문
- if 문
  - `if(조건식) { }`
  - `if(조건식) { } else {}`
  - `if(조건식1) { } elif(조건식2) { } else{ }`
  ```java
	public static void main(String[] args) {
      // if(조건문) { }
      int a = 5;
      if(a > 3) {
          System.out.println("a는 3보다 크다.");
      }
      if (a > 3)
          System.out.println("한 문장은 중괄호 생략 가능");
      
      // if(조건문) { } else { }
      int b = 2;
      if(b > 3) {
          System.out.println("b는 3보다 크다.");
      }
      else {
          System.out.println("b는 3보다 작거나 같다.");
      }
      
      // if(조건문) { } else if(조건문2) { } else { }
      int c = 5;
      if(c > 7) {
          System.out.println("c는 7보다 크다.");
      }
      else if(c > 3) {
          System.out.println("c는 3보다 크고 7보다 작거나 같다.");
      }
      else {
          System.out.println("c는 3보다 작다.");
      }
	}
  ```
- switch 문
  - 인자로 선택 변수를 받아 변수의 값에 따라서 실행문이 결정
  - 선택에 따라 break 사용 가능
  - if문의 else와 같이, default 사용 가능
  ```java
  public static void main(String[] args) {
  int month1 = 12;
  
  switch(month1) {
  case 1:
      System.out.println("31일");
      break;
  case 2:
      System.out.println("28일");
      break;
  // 생략
  case 12:
      System.out.println("31일");
      break;
  }
  // 31일

  int month2 = 13;
  switch(month2) {
  case 1:
  case 3:
  case 5:
  // 생략
  case 12:
      System.out.println("31일");
      break;
  case 4:
  case 6:
  case 9:
  case 11:
      System.out.println("30일");
      break;
  case 2:
      System.out.println("28일");
      break;
  default:
      System.out.println("없는 달 입니다.");
  }
  // 없는 달 입니다.
  ```

### 반복문
- for문
  - `for(초기화식; 조건식; 증감식) {반복 수행할 문장}`
  - 증감식은 반복 수행할 문장이 끝날 때마다 실행
  - 초기화식, 증감식은 `,`를 이용하여 둘 이상을 작성 가능
  - 필요하지 않은 부분은 생략 가능 (for(;;): 무한 루프 -> 조건식은 비어있으면 True로 인식)
  ```java
	public static void main(String[] args) {
		for(int i = 0; i < 3; i++) {
			System.out.println(i);
		}
		// 0
		// 1
		// 2
	
		for(int i = 0, j = 10; i < 3; i += 2, j--) {
			System.out.printf("i: %d \n", i);
			System.out.printf("j: %d \n", j);
		}
		// i: 0 
		// j: 10 
		// i: 2 
		// j: 9 
		
		int i = 0;
		for(int i = 1; i < 3; i++) {
			System.out.println(i);
		}
		// 오류 발생 (이미 선언된 변수를 for문에서 다시 선언하려 함)
		
		for(int j = 0; j < 3; j++) {
			System.out.println(j);
		}
		int j = 0;
		// 오류 x (for문이 끝나면서 j가 사라짐)
	}
  ```

- while문
  - `while(조건식) {반복 수행할 문장}`
  - 조건식 생략 불가능
  ```java
  public static void main(String[] args) {
  	int n = 1;
		while(n < 4) {
			System.out.println(n);
			n++;
		}
  }
  // 1
  // 2
  // 3
  ```

- do while문
  - `do {반복 수행할 문장;} while(조건식);`
  - 먼저 반복 수행 문장 수행 후, 조건식 판단 (최소 한번은 수행)
  - 조건식 생략 불가능
  ```java
  public static void main(String[] args) {
  	int n = 3;
		do {
      System.out.println(n);
			n++;
		} while(n < 3);
  }
  // 3
  ```

- break
  - 반복문에서 빠져나올 때 사용
  - 반복문에 이름(라벨)을 붙여 한번에 빠져 나올 수 있음
  ```java
  public static void main(String[] args) {
		for(int n = 0; n < 5; n++) {
			System.out.println(n);
			if(n==2) {
				break;
			}
		}
    // 0
    // 1
    // 2
		
		loop1 :
		for(int a = 0; a < 3; a++) {
			loop2:
			for(int b = 10; b > 7; b--) {
				System.out.println("a="+a + " b="+b);
				if (a==1) {
					break loop1;
				}
			}
		}
    // b=0 c=10
    // b=0 c=9
    // b=0 c=8
    // b=1 c=10
  }
  ```

- continue
  - 반복문의 처음으로 보내기
  - 반복문에 이름(라벨)을 붙여 한번에 빠져 나올 수 있음
  ```java
  public static void main(String[] args) {
		int e = 0;
		loop3:
		while(e < 5) {
			e++;
			if (e % 2 == 0) {
				continue loop3;
			}
			System.out.println(e);
		}
	}
  // 1
  // 3
  // 5
  ```

## 4. 배열(Array)

### 배열
- 같은 종류의 데이터를 저장하기 위한 자료구조
- 크기가 고정되어 있음(한 번 생성된 배열을 크기를 바꿀 수 없음)
- 배열을 객체로 취급(참조형) -> 배열의 요소를 참조
- `배열이름.length'를 통해 배열 길이 조회 가능
- 배열의 선언
  - `타입[] 배열이름` <- 보통 이 형태로 사용
    - ex) int[] arr
  - `타입 배열이름[]`
    - ex) int arr[]
  - 타입 종류: int, char, boolean, String, Date

- 배열의 생성과 초기화
  - `자료형[] 배열이름 = {값1, 값2, 값3};` -> 선언과 동시에 초기화
  - `배열이름 = new 자료형[] {값1, 값2, 값3};` -> 배열 생성 및 값 초기화
  - `배열이름 = new 자료형[길이];` -> 배열 생성(자료형의 초기값으로 초기화)

  |자료형|기본값|
  |---|---|
  |boolean|false|
  |char|공백문자|
  |byte, short, int|0|
  |long|0L|
  |float|0.0f|
  |double|0.0|
  |참조형 변수(String, Date, ...)|null(참조x)|

```java
public static void main(String[] args) {
    int[] score1; // 이 형식으로 사용 권장
    int score2[]; // 이 형식으로는 권장 x
    
    score1 = new int[] {1, 2, 3, 4, 5};
    System.out.println(score3); // 메모리 주소 값 반환
    
    int[] score3 = {1, 2, 3, 4, 5};
    System.out.println(score3[0]); // 1
    
    int[] score4 = new int[3];
    score4[0] = 10;
    score4[1] = 20;
    for(int i = 0; i<score4.length; i++) {
      System.out.println(score4[i]);
    }
    // 10
    // 20
    // 0
}
```