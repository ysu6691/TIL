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

<img src="https://user-images.githubusercontent.com/109272360/220949956-a439612f-182b-4592-8bb2-add89a53b49a.png" width="700px">

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

	<img src="https://user-images.githubusercontent.com/109272360/220951574-6171ac0d-2f06-4bc8-83e2-ef51e40aa64d.png" width="600px">

	```java
	// 객체를 생성한 상태(비영속)
	Member member = new Member();
	member.setId(1);
	member.setUsername("member1");
	```

- 영속 (managed)
	- 영속성 컨텍스트에 관리되는 상태
	- `persist()` 메소드를 통해 영속 상태로 만들 수 있다.

	<img src="https://user-images.githubusercontent.com/109272360/220951624-69a63e22-6f7f-46b3-8659-39c2335dfbf7.png" width="600px">

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

	<img src="https://user-images.githubusercontent.com/109272360/220956381-fd7a52d5-7f6c-4bed-9264-1aa4ebde4ae6.png" width="700px">

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

	<img src="https://user-images.githubusercontent.com/109272360/220956228-d27bc958-b247-406b-9947-aab4dea129f8.png" width="700px">

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

`@GeneratedValue` 내 `strategy`를 지정해 어떤 전략으로 키를 자동 생성할지 결정할 수 있다. (`AUTO`: DBMS에 따라 자동 지정, 기본값)

- **IDENTITY 전략**
	- 키 생성을 데이터베이스에 위임한다.
	- 주로 MySQL, PostgreSQL, SQL Server, DB2에서 사용한다. (예: AUTO_INCREMENT)
	- 키 생성을 DB에 위임하다보니 INSERT 이후에만 해당 객체의 id 값을 알 수 있다는 단점이 있다.
	- 따라서 IDENTITY 전략에서는 INSERT SQL을 `tx.commit()`이 아닌 `em.persist()` 시점에 실행해 id를 영속성 컨텍스트에 저장한다.
	- 따라서 `persist()` 이후에는 객체의 id를 직접 조회할 수 있다.

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
	    System.out.println(member.getId()); // id 조회 가능
    
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

## 4. 연관관계 매핑

### 단방향 연관관계

<img src="https://user-images.githubusercontent.com/109272360/221084821-7f9b8ab9-ef54-43ff-a3ec-7f1e0352e318.png" width="550px">

```java
@Entity
public class Member {

    @Id @GeneratedValue
    private Long id;

    @Column(name = "USERNAME")
    private String name;
    
    private int age;
    
		// Team과의 연관관계 생성
    @ManyToONE
    @JoinColumn(name = "TEAM_ID") // 테이블에서의 외래키
    private Team team

		// getter, setter
}
```

```java
@Entity
public class Team {

    @Id @GeneratedValue
    private Long id;

    private String name;

		// getter, setter
}
```

- 연관관계 저장하기
	```java
	// 팀 저장
	Team team = new Team();
	team.setName("TeamA");
	em.persist(team);

	// 회원 저장
	Member member = new Member();
	member.setName("member1");
	member.setTeam(team); // 단방향 연관관계 설정
	em.persist(member);
	```

- 참조로 연관관계 조회하기
	```java
	// 조회
	Member findMember = em.find(Member.class, member.getId());

	// 참조를 사용해서 연관관계 조회
	Team findTeam = findMember.getTeam();
	```

- 연관관계 수정
	```java
	// 새로운 팀 저장
	Team teamB = new Team();
	teamB.setName("TeamB");
	em.persist(teamB);

	// 회원1에 새로운 팀 설정
	// persist() 없이도 자동 반영
	member.setTeam(teamB);
	```

### 양방향 연관관계

<img src="https://user-images.githubusercontent.com/109272360/221085893-86898fb8-4a98-43c5-9d42-1b9ffa7d8f2a.png" width="550px">

```java
@Entity
public class Member {

    @Id @GeneratedValue
    private Long id;

    @Column(name = "USERNAME")
    private String name;
    
    private int age;
    
		// Team과의 연관관계 생성
    @ManyToONE
    @JoinColumn(name = "TEAM_ID")
    private Team team

		// getter, setter
}
```

```java
@Entity
public class Team {

    @Id @GeneratedValue
    private Long id;

    private String name;

		// 양방향 매핑
		// mappedBy: 반대편 엔티티가 참조하는 이름
		@OneToMany(mappedBy = "team")
		List<Member> members = new ArrayList<Member>();

		// getter, setter
}
```

- 양방향 매핑
	```java
  Team team = new Team();
  team.setName("teamA");
  em.persist(team);

  Member member = new Member();
  member.setUsername("member1");
  member.setAge(20);
	member.setTeam(team);
  em.persist(member);

	// flush & clear 작업을 하지 않으면 엔티티 매니저는 team을 영속성 컨텍스트에서 가져온다.
	// 영속성 컨텍스트에서의 team은 바뀐 member를 인식하지 못한다. (member에만 team을 추가했기 때문)
	// 따라서 flush 후 영속성 컨텍스트를 비우면 DB에서 갱신된 team을 가져올 수 있다.
	em.flush();
	em.clear();

  Team findTeam = em.find(Team.class, team.getId());
	int memberSize = findTeam.getMembers().size(); // 역방향 조회
	```

> **양방향 매핑 시 주의할 점**
> 
> 양방향으로 매핑하더라도 다음과 같이 `member` 객체에서만 `team` 객체를 저장하는 경우, `team` 객체는 `member` 객체를 인지하지 못한다.
> 
> ```java
> // team 생성
> Team team = new Team();
> team.setName("teamA");
> em.persist(team);
> 
> // member 생성
> Member member = new Member();
> member.setUsername("member");
> member.setAge(20);
> member.setTeam(team); // teamA와의 연관관계 생성
> em.persist(member);
> 
> // member -> team 조회 성공
> Team findTeam = member.getTeam();
> System.out.println(findTeam.getName()); // teamA
> 
> // team -> member 조회 실패
> System.out.println(team.getMembers()); // null
> ```
> 
> 따라서 순수 객체 상태를 유지하기 위해서는 항상 **양쪽에 값을 설정**해야 한다.
> 
> ```java
> Team team = new Team();
> team.setName("teamA");
> em.persist(team);
> 
> Member member = new Member();
> member.setUsername("member");
> member.setAge(20);
> member.setTeam(team);
> em.persist(member);
> 
> // 역방향으로도 저장
> team.getMembers().add(member);
> 
> // member -> team 조회 성공
> Team findTeam = member.getTeam();
> System.out.println(findTeam.getName()); // teamA
> 
> // team -> member 조회 성공
> List<Member> findMembers = team.getMembers();
> for (Member findMember : findMembers) {
>     System.out.println(findMember.getUsername()); // member
> }
> ```
>
> 이는 한 객체의 setter를 양방향 매핑에 맞게 고침으로써 간편하게 적용가능하다.
> 
> ```java
> @Entity
> public class Team {
> 		...
> 
> 		// setMembers 대신 작성
> 		// 양방향 모두 저장
>     public void changeMembers(Member member) {
>         members.add(member);
>         member.setTeam(this);
>     }
> }
> ```
> ```java
> Member member = new Member();
> member.setUsername("member1");
> member.setAge(20);
> em.persist(member);
> 
> Team team = new Team();
> team.setName("teamA");
> team.changeMembers(member);
> em.persist(team);
> 
> // 역방향으로 저장할 필요 x
> // member.setTeam(team);
> 
> // member -> team 조회 성공
> Team findTeam = member.getTeam();
> System.out.println(findTeam.getName()); // teamA
> 
> // team -> member 조회 성공
> List<Member> findMembers = team.getMembers();
> for (Member findMember : findMembers) {
>     System.out.println(findMember.getUsername()); // member1
> }
> ```

### 연관관계의 주인

객체끼리 연관관계를 맺기 위해서는 한 객체가 다른 객체의 정보를 갖고 있어야 한다.

따라서 다른 객체와 연관관계를 맺을 **연관관계 주인**을 정해야 하는데, 연관관계 주인은 주로 **테이블 상에서 외래키를 갖고 있는 곳**으로 정한다.

(비지니스 로직을 기준으로 정하지 않는다.)

이렇게 연관관계 주인을 정한 뒤, 연관관계 주인이 맺은 단방향 매핑만으로도 연관관계 매핑은 완료된다.

따라서 우선적으로 단방향 매핑 후 양방향 매핑은 필요 시 추가해도 무방하다.

(양방향 매핑은 반대 방향으로의 조회 기능이 추가된 것 뿐)

### 다양한 연관관계 매핑

- **일대일 (1:1)**
	- 외래키를 갖고 있는 객체에 `@OneToOne` 사용
	- 양방향 매핑 시 반대편 객체에 `@OneToOne(mappedBy = "참조하는 속성명")` 사용

- **다대일 (N:1)**
	- 외래키를 갖고 있는 객체(N)에 `@ManyToOne` 사용
	- 양방향 매핑 시 반대편 객체(1)에 `@OneToMany(mappedBy = "참조하는 속성명")` 사용

