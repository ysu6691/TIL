# Java 고급

## 1. Generic

제네릭은 컴파일 시 타입을 체크해주는 기능으로, 메소드, 클래스, 인터페이스 등에서 **타입을 파라미터로 사용**할 수 있게 해준다.

즉, 미리 정해진 타입을 사용하지 않고 **필요에 의해 타입을 지정할 수 있다.**

초기 선언 시 `< 타입 파라미터 >` 형태로 작성한다.

예시: `public class ClassName<T>{}`

- 타입 파라미터: 특별한 의미의 알파벳보다는 단순히 임의의 참조형 타입을 말함 (관례적으로 아래와 같이 주로 사용)
  - T: reference Type
  - E: Element
  - K: Key
  - V: Value
  - N: Number

제네릭을 사용할 때는 변수 쪽과 생성 쪽의 타입을 반드시 맞춰야 한다.

```java
package generic;

class ClassName<T> {

}

public class GenericTest {
	public static void main(String[] args) {
		ClassName<String> generic1 = new ClassName<String>();

    // 생성자에 타입을 생략하면 변수의 타입을 따라간다.
		ClassName<String> generic2 = new ClassName<>();

    // 타입을 명시하지 않으면 경고를 낸다. (에러는 발생 x)
		ClassName generic3 = new ClassName();
	}
}
```

### 제네릭 사용 이유

제네릭은 **객체 타입에 대한 안전성 향상** 및 **형변환의 번거로움을 감소**시키는 효과가 있다.

예시: 미리 타입을 `Obejct`로 지정한 클래스와 제네릭을 사용한 클래스의 멤버 변수를 각각 `String` 타입의 변수에 저장하기

- generic을 사용하지 않는 경우

  ```java
  package generic;

  public class NormalBox {
    // 타입을 Object로 지정
    private Object some;

    public Object getSome() {
      return some;
    }

    public void setSome(Object some) {
      this.some = some;
    }
  }
  ```

  ```java
  package generic;

  public class UseBoxTest {
    public static void main(String[] args) {
      NormalBox nbox = new NormalBox();

      // 모든 객체 형태로 변환이 가능하다.
      nbox.setSome("Hello");

      // some을 바로 String으로 지정하는 것이 불가능하다.
      Object some = nbox.getSome();
      if (some instanceof String) {
        String someStr = (String) some;
        System.out.println(someStr);
      }
    }
  }
  ```

- generic을 사용하는 경우

  ```java
  package generic;

  public class GenericBox<T> {
    private T some;

    public T getSome() {
      return some;
    }

    public void setSome(T some) {
      this.some = some;
    }
  }
  ```

  ```java
  package generic;

  public class UseBoxTest {
    public static void main(String[] args) {
      GenericBox<String> gbox = new GenericBox<String>();

      // String으로의 변환만 가능하다.
      gbox.setSome("Hello");

      // 바로 String으로 타입 지정이 가능하다.
      String some = gbox.getSome();
      System.out.println(some);
    }
  }
  ```

### Type parameter의 제한

필요에 따라 구체적인 타입 제한이 필요한 상황이 발생한다.

이러한 경우 타입 파라미터 선언 뒤 `extends`와 함께 상위 타입을 명시한다.

```java
package generic;

// Number 이하의 타입 (Byte, Short, Integer, ...)로 제한
class NumberBox<T extends Number> {
	
}

public class UseBoxTest {
	public static void main(String[] args) {
		NumberBox<Integer> nbox = new NumberBox<Integer>();
	}
}
```

### 와일드 카드

- Generic type<?>: 타입에 제한이 없음

- Generic type<? extends T>: T 또는 T를 상속받은 타입들만 사용 가능

- Generic type<? super T>: T 또는 T의 조상 타입만 사용 가능

```java
package generic;

class Person {
	
}

class Student extends Person {
	
}

class PersonBox<T> {
	
}

public class WildCardTest {
	public static void main(String[] args) {
		PersonBox<Object> pobj = new PersonBox<>();
		PersonBox<Person> pper = new PersonBox<>();
		PersonBox<Student> pst = new PersonBox<>();
		
		// 모든 타입 지정 가능
		PersonBox<?> pAll = pobj;
		pAll = pper;
		pAll = pst;
		
		// Person 또는 Person을 상속 받은 경우에만 가능
		PersonBox<? extends Person> pChild = pper;
		pChild = pst;
		// pChild = pobj; // 에러 발생
		
		// Person 또는 Person의 조상만 가능
		PersonBox<? super Person> pSuper = pper;
		pSuper = pobj;
		// pSuper = pst; // 에러 발생
	}
}
```

## 2. Collection Framework

객체들을 한 곳에 모아놓고 편리하게 사용할 수 있는 환경을 제공한다.

