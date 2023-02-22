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

> **참고: ORM이란?**
> 
> ORM이란 객체 관계 매핑(Object-relational mapping)을 뜻한다.
> 
> **객체는 객체대로, 관계형 데이터베이스는 관계형 데이터베이스대로 설계**되도록 ORM 프레임워크가 중간에서 매핑한다.

#