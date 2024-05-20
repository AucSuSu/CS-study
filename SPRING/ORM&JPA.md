## ORM(Object Relational Mapping) 이란?
- ORM은 애플리케이션의 Class와 RDB(Relational DataBase)의 테이블을 매핑(연결)한다는 뜻.
- 기술적으로는 객체를 RDB 테이블에 자동으로 영속화 해주는 것.
- RDB(관계형 데이터베이스)의 Table과 자바 객체를 매핑시켜 좀 더 객체지향적인 개발을 가능하게 해주는 기술이다.
- 객체와 테이블과간의 패러다임의 불일치를 해결하는 기술


## JPA란?
- JPA(Java Persistence API)는 자바 진영에서 ORM 기술 표준으로 사용되는 인터페이스의 모음.
- 실제적으로 구현된 것이 아니라, 구현된 클래스와 매핑을 시켜주기 위해 사용하는 프레임워크
- **대표적인 오픈소스로는 Hibernate가 있다.**

![image](https://github.com/AucSuSu/CS-study/assets/109134365/ba033128-ca4d-48d9-ae56-d063b0ff917d)

JPA는 애플리케이션과 JDBC 사이에서 동작한다. 그래서 JPA를 쓴다해서 JDBC를 사용 안하는것이 아니다.

## 왜 JPA를 사용해야 할까?
> JPA는 SQL 중심이 아닌 객체 중심으로 개발하기 때문에, 생산성이 좋아지고 유지보수가 수월하다.

아래의 예시를 보자

![image](https://github.com/AucSuSu/CS-study/assets/109134365/8a8da19b-9f43-426a-8f58-ee3c42fec6ec)


이런 테이블 구조에서 Album 클래스를 저장한다고 가정해보자

JPA를 사용하게 되면 insert 작업을 아래처럼 할 수 있다.

```java
// Album 객체저장
jpa.persist(album);
```

```sql
// 변환된 쿼리
INSERT INTO ITEM (ID, NAME, PRICE) .....
INSERT INTO ALBUM (ARTIST) .....
```

Album을 insert 하게 되면 Album의 부모인 ITEM 객체도 Inset하게 된다.

조회도 마찬가지 이다.
```java
// JAVA 코드
String albumId = "id100";
Album album = jpa.find(Album.class, albumId);
```

```sql
// 변환된 쿼리
SELECT I.*, A.*
  FROM ITEM I
  JOIN ALBUM A ON I.ITEM_ID = A.ITEM_ID
```

자식인 Album 객체를 조회하게되면 부모인 Item에서도 조회를 수행하게 된다.


다음으로, 필드로 객체를 가진 연관관계를 보자.

![image](https://github.com/AucSuSu/CS-study/assets/109134365/67881b13-9d69-4fae-b683-a74637a4b4fd)

Member 클래스가 Team 클래스를 필드로 가지고있다. 코드로 나타내면 아래와 같다.

```java
class Member {
 String id;
 Team team;
 String username;
}


class Team {
 Long id;
 String name;
}
```

member를 조회하게 되면 team도 가져온다.
```java
// JAVA 코드
Member member = jpa.find(Member.class, memberId);
Team team = member.getTeam();
```

```sql
// 변환된 쿼리
SELECT M.*, T.*
 FROM MEMBER M
 JOIN TEAM T ON M.TEAM_ID = T.TEAM_ID
```

## JPA의 특징
### 1. 1차 캐시와 동일성 보장

JPA는 동일한 트랜잭션에서 조회한 엔티티는 같은 엔티티를 반환한다 -> 약간의 조회 성능 향상

```java
String memberId = "100";
Member member1 = jpa.find(Member.class, memberId); // SQL
Member member2 = jpa.find(Member.class, memberId); // 캐시

member1 == member2; //같다.
```

### 2. 트랜잭션을 지원하는 쓰기 지연 - INSERT
JPA는 트랜잭션을 커밋할 때까지 INSET SQL을 쏘지않고, 모은다.
JDBC BATCH SQL 기능을 사용해서 한번에 SQL을 전송한다.

```java
transaction.begin(); // [트랜잭션] 시작
em.persist(memberA);
em.persist(memberB);
em.persist(memberC);
//여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.
//커밋하는 순간 데이터베이스에 INSERT SQL을 모아서 보낸다.
transaction.commit(); // [트랜잭션] 커밋
```

### 3. 트랜잭션을 지원하는 쓰기 지연 - UPDATE
1. UPDATE, DELETE로 인한 행(ROW) 락(LOCK) 시간 최소화


**이게 무슨말임???**

원래 DB에 접근하는 트랜잭션 단위에서 트랜잭션이 열리기 시작하고 닫힐때까지 그 데이터(행)에 대한 Lock이 걸린다.

하지만 JPA를 쓰게되면 트랜잭션의 커밋 전까지 Lock이 걸리지않고, 커밋할때만 LOCK이 걸리므로 락 시간을 최소화 한다.

```java
transaction.begin(); // [트랜잭션] 시작
changeMember(memberA); 
deleteMember(memberB); 
비즈니스_로직_수행(); //비즈니스 로직 수행 동안 DB 로우 락이 걸리지 않는다. 
//커밋하는 순간 데이터베이스에 UPDATE, DELETE SQL을 보낸다.
transaction.commit(); // [트랜잭션] 커밋
```

### 4. 지연 로딩과 즉시 로딩
- 지연 로딩 : 객체가 실제 사용될 때 로딩
- 즉시 로딩 : JOIN SQL로 한번에 연관된 객체까지 미리 조회

![image](https://github.com/AucSuSu/CS-study/assets/109134365/6d3e0b3a-d523-4c5d-b8d4-708e18a48ace)

지연 로딩에서는 team.getName()을 실행할 때 select를 하지만, 즉시 로딩은 find member를 할때 select가 이루어진다.


## ETC
스프링에서 이때까지 사용해왔던 JPA는 JPA를 이용하는 Spring-Data_Jpa 프레임워크이지 JPA는 아니다.

![image](https://github.com/AucSuSu/CS-study/assets/109134365/8caff160-1904-4991-93e7-fe6225de9632)