`java.util` 패키지에서 지원한다.

- 정적 자료구조
  - 고정된 크기의 자료구조
  - ex) 배열

- 동적 자료구조
  - 요소의 개수에 따라 자료구조의 크기가 동적으로 증가하거나 감소
  - ex) 리스트, 스택, 큐 등

### Collection Framework의 인터페이스

- List
  - 순서가 있는 데이터의 집합
  - 중복 허용
  - ex) ArrayList, LinkedList, stack, queue

- Set
  - 순서를 유지하지 않는 데이터의 집합
  - 중복 허용 x
  - ex) HashSet, TreeSet

- Map
  - key, value 쌍으로 데이터를 관리하는 집합
  - 순서가 없다.
  - key: 중복 허용 x / value: 중복 허용
  - ex) HashMap, TreeMap

### 주요 메소드
  
- **List**
  - 추가
    - `add(E element)`: 리스트에 요소 추가
    - `add(int index, E element)`: 특정 인덱스에 요소 추가
    - `addAll(Collection c)`: Collection의 객체들을 추가
  - 조회
    - `contains(Object o)`: 해당 객체를 포함하는지 확인
    - `get(int index)`: 특정 인덱스에 존재하는 요소 반환
    - `indexOf(Object o)`: 해당 객체의 인덱스 반환
    - `isEmpty()`: 비어있는지 확인
    - `size()`: 요소의 총 개수 반환
    - `iterator()`: iterator를 반환
  - 삭제
    - `remove(Object o)`: 해당 객체 제거하고 boolean 반환
    - `remove(int index)`: 해당 인덱스의 요소 제거
    - `clear()`: 모든 요소 제거
  - 수정
    - `set(int index, E element)`: 해당 인덱스 요소를 전달받은 객체로 대체
  - 기타
    - `equals(Object o)`: 리스트와 해당 객체가 같은지 확인
    - `toArray()`: 모든 요소를 Object 타입의 배열로 반환

- **Set**
  - 추가
    - `add(E element)`: 리스트에 요소 추가
    - `addAll(Collection c)`: Collection의 객체들을 추가
  - 조회
    - `contains(Object o)`: 해당 객체를 포함하는지 확인
    - `isEmpty()`: 비어있는지 확인
    - `size()`: 요소의 총 개수 반환
    - `iterator()`: iterator를 반환
  - 삭제
    - `remove(Object o)`: 해당 객체 제거하고 boolean 반환
    - `clear()`: 모든 요소 제거
  - 기타
    - `equals(Object o)`: 셋과 해당 객체가 같은지 확인
    - `toArray()`: 모든 요소를 Object 타입의 배열로 반환

- **HashMap**
  - 추가
    - `put(K key, V value)`: key, value 매핑
  - 조회
    - `containsKey(Object key)`: 해당 key를 포함하는지 확인
    - `containsValue(Object value)`: 해당 value를 포함하는지 확인
    - `get(Object key)`: 해당 key에 대응하는 값 반환, 키가 없다면 null 반환
    - `isEmpty()`: 비어있는지 확인
    - `size()`: 요소의 총 개수 반환
    - `iterator()`: iterator를 반환
  - 삭제
    - `remove(Object key)`: 해당 key에 해당하는 매핑 제거하고 value 반환
    - `remove(Object key, Object value)`: 해당 value에 대응하는 key의 매핑 제거
    - `clear()`: 모든 매핑 제거
  - 수정
    - `replace(K key, V oldValue, V newValue)`: oldValue에 대응하는 key의 value를 newValue로 대체
  - 기타
    - `equals(Object o)`: 맵과 해당 객체가 같은지 확인

### Comparable interface & Comparator interface

```java
public class Person {
	String name;
	String pNum;
	
	public Person(String name, String pNum) {
		this.name = name;
		this.pNum = pNum;
	}
}
```

```java
package comparable;

import java.util.List;
import java.util.ArrayList;
import java.util.Collections;

public class SortTest {
	public static void main(String[] args) {
		List<Person> list = new ArrayList<>();
		
		list.add(new Person("김", "1"));
		list.add(new Person("이", "2"));
		list.add(new Person("박", "3"));
		
		Collections.sort(list); // 에러
	}
}
```

위 예시에서 `Person` 클래스는 정렬을 지원하지 않기 때문에 `sort()` 메소드를 사용할 수 없다.

이러한 경우 `Comparable` 인터페이스를 상속받아 사용자 정의 정렬을 구현할 수 있다.

`Comparable` 인터페이스를 상속받은 클래스는 `compareTo()` 메소드를 이용해 override 함으로써 정렬을 구현한다.

즉, `sort()` 메소드를 실행했을 때 객체를 비교할 수 있는 기준이 생기게 된다.

`compareTo()`는 한 개의 인자를 받아, **자기 자신과 인자를 비교**한다.

- 메소드를 호출하는 객체가 인자로 들어온 객체보다 큰 경우: 양수 return
- 메소드를 호출하는 객체가 인자로 들어온 객체와 같은 경우: 0 return
- 메소드를 호출하는 객체가 인자로 들어온 객체보다 작은 경우: 음수 return

```java
package comparable;

public class Person implements Comparable<Person> {
	String name;
	String pNum;
	
	public Person(String name, String pNum) {
		this.name = name;
		this.pNum = pNum;
	}
	
  // 정렬기준을 pNum으로 정의
	@Override
	public int compareTo(Person o) {
		return this.pNum.compareTo(o.pNum);
	}
  	
	// 출력을 위한 override
	@Override
	public String toString() {
		return "name=" + name + ", pNum=" + pNum;
	}
}
```

```java
package comparable;

import java.util.List;
import java.util.ArrayList;
import java.util.Collections;

public class SortTest {
	public static void main(String[] args) {
		List<Person> list = new ArrayList<>();

		list.add(new Person("김", "1"));
		list.add(new Person("이", "2"));
		list.add(new Person("박", "3"));

    // pNum을 기준으로 오름차순으로 정렬
		Collections.sort(list);

		for (Person p : list) {
			System.out.println(p);
      // name=김, pNum=1
      // name=이, pNum=2
      // name=박, pNum=3
		}
	}
}
```

또는 다음과 같이 `Comparator`를 이용해 정렬할 수도 있다.

`Comparator`는 `compare()` 메소드를 이용해 정렬을 구현한다.

`compare()`는 두 개의 인자를 받아, **두 인자를 비교**한다.

```java
package comparable;

import java.util.List;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

public class SortTest {
	public static void main(String[] args) {
		List<Person> list = new ArrayList<>();
		
    // Comparator 생성
		Comparator<Person> comparator = new Comparator<Person>() {
			@Override
			public int compare(Person p1, Person p2) {
        // pNum을 오름차순으로 정렬
				return p1.pNum.compareTo(p2.pNum);
			}
		};

		list.add(new Person("김", "1"));
		list.add(new Person("이", "2"));
		list.add(new Person("박", "3"));

    // 두 번째 인자로 comparator 사용
		Collections.sort(list, comparator);

    // 아래와 같이 익명 클래스를 이용해 일회성으로 정렬할 수도 있다.
    // Collections.sort(list, new Comparator<Person>() {
		// 	@Override
		// 	public int compare(Person o1, Person o2) {
		// 		return o1.pNum.compareTo(o2.pNum);
		// 	}
		// });

		for (Person p : list) {
			System.out.println(p);
			// name=김, pNum=1
			// name=이, pNum=2
			// name=박, pNum=3
		}
	}
}
```

## 3. 예외처리

### 에러와 예외
- 에러(Error)
  - 메모리 부족, stack overflow와 같이 일단 발생하면 복구할 수 없는 상황
  - 프로그램의 비정상적 종료를 막을 수 없음

- 예외(Exception)
  - 읽으려는 파일이 없거나 네트워크 연결이 안되는 등 수습될 수 있는 비교적 상태가 약한 것들
  - 프로그램 코드에 의해 수습될 수 있는 상황

### 예외 처리

예외 발생 시 프로그램의 비정상종료를 막고 정상적인 실행 상태를 유지하는 것을 뜻한다.

예외 감지 및 예외 발생 시 동작할 코드 작성이 필요하다.

- 예외 클래스의 계층
  - Error 계열
  - Checked exception 계열: 예외에 대한 대처 코드가 없으면 컴파일이 진행되지 않음
  - Unchecked exception 계열(RuntimeException의 하위 클래스): 예외에 대한 대처 코드가 없더라도 컴파일은 진행됨

- 예외 처리 키워드
  - 직접 처리
    - try
    - catch
    - finally
  - 간접 처리
    - throws
  - 사용자 정의 예외처리
    - throw

### try ~ catch ~ finally 구문

예외가 발생했을 때 처리할 코드를 작성할 수 있다.

JVM은 try 블록에서 예외가 발생했을 때 해당 Exception 클래스 객체를 생성해 catch 블록으로 던진다(throw).

던져진 exception을 처리할 수 있는 catch 블록이 없으면 예외 처리는 실패한다.

만약 finally 블록이 있다면, 예외 발생 여부와 관계없이 실행할 코드를 적을 수 있다.

중간에 return을 만나는 경우도 finally 블록을 먼저 수행 후 return을 실행한다.

```java
try {
    // 예외가 발생할 수 있는 코드
} catch (Exception e) {
    // 예외가 발생할 때 처리할 코드
} finally {
    // 예외 발생 여부와 관계없이 실행할 코드
}
```

try 블록에서 여러 종류의 예외가 발생하는 경우, 하나의 try 블록에 여러 개의 catch 블록을 추가할 수 있다.

이때 JVM이 던진 예외가 catch 문장을 찾을 때는 **다형성이 적용**되므로, 상위 타입의 예외가 먼저 선언되는 경우 뒤에 등장하는 catch 블록은 동작하지 않는다.

### Exception 객체의 정보 활용

Throwable의 주요 메소드

- `public String getMessage()`: 발생된 예외에 대한 구체적인 메시지를 반환
- `public Throwable getCause()`: 예외의 원인이 되는 Throwable 객체 또는 null을 반환
- `public void printStackTrace()`: 예외가 발생된 메소드가 호출되기까지의 메소드 호출 스택을 출력 (디버깅의 수단으로 주로 사용)

```java
package exception;

public class ExceptionTest {
	public static void main(String[] args) {
		int[] nums = { 1 };
		
		try {
			System.out.println(nums[2]);
		} catch (ArrayIndexOutOfBoundsException e) {
			System.err.println("배열의 크기를 벗어남");
			System.out.println(e.getMessage()); // 2
			e.printStackTrace();
			// java.lang.ArrayIndexOutOfBoundsException: 2
			// at exception.ExceptionTest.main(ExceptionTest.java:8)
		}
		
		System.out.println("프로그램을 종료합니다.");
		// 프로그램을 종료합니다.
	}
}
```

### throws

`throws` 키워드를 이용해 예외 처리를 위임할 수 있다.

예외를 전달받은 메소드는 다시 예외 처리의 책임이 발생하고, 직접 처리할지 아니면 다시 위임할지 정할 수 있다.

- checked exception과 throws

  `throws`를 명시해야 한다.

  ```java
  package exception;

  public class CheckedExceptionTest {
    public static void main(String[] args) {
      try {
        method1();
      } catch (ClassNotFoundException e) {
        e.printStackTrace();
      }
    }

    public static void method1() throws ClassNotFoundException {
      method2();
    }
    
    public static void method2() throws ClassNotFoundException {
      Class.forName("ysu");
    }
  }
  ```

- Unchecked Exception과 throws

  `throws`를 명시하지 않아도 전달이 된다.

  ```java
  package exception;

  public class UnCheckedExceptionTest {
    public static void main(String[] args) {
      try {
        method1();
      } catch (Exception e) {
        e.printStackTrace();
      }
    }

    public static void method1() {
      method2();
    }
    
    public static void method2() {
      int i = 1 / 0;
    }
  }
  ```

- override와 throws

  하위 클래스에서 메소드를 override하면, 조상클래스 메소드가 던지는 예외보다 부모 예외를 던질 수 없다.

  ```java
  class Parent {
    void methodA() throws IOException{}
    void methodB() throws ClassNotFoundException{}
  }

  // Parent를 상속받는 클래스 생성
  public class OverridingTest extends Parent {

    // 하위 예외는 던질 수 있다.
    @Override
    void methodA() throws FilNotFoundException{}

    // 부모 예외는 던질 수 없다.
    @Override
    void methodB() throws Exception {}
  }
  ```

### 사용자 정의 예외

대부분 Exception 또는 RuntimeException 클래스를 상속받아 작성한다.

- check exception 활용
  - 명시적 예외 또는 throws 필요
  - 코드는 복잡해지지만 처리 누락 등 오류 발생 가능성은 줄어든다.
- runtime exception 활용
  - 묵시적 예외 처리 가능
  - 코드가 간결해지지만 예외 처리 누락 가능성 발생

```java
package exception;

public class FruitNotFoundException extends Exception {
	public FruitNotFoundException(String name) {
		super(name + "에 해당하는 과일이 없습니다.");
	}
}
```

```java
package exception;

public class CustomExceptionTest {
	private static String[] fruits = {"사과", "오렌지", "토마토"};
	
	public static void main(String[] args) {
		try {
			getFruit("배");
		} catch (FruitNotFoundException e) {
			e.printStackTrace();
		}
	}
	
	private static void getFruit(String name) throws FruitNotFoundException {
		for (int i = 0; i < fruits.length; i++) {
			if (fruits[i] != null && fruits[i].equals(name)) {
				fruits[i] = null;
				return;
			}
		}
		throw new FruitNotFoundException(name);
	}
}

// exception.FruitNotFoundException: 배에 해당하는 과일이 없습니다.
// 	 at exception.CustomExceptionTest.getFruit(CustomExceptionTest.java:21)
// 	 at exception.CustomExceptionTest.main(CustomExceptionTest.java:8)
```