- **다대다 (N:M)**
	- `@ManyToMany`이 있지만 실무에서는 거의 사용 x

		다대다로 연결된 테이블은 중개 테이블을 생성한다.

		이때 중개 테이블은 두 엔티티의 id를 갖고 있는데, 여기에 추가적인 컬럼이 필요할 수도 있다.

		이러한 경우 `@ManyToMany`의 사용이 불가능하다.

		따라서 **중개 테이블용 엔티티를 추가한 뒤 두 엔티티와 연결**시킨다.

	- 중개 테이블용 엔티티에서 N:M으로 연결시키고자 하는 두 엔티티를 `@ManyToOne`으로 추가한다.

		(두 테이블의 id는 중개 테이블이 갖고 있음)

		두 엔티티는 `@OneToMany(mappedBy = "참조하는 속성명")`으로 중개 테이블을 추가한다.
	
### 상속관계 매핑

상속관계 매핑이란 슈퍼타입 서브타입 논리 모델을 실제 물리 모델로 구현하는 방법이다.

슈퍼타입 엔티티는 `@Inheritance(strategy=InheritanceType.타입명)`으로 슈퍼타입임을 명시한다.

서브타입 엔티티는 슈퍼타입 엔티티를 `extends`로 상속받는다.

```java
@Entity
@Inheritance(strategy=InheritanceType.타입명)
// 슈퍼타입 테이블에 현재 데이터가 어떤 서브타입의 데이터인지 정보를 추가할 수 있다.
// @DiscriminatorColumn(name="xxx") (name 안 적을 시 기본값: DTYPE)
public class Item {
	
	@Id @GeneratedValue
	private Long id;

	private String name;
	private int price;

	// getter, setter
}
```

```java
@Entity
// 슈퍼타입 테이블의 DTYPE 컬럼에 현재 서브타입을 어떻게 명시할지 정할 수 있다.
// 어노테이션 사용하지 않으면 기본값: 현재 엔티티명
// @DiscriminatorValue("xxx")
public class Album extends Item {
    private String artist;

		// artist에 대한 getter, setter
}
```

<img src="https://user-images.githubusercontent.com/109272360/221390690-f88c1d5b-1581-4b5b-8c7f-9cceb234fdaa.png" width="550px">

- 조인 전략
	- `JOINED` type 사용
	- 장점
		- 테이블 정규화
		- 외래키 참조 무결성 제약조건 활용 가능
		- 저장공간 효율화
	- 단점
		- 조회시 조인을 많이 사용 -> 성능 저하
		- 조회 쿼리가 복잡함
		- 데이터 저장시 INSERT SQL 2번 호출

	<img src="https://user-images.githubusercontent.com/109272360/221390730-a3443d4f-3650-4dda-b236-e8da8693966d.png" width="550px">

- 단일 테이블 전략
	- `SINGLE_TABLE` type 사용
	- `@DiscriminatorColumn`을 명시하지 않아도 `DTYPE`이 테이블에 자동으로 들어감
	- 장점
		- 조인이 필요 없으므로 일반적으로 조회 성능이 빠름
		- 조회 쿼리가 단순함
	- 단점
		- 자식 엔티티가 매핑한 컬럼은 모두 null 허용
		- 단일 테이블에 모든 것을 저장하므로 상황에 따라 조회 성능이 오히려 느려질 수 있음
	
	<img src="https://user-images.githubusercontent.com/109272360/221390803-9faf1a5e-9f3c-4caf-b994-debea92c636d.png" width="200px">

- 구현 클래스마다 테이블 전략
	- `TABLE_PER_CLASS` type 사용
	- 슈퍼타입 엔티티를 `abstract`를 이용해 추상 클래스로 생성
	- 데이터베이스 설계자와 ORM 전문가 둘 다 추천하지 않는 방식
	- 장점
		- 서브 타입을 명확하게 구분해서 처리할 때 효과적
		- not null 제약조건 활용 가능
	- 단점
		- 여러 자식 테이블을 함께 조회할 때 성능이 느림
		- 자식 테이블을 통합해서 쿼리하기 어려움

	<img src="https://user-images.githubusercontent.com/109272360/221390892-5fb38e90-fb5f-4967-bda3-1164aa36204b.png" width="550px">

### MappedSuperclass

두 개 이상의 엔티티가 공통 속성을 갖고 있을 때, 각 엔티티마다 속성을 명시하기 번거로울 수 있다.

이런 상황에서 공통 속성을 한 엔티티가 관리하고 다른 엔티티들이 상속을 받을 수 있다.

공통 속성을 갖는 엔티티에 `@MappedSuperclass`를 사용하면 상속받는 클래스에 속성만 제공하며, 부모 클래스는 조회, 검색이 불가능하다.

직접 생성해서 사용할 일이 없으므로 **추상 클래스**로 생성하는 것을 권장한다.

<img src="https://user-images.githubusercontent.com/109272360/221391797-a6cff766-2979-4c38-9e58-a306fdb031a5.png" width="600px">


```java
@MappedSuperClass
public abstract class BaseEntity {
    private String createBy;
    private LocalDateTime createdDate;
		private String lastModifiedBy;
		private LocalDateTime lastModifiedDate;

		// getter, setter
}
```

```java
@Entity
public class Member extends BaseEntity {
    private String name;

		// getter, setter
}
```

## 5. 값 타입

JPA의 데이터 타입을 분류하면 다음과 같다.

- 엔티티 타입
  - `@Entity`로 정의하는 객체
  - 데이터가 변해도 식별자로 지속해서 추적 가능
- 값 타입
  - int, Integer, String 처럼 단순히 값으로 사용하는 자바 기본 타입이나 객체
  - 식별자가 없고 값만 있으므로 변경시 추적 불가

값 타입은 다시 다음과 같이 분류할 수 있다.

- 기본값 타입
  - 자바 기본 타입(int, double)
  - 래퍼 클래스(Integer, Long)
  - String
- 임베디드 타입(embedded type, 복합 값 타입)
- 컬렉션 값 타입(collection value type)

값 타입은 **생명 주기를 엔티티에 의존**한다.

또한 하나의 값 타입이 여러 곳에서 사용되지 않도록 **절대 공유하지 않아야 한다.**

(한 객체의 속성을 변경했을 때 다른 객체의 속성이 같이 변경되는 side effect 발생 가능)

### 기본값 타입

자바 기본 타입, 래퍼 클래스, String이 기본값 타입에 해당된다.

### 임베디드 타입

새로운 값 타입을 직접 정의할 수 있다.

만약 `Member` 엔티티가 `city`, `street`, `zipcode` 속성을 가지고 있다면, 이 세 속성은 `Address`라는 타입으로 묶어서 관리할 수 있다.

`@Embeddable`로 값 타입을 정의하고, `@Embedded`로 정의한 값 타입을 사용할 수 있다.

생성한 임베디드 타입은 **기본 생성자를 필수로 넣어야 한다.**

임베디드 타입은 재사용성과 높은 응집도를 가지고 있어, 타입 내부에서 해당 타입에 관한 메소드를 작성할 수 있다.

임베디드 타입을 사용해도 테이블의 컬럼에는 차이가 없다.

<img src="https://user-images.githubusercontent.com/109272360/221508147-b1f5aca9-366e-4ee0-8d42-fa0d55baa705.png" width="400px">

```java
@Entity
public class Member {
    
    @Id @GeneratedValue
    private Long id;

    private String name;

    @Embedded
    private Address address;

	// getter, setter
}
```

```java
@Embeddable
public class Address {

    private String city;
    private String street;
    private String zipcode;

    // 기본 생성자를 필수로 넣어야 한다.
    public Address() {

	}

    public Address(String city, String street, String zipcode) {
        this.city = city;
        this.street = street;
        this.zipcode = zipcode;
    }

    // getter, setter
}
```

```java
Member member = new Member();
// setter의 인자로 해당 임베디드 타입을 넣는다.
member.setAddress(new Address("city", "street", "zipcode"));
```

또한 임베디드 타입은 엔티티를 속성으로 가질 수 있다.

```java
@Entity
public class PhoneServiceProvider {

    @Id @GeneratedValue
    @Column(name = "PHONE_SERVICE_PROVIDE_ID")
    private Long id;

    private String name;

	private PhoneNumber phoneNumber;

    // getter, setter
}
```
```java
@Embeddable
public class PhoneNumber {

    private String number;

    // 엔티티를 속성으로 가질 수 있다.
    @ManyToOne
    @JoinColumn(name = "PHONE_SERVICE_PROVIDER_ID")
    private PhoneServiceProvider provider;

    public PhoneNumber() {

    }

    public PhoneNumber(String number, PhoneServiceProvider provider) {
        this.number = number;
        this.provider = provider;
    }
	
	// getter, setter
}
```
```java
@Entity
public class Member {

    @Id @GeneratedValue
    private Long id;

    @Embedded
    private PhoneNumber phoneNumber;

    // getter, setter
}
```

```java
PhoneServiceProvider provider = new PhoneServiceProvider();
provider.setName("xxx");
em.persist(provider);

Member member =  new Member();
member.setPhoneNumber(new PhoneNumber("010-0000-0000", provider));
em.persist(member);
```

한 엔티티에서 같은 값 타입을 사용하면 컬럼 명이 중복되게 된다.

이런 경우 `@AttributeOverrides`, `@AttributeOverride`을 사용해서 컬럼명을 재정의해야한다.

```java
@Entity
public class Member {
    ...

    @Embedded
    private Address homeAddress;

    @Embedded
    @AttributeOverrides(
            @AttributeOverride(name="city",
                    column=@Column(name = "WORK_CITY")),
            @AttributeOverride(name="street",
                    column=@Column(name = "WORK_STREET")),
            @AttributeOverride(name="zipcode",
                    column=@Column(name = "WORK_ZIPCODE"))
    )
    private Address workAddress;

    ...
}
```