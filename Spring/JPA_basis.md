# 1. JPA 기본

## 1. JPA란?

### 객체 vs 관계형 데이터베이스

자바에서 관리하는 객체는 SQL 변환을 통해 관계형 데이터베이스에 저장된다.

관계형 데이터베이스에 맞게 객체끼리의 연관관계 또한 **외래키**로 연결시킬 수 있다.

아래의 경우 `Member` 클래스는 `Team` 클래스와 외래키로 연결되어 있다.

```java
class Member {
    Long id;
    String username;
    Long teamId; // Team 클래스와 연관관계를 맺는다.
}
```
```java
class Team {
    Long id;
    String name;
}
```

하지만 객체 중심의 모델링은 객체끼리 **참조**를 통해 연관관계를 맺어야 한다.

따라서 객체 중심 모델링을 하게 되면 `Member` 클래스는 다음과 같이 바뀐다.

```java
class Member {
    Long id;
    String username;
    Team team; // 객체 자체를 참조해 연관관계를 맺는다.
}
```

이러한 경우 `member`는 **객체 상태**인 `team`의 정보를 알고 있어야 하므로, `id = 1`인 `member` 객체를 꺼내오기 위해서는 다음과 같은 번거로움이 발생한다.

1. DB에서 `id`로 `member`를 조회한다.

2. 찾은 `member`의 `teamId`를 이용해 DB에서 `team`을 조회한다.

3. `member` 객체와 `team` 객체의 연관관계를 맺는다.

하지만 만약 이러한 객체를 DB가 아닌 자바 컬렉션에서 관리한다면, `list.get(memberId)` 하나만으로도 `team` 객체를 알고 있는 `member` 객체를 꺼내올 수 있다.

이는 객체 자체를 관계형 데이터베이스에 그대로 저장할 수 없기 때문에 발생하는 문제이다.

이처럼 객체와 관계형 데이터베이스의 **패러다임의 차이**에서 발생하는 문제를 해결해주는 것이 바로 **JPA**이다.

### JPA(Java Persistence API)

자바 진영의 ORM 기술 표준이다.

자바에서 작성한 코드를 적절한 SQL 쿼리로 변환하며 패러다임의 불일치를 해결해준다.

SQL 중심적인 개발에서 객체 중심적인 개발을 가능하게 한다.

특정 데이터베이스에 종속되지 않고, `persistence.xml`에서 지정한 데이터베이스 문법을 따른다.

> **ORM이란?**
> 
> ORM이란 객체 관계 매핑(Object-relational mapping)을 뜻한다.
> 
> **객체는 객체대로, 관계형 데이터베이스는 관계형 데이터베이스대로 설계**되도록 ORM 프레임워크가 중간에서 매핑한다.

### JDBC

JDBC(Java DataBase Connectivity)란 자바 내에서 SQL문을 실행하기 위한 API이다.

JDBC DriverManager가 DBMS에 맞는 드라이버를 생성하여 JDBC API에 맞게 동작할 수 있게 처리한다.

따라서 JDBC를 사용하면 DBMS 종류에 상관없이 동일한 방식으로 DB를 조작할 수 있다.

하지만 간단한 SQL을 실행하는 데도 중복된 코드를 반복적으로 사용해야 한다는 단점이 있다.

이런 번거로움은 **Persistence Framework**를 이용해 해결할 수 있다.

> **Persistence Framework**
> 
> Persistence Framework에는 SQL Mapper와 ORM이 있다.
> 
> SQL Mapper는 SQL문을 직접 사용해 DB를 조작하고, ORM은 메소드를 이용해 DB를 조작한다.
> 
> SQL에는 Mybatis, JdbcTemplate이 있고, ORM에는 JPA, Hibernate 등이 있다.

### Hibernate

JPA는 특정 기능을 하는 라이브러리가 아니고 ORM을 사용하기 위한 인터페이스를 모아둔 것이다.

따라서 JPA를 사용하기 위해서는 JPA의 구현체인 Hibernate, EclipseLink, DataNucleus 같은 ORM 프레임워크를 사용해야 한다.

그 중 가장 범용적으로 다양한 기능을 제공하는 **Hibernate**를 주로 사용한다.

Hibernate가 지원하는 메소드 내부에서는 JDBC API가 동작한다.

