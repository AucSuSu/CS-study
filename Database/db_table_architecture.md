## DB 테이블 설계 과정
> 하향식 방법, 상향식 방법

### 하향식 방법
- 타사의 서비스를 참고해 벤치마킹
- 장점
  - 빠르게 엔티티를 도출
  - 설계의 안정성
- 단점
  - 근거 부족
  - 서비스 변화에 대한 유연한 대처 불가

### 상향식 방법
- 기획안에 나온 요구사항을 분석해 실체 엔티티를 먼저 도출
- 장점
  - 근거가 명확
  - 변경에 용이한 엔터티 설계
- 단점
  - 시간이 오래 걸림

## 상향식 방법으로 설계
1. 기획안을 참고해 모든 **키워드** 추출
2. 추출한 키워드를 **행위** 와 **데이터**로 구분
   - 행위와 데이터는 행위 엔티티, 실체 엔티티로 매핑
   - 모든 행위나 데이터가 DB에 담겨야 하는 것은 아님! (서버에서 ENUM으로 관리하는 데이터가 있을 수 있음)
3. 2-1에서 설계한 엔티티에 **관계를 매핑**
   - 관계는 JOIN을 염두에 둘 것

## 식별 관계와 비식별 관계
### 식별 관계
- 부모 테이블의 기본키를 자식 테이블의 기본키(PK)로 설정
- 구조 변경에 어려움

### 비식별 관계
- 부모 테이블의 기본키를 자식 테이블의 외래키(FK)로 설정
- 구조 변경에 용이

-> 현업에서는 식별 관계보다 비식별 관계 선호.(구조 변경에 용이)

## 1대1 관계, 1대다 관계, 다대다 관계
### 1대1 관계
- 학생-학번
- 학생은 하나의 학번을 가진다.
- 학번은 하나의 학생을 가진다.

### 1대다 관계
- 학생-학교
- 학생은 하나의 학교에 소속된다.
- 학교는 여러 명의 학생을 가질 수 있다.

### 다대다 관계
- 학생-수업
- 각 테이블의 PK를 학생-수업 관계 테이블의 FK로 사용하여 연결
- 즉, 1대다-관계 테이블-다대1 관계가 되는 것
![다대다관계](https://github.com/AucSuSu/CS-study/assets/64372881/e6600026-dd1f-4126-b5fa-935a2f2dd426)


## 쿼리문
### 학생 테이블 생성
```sql
create table student
(    student_id    INT          NOT NULL,
     student_name  VARCHAR(100) NOT NULL,
     gender        CHAR(1)          NULL,
     age           TINYINT          NULL,
     class         TINYINT          NULL,
     PRIMARY KEY   (student_id)           );
)
```
create table <테이블 명>
(<column명> <data type> <NULL 허용 여부>
...
PRIMARY KEY(<pk로 삼을 column명>));

### 테이블 값 추가
```sql
insert into    student (student_id, student_name, gender, age, class)
     values    (1, '홍길동', 'M', 17, 1);
```
insert into <테이블 명>
values (값1, 값2,...);

### 참고
- https://yeongunheo.tistory.com/entry/DB-%EC%84%A4%EA%B3%84%ED%95%98%EB%8A%94-%EB%B2%95-feat-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EB%AA%A8%EB%8D%B8%EB%A7%81
- https://velog.io/@ek1816/SQL-%EA%B8%B0%EC%B4%88-1