## 2. 영속성 관리

### 영속성 컨텍스트

영속성 컨텍스트란 **엔티티를 영구 저장하는 환경**이라는 뜻이다. (DB에 저장되지는 않음)

**엔티티 매니저 팩토리**에서 **엔티티 매니저**를 생산한다.

엔티티 매니저는 영속성 컨텍스트에 엔티티를 보관하고 관리한다.

- **J2SE** 환경: 엔티티 매니저와 영속성 컨텍스트가 1:1 관계
- **J2EE, 스프링 프레임워크** 같은 컨테이너 환경: 엔티티 매니저와 영속성 컨텍스트가 N:1 관계

<img src="https://user-images.githubusercontent.com/109272360/220949956-a439612f-182b-4592-8bb2-add89a53b49a.png">

```java
// EntityManagerFactory 생성
EntityManagerFactory emf = Persistence.createEntityManagerFactory(persistenceUnitName);

// EntityManager 생성
EntityManager em = emf.createEntityManager();

// 트랜잭션 생성
// JPA에서 데이터를 변경하는 작업은 모두 트랜잭션 안에서 이루어져야 한다.
EntityTransaction tx = em.getTranscation();

// 트랜잭션 시작
tx.begin();

try {
	// 데이터 조작 코드 작성

	// 트랜잭션 commit
	tx.commit();
} catch (Exception e) {
	// 에러 발생시 트랜잭션 rollback
	tx.rollback();
} finally {
	// 엔티티 매니저 닫기
	em.close();
}

// 엔티티 매니저 팩토리 닫기
emf.close();
```

### 엔티티의 생명주기
- 비영속 (new/transient)
	- 영속성 컨텍스트와 전혀 관계가 없는 새로운 상태
	<img src="https://user-images.githubusercontent.com/109272360/220951574-6171ac0d-2f06-4bc8-83e2-ef51e40aa64d.png">
	```java
	// 객체를 생성한 상태(비영속)
	Member member = new Member();
	member.setId(1);
	member.setUsername("member1");
	```

- 영속 (managed)
	- 영속성 컨텍스트에 관리되는 상태
	- `persist()` 메소드를 통해 영속 상태로 만들 수 있다.
	<img src="https://user-images.githubusercontent.com/109272360/220951624-69a63e22-6f7f-46b3-8659-39c2335dfbf7.png">
	```java
	// 객체를 생성한 상태(비영속)
	Member member = new Member();
	member.setId(1);
	member.setUsername("member1");

	// 객체를 저장한 상태(영속)
	// 주의: DB에 저장된 것은 아님
	em.persist(member);

	// DB에 쿼리 날리기
	tx.commit();
	```
- 준영속 (detached)
	- 영속성 컨텍스트에 저장되었다가 분리된 상태
	- `em.detach(member);`

- 삭제 (removed)
	- 삭제된 상태
	- `em.remove(member);`

### 영속성 컨텍스트의 이점
- 1차 캐시
	- 엔티티 매니저는 우선적으로 데이터를 1차 캐시에서 찾는다.
	- 1차 캐시에 데이터가 없으면 DB를 조회한다.
	- 만약 데이터가 1차 캐시에 없고 DB에 있다면, DB에서 조회한 데이터를 1차 캐시에 저장한 뒤 반환한다.
	<img src="https://user-images.githubusercontent.com/109272360/220956381-fd7a52d5-7f6c-4bed-9264-1aa4ebde4ae6.png">
	```java
	Member member = new Member();
	member.setId(1);
	member.setUsername("member1");

	// 1차 캐시에 저장
	em.persist(member);

	// 1차 캐시에서 조회
	Member findMember1 = em.find(Member.class, 1);

	// DB에서 조회
	Member findMember2 = em.find(Member.class, 2);
	```

- 영속 엔티티의 동일성(identity) 보장
	- DB에서는 같은 데이터를 각각 다른 변수에 저장할 경우 두 변수는 다른 변수로 취급된다.
	- 하지만 영속성 컨텍스트에서 찾은 데이터는 다른 변수에 저장되어도 같은 변수로 취급된다.
	```java
	Member a = em.find(Member.class, 1);
	Member b = em.find(Member.class, 1);
	System.out.println(a == b); // true
	```

- 트랜잭션을 지원하는 쓰기 지연(transactional write-behind)
	- `persist()` 메소드 실행 시 1차 캐시에 저장함과 동시에 **쓰기 지연 SQL 저장소**에 SQL 쿼리를 저장한다.
	- `commit()` 메소드 실행 시 저장했던 SQL 쿼리를 DB에 한 번에 날린다.
	<img src="https://user-images.githubusercontent.com/109272360/220956228-d27bc958-b247-406b-9947-aab4dea129f8.png">
	```java
	EntityManagerFactory emf = Persistence.createEntityManagerFactory(persistenceUnitName);
	EntityManager em = emf.createEntityManager();
	EntityTransaction tx = em.getTranscation();
	tx.begin();

  // 영속성 컨텍스트에 저장 & 쓰기 지연 SQL 저장소에 저장
	em.persist(memberA);
	em.persist(memberB);

	// DB에 저장 & 저장했던 SQL 쿼리 날리기
	tx.commit();
	```

- 변경 감지(Dirty Checking)
	- 영속 엔티티를 조회할 때의 스냅샷을 저장해, `commit()` 시 기존 스냅샷과 비교한 뒤 자동 수정
	<img src="https://user-images.githubusercontent.com/109272360/220958175-039db935-68a0-4a74-983f-2a3fb5d2f45b.png">
	```java
	EntityManagerFactory emf = Persistence.createEntityManagerFactory(persistenceUnitName);
	EntityManager em = emf.createEntityManager();
	EntityTransaction tx = em.getTranscation();
	tx.begin();

	// 영속 엔티티 조회 & 스냅샷 저장
	Member memberA = em.find(Member.class, 1);

	// 영속 엔티티 데이터 수정
	memberA.setUsername("changedName");

	// DB 저장 & 스냅샷과 비교
	transaction.commit();

	//em.update(member) 이런 코드 없이도 변경을 감지해 자동 수정
	```

- 지연 로딩(Lazy Loading)

### 플러시(flush)

영속성 컨텍스트의 변경 내용을 데이터베이스에 반영하는 것을 뜻한다.

쓰기 지연 SQL 저장소의 쿼리를 데이터베이스에 전송한다.

(영속성 컨텍스트를 비우지는 않음)

- 영속성 컨텍스트를 플러시하는 방법
	- `em.flush()`: 직접 호출
	- `tx.commit()`: 플러시 자동 호출
	- JPQL 쿼리 실행: 플러시 자동 호출

- 플러시 모드 옵션

	`em.setFlushMode(모드)`을 통해 플러시 모드 지정

	- `FlushModeType.AUTO`: 커밋이나 쿼리를 실행할 때 플러시 (기본값)

	- `FlushModeType.COMMIT`: 커밋할 때만 플러시

### 준영속 상태

영속 상태의 엔티티가 영속성 컨텍스트에서 분리된 상태

영속성 컨텍스트가 제공하는 기능을 사용하지 못한다.

- 준영속 상태로 만드는 방법
	- `em.detach(entity)`: 특정 엔티티만 준영속 상태로 변환
	- `em.clear()`: 영속성 컨텍스트를 초기화
	- `em.close()`: 영속성 컨텍스트를 종료

## 3. 엔티티 매핑

### 객체와 테이블 매핑

`@Entity`가 붙은 클래스는 JPA가 관리하는 엔티티가 된다.

기본 생성자(파라미터가 없는 public 또는 protected)를 필수로 사용해야 한다.

`name` 속성을 이용해 JPA에서 사용할 엔티티 이름을 지정할 수 있다. (기본값: 클래스 이름)

`@Table` 어노테이션을 이용해 엔티티와 매핑할 테이블을 지정할 수 있다.

- `@Table` 속성
	- `name`: 매핑할 테이블 이름(기본값: 엔티티 이름)
	- `catalog`: 데이터베이스 catalog 매핑
	- `schema`: 데이터베이스 schema 매핑
	- `uniqueConstraints(DDL)`: DDL 생성 시에 유니크 제약 조건 생성

```java
@Entity
// @Entity(name = "Member") 같은 클래스명이 없으면 기본값 사용 권장
// @Table(name = "MBR") 매핑할 테이블의 이름을 직접 지정할 수 있다.
public class Member {

}
```

### 데이터베이스 스키마 자동 생성

Hibernate는 DB 스키마 자동 생성 기능을 제공한다.

`persistence.xml` 파일 내 `hibernate.hdm2ddl.auto` property를 생성하면 스키마 자동 생성을 이용할 수 있다.

즉 **`@Entity`를 이용해 엔티티로 설정한 클래스는 별도의 SQL문 없이도 DB에 스키마를 자동으로 생성**한다.

`value`에 따른 기능은 다음과 같다.

- `create`: 기존 테이블 삭제 후 다시 생성 (DROP + CREATE)
- `create-drop`: create와 같으나 종료시점에 테이블 DROP
- `update`: 변경분만 반영
- `validate`: 엔티티와 테이블이 정상 매핑되었는지만 확인
- `none`: 사용하지 않음

**주의: 운영 장비에는 절대 create, create-drop, update를 사용하면 안된다.**

### 필드와 컬럼 매핑

다음과 같은 어노테이션을 통해 필드와 컬럼을 매핑할 수 있다.

- `@Column`
	- 컬럼과 매핑
	
	|속성|설명|기본값|
	|--|--|--|
	|`name`|필드와 매핑할 테이블 컬럼 이름|객체 필드 이름|
	|`insertable`, `updatable`|등록, 변경 가능 여부|TRUE|
	|`nullable`(DDL)|false: not null, true: null 허용||
	|`unique`(DDL)|유니크 제약 조건||
	|`columnDefinition`(DDL)|컬럼 정보 직접 작성||
	|`length`(DDL)|문자 길이 제약 조건(String에만 사용)|255|
	|`precision`, `scale`(DDL)|BigDecimal, BigInteger에 사용|precision=19, scale=2|

- `@Enumerated`
	- 자바 enum 타입을 매핑할 때 사용
	- 속성
		- `EnumType.ORDINAL`: enum 순서를 데이터베이스에 저장 (기본값)
		- `EnumType.STRING`: enum 이름를 데이터베이스에 저장
	- **주의: ORDINAL 사용 x (이후 enum 순서가 바뀌면 혼동 가능)**

- `@Temporal`
	- 날짜 타입(java.util.Date, java.util.Calendar)을 매핑할 때 사용
	- LocalDate, LocalDateTime을 사용할 때는 생략 가능 (최신 하이버네이트 지원)
	- 속성
		- `TemporalType.DATE`: date 타입과 매핑 (예: 2023-02-24)
		- `TemporalType.TIME`: time 타입과 매핑 (예: 10:12:30)
		- `TemporalType.TIMESTAMP`: timestamp 타입과 매핑 (예: 2023-02-24 10:12:30)

- `@Lob`
	- 데이터베이스 BLOB, CLOB 타입과 매핑
	- CLOB: String, char[], java.sql.CLOB
	- BLOB: byte[], java.sql.BLOB
	
- `@Transient`
	- 메모리상에서만 임시로 어떤 값을 보관하고 싶을 때 사용
	- 데이터베이스에 저장 및 조회되지 않는다.

```java
package hellojpa; 

import javax.persistence.*; 
import java.time.LocalDate; 
import java.time.LocalDateTime; 
import java.util.Date; 

@Entity 
public class Member { 

		@Id 
		private Long id; 

		@Column(name = "name") 
		private String username; 

		private Integer age; 

		@Enumerated(EnumType.STRING) 
		private RoleType roleType; 

		@Temporal(TemporalType.TIMESTAMP) 
		private Date createdDate; 

		@Temporal(TemporalType.TIMESTAMP) 
		private Date lastModifiedDate; 

		private LocalDateTime localDateTime;

		@Lob 
		private String description; 

		@Transient
		private Integer tmp;
}
```

### 기본 키 매핑

`@Id`을 이용해 기본 키를 매핑할 수 있다.

`@Id`만 사용하면 키를 직접 할당할 수 있고, `@GeneratedValue`를 사용하면 키를 자동으로 생성한다.

`@GeneratedValue` 내 `strategy`를 지정해 어떤 전략으로 키를 자동 생성할지 결정할 수 있다.

- **IDENTITY 전략**
	- 키 생성을 데이터베이스에 위임한다.
	- 주로 MySQL, PostgreSQL, SQL Server, DB2에서 사용한다. (예: AUTO_INCREMENT)
	- 키 생성을 DB에 위임하다보니 INSERT 이후에만 해당 객체의 id 값을 알 수 있다는 단점이 있다.
	- 따라서 IDENTITY 전략에서는 INSERT SQL을 `tx.commit()`이 아닌 `em.persist()` 시점에 실행해 id를 영속성 컨텍스트에 저장한다.
	- 따라서 `persist()` 이후에는 객체의 id를 `객체.id`로 직접 조회할 수 있다.

	```java
	@Entity 
	public class Member { 
			@Id 
			@GeneratedValue(strategy = GenerationType.IDENTITY) 
			private Long id;
	}
	```
	```java
	...

	try {

		Member member = new Member(); // 아직 id 모름

		// INSERT 실행
		em.persist(member);
		System.out.println(member.id); // id 조회 가능

		tx.commit();
	}
	...
	```

- **SEQUENCE 전략**
	- 데이터베이스 시퀀스란 자동으로 순차적으로 증가하는 순번을 반환하는 데이터베이스 객체이다.
	- 오라클, PostgreSQL, DB2, H2 데이터베이스에서 사용한다.
	- 테이블마다 다른 시퀀스를 사용하고 싶다면 클래스에 `@SequenceGenerator()`을 사용한다.
	- `em.persist()` 시점에 DB에서 다음 시퀀스 번호를 받아와 영속성 컨텍스트에 id를 저장한다. (IDENTITY와는 달리 INSERT를 날리지는 않음)
	- 결국 `persist()`마다 DB를 거쳐야한다는 단점이 있는데, 이는 `allocationSize`를 지정해 해결할 수 있다.
	- `allocationSize`에서 설정한 값만큼 DB에서 미리 시퀀스 값을 받아와 메모리에서 사용하며, DB에는 그 다음 시퀀스 값을 저장한다.
	
	```java
	@Entity 
	@SequenceGenerator( 
					name = "MEMBER_SEQ_GENERATOR", // 식별자 생성기 이름(필수) 
					sequenceName = "MEMBER_SEQ", // 매핑할 데이터베이스 시퀀스 이름
					initialValue = 1, allocationSize = 50) // 기본값: initialValue: 1, allocationSize: 50
	public class Member { 
			@Id 
			@GeneratedValue(strategy = GenerationType.SEQUENCE, 
							generator = "MEMBER_SEQ_GENERATOR") 
			private Long id; 
	}
	```
	```java
	...

	try {

		// 아직 id 모름
		Member member = new Member1();
		Member member = new Member2();
		Member member = new Member3();

		em.persist(member1);
		// call next value for Member_SEQ ->  DB 시퀀스 1로 설정
		// call next value for Member_SEQ -> DB 시퀀스 51로 설정
		em.persist(member2); // 메모리에서 2 가져오기 (DB 조회 x)
		em.persist(member3); // 메모리에서 3 가져오기 (DB 조회 x)
		System.out.println(member1.id); // 1 (id 조회 가능)
		System.out.println(member2.id); // 2 (id 조회 가능)
		System.out.println(member3.id); // 3 (id 조회 가능)

		tx.commit();
	}
	...
	```

- **TABLE 전략**
	- 키 생성 전용 테이블을 하나 만들어서 데이터베이스 시퀀스를 흉내낸다.
	- 시퀀스 전략과 동일하게 `initialValue`, `allocationSize` 등을 사용할 수 있다.
	- 모든 데이터베이스에 적용할 수 있다.
	- 다른 전략에 비해 성능이 떨어진다는 단점이 있다.

	```sql
	create table MY_SEQUENCES ( 
			sequence_name varchar(255) not null, 
			next_val bigint, 
			primary key ( sequence_name ) 
	)
	```
	```java
	@Entity 
	@TableGenerator(
					name = "MEMBER_SEQ_GENERATOR", 
					table = "MY_SEQUENCES", 
					pkColumnValue = "MEMBER_SEQ", allocationSize = 1) 
	public class Member { 
			@Id 
			@GeneratedValue(strategy = GenerationType.TABLE, 
							generator = "MEMBER_SEQ_GENERATOR") 
			private Long id; 
	}
	```