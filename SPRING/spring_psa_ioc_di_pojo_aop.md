> 오늘 알아갈 내용
> - PSA: 이식 가능한 서비스 추상화
> - IoC: 제어의 역전
> - DI: 의존성 주입
> - POJO: 종속되지 않은 상태로 개발하는 개념
> - AOP: 관점 지향 프로그래밍
> - 스프링 빈과 컨테이너


# POJO
: Plain Old Java Object의 줄임말로, **오래된 방식의 간단한 자바 객체**를 뜻한다.
![image](https://github.com/AucSuSu/CS-study/assets/75782242/d8ecfce5-e624-4c26-8917-7bca81aac1aa)


위 이미지는 Spring 삼각형이라는 유명한 이미지로 Spring의 핵심 개념(IoC/DI, AOP, PSA를 통해 POJO 달성 가능)

간단하게 말해서 필드와 Getter, Setter와 같은 기본 기능만을 갖는 기본 객체를 의미


➡️ "특정 기술"(Framework)에 **종속된 상태로 개발하지 않는**, 객체지향적인 원리에 충실하면서 환경과 기술에 종속되지 않고, 필요에 따라 재활용될 수 있는 방식으로 설계된 오브젝트. 특정 기술에 대한 종속성으로 객체지향적 설계가 힘들어 코드의 유지보수와 재사용이 불편해 POJO라는 개념이 탄생했다.



### 1. POJO를 지킨 코드
```java
package com.jpasample.service;

public class POJOClass {
    private String name;
    private int age;

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }


}
```
단순한 getter, setter로 POJO의 개념을 잘 지킴



### 2. POJO를 무시한 코드
```java
public class POJOClass extends UserService{

    private String name;
    private int age;

    @Override
    public List<User> findUsers() {
        return super.findUsers();
    }
}
```
UserService 클래스의 기능을 사용하기 위해 **extends**를 받은 코드

UserService의 메서드를 사용하기 위해 많은 양의 코드를 리팩토링해야 하며, 코드의 가독성이나 유지보수, 확장 측면에서 **어려움이 있음!**


### 3. Spring의 POJO 개념을 지킨 코드
```java
public class POJOClass{

    @Autowired
    UserRepository userRepository;
    
    private String name;
    private int age;
    
    public void test(){
        userRepository.findAll();
    }
}
```
`@Autowired`로 **의존성 주입**을 하여 **느슨한 결합력**을 갖게 되면서, 직접적으로 **extends, implements를 하지 않고도 해당 클래스의 메소드에 접근** 가능

# IoC
: Inversion of Control의 줄임말로, **제어의 역전**을 뜻한다.

### 1. IoC가 적용되지 않은 경우
: 객체를 생성할 때는 객체가 필요한 곳에서 **직접 생성**했을 경우

```java
public class A{
    // 클래스 A에서 new 키워드로 클래스 B의 객체 생성
    b = new B(); 
}
```


### 2. IoC 적용
: 다른 객체를 직접 생성하거나 제어하는 것이 아닌, **외부에서 관리하는 객체를 가져와** 사용하는 것

: 스프링에서는 객체들을 관리하기 위해 제어의 역전을 사용

```java
public class A{
    // 객체를 생성한 것이 아니라, 어디선가 받아온 객체 B를 b에 할당(== 객체를 주입 받음)
    private B b; 
}
```
위의 코드에서는 클래스 B 객체를 직접 생성하는 것이 아니므로, 어디선가 받아와서 사용하고 있다.


➡️ 제어의 역전을 사용하면 객체를 외부에서 관리하고, 실제로 사용할 때는 외부에서 제공해주는 **객체를 받아온다**.

> 💎 객체를 관리하는 주체인 **외부**를 **스프링 컨테이너**라고 한다!


# DI
: Dependency Injection의 줄임말로 **의존성 주입**을 뜻함. 제어의 역전을 구현하기 위해 사용하는 방법
- DI는 어떤 클래스가 다른 클래스에 의존한다는 뜻

### 객체를 주입 받는 모습
```java
public class A {
    // A에서 B를 주입 받음
    @Autowired
    B b;
}
```

- `@Autowired`: 스프링 컨테이너에 있는 빈을 주입하는 어노테이션
    - 해당 어노테이션으로 빈을 주입받을 수 있도록 DI 지원
- `빈`: 스프링 컨테이너에서 관리하는 객체


**스프링 컨테이너에서 객체를 주입**했기 때문에 스프링 컨테이너가 B 객체를 만들어서 클래스 A에 전달
![alt text](image.png)

# 스프링 빈과 컨테이너
스프링은 스프링 컨테이너를 제공한다.
스프링 컨테이너는 빈을 생성하고 관리한다.
즉, 빈이 생성되고 소멸되기까지의 생명주기를 스프링 컨테이너가 관리

## 빈
: 스프링 컨테이너가 생성하고 관리하는 객체
- `@Autowired`로 주입 받은 B 객체가 빈
- 스프링은 빈을 스프링 컨테이너에 등록하기 위해 **XML 파일에서 설정**하거나 **어노테이션으로 등록**하는 등의 방법 제공

### 클래스를 빈으로 등록하는 방법
```java
@Component
public class MyBean{

}
```
1. `@Component` 로 `MyBean` 클래스가 빈으로 등록 
2. 스프링 컨테이너에서 MyBean 클래스를 관리


    2-1. 이 때 빈의 이름은 클래스 이름의 첫 글자를 소문자로 바꿔 관리 => `myBean`을 관리

### 💎 빈 == 스프링의 객체 라고 생각하자!


# AOP
: Aspect Oriented Programming의 줄인 표현, 즉 **관점 지향 프로그래밍**
- 프로그래밍을 관점, 부가 관점으로 나누어서 관심 기준으로 모듈화하는 것
- 부가 관점 코드를 핵심 관점 코드에서 분리
➡️ 개발자가 **핵심 관점 코드에만 집중**할 수 있도록 해주고, 프로그램의 **변경과 확장에도 유연한 대응 가능**


# PSA
: Portable Service Abstraction의 줄임말로, 이식 가능한 추상화 서비스를 뜻한다.

➡️ 스프링에서 제공하는 다양한 기술들을 추상화해 개발자가 쉽게 사용하는 인터페이스

> ex) 스프링에서 데이터베이스에 접근하기 위한 기술로 JPA, MyBatis, JDBC 등이 있다. 여기에서 어떤 기술을 사용하든 일관된 방식으로 데이터베이스에 접근하도록 인터페이스 지원.

# 한 줄 요약
- IoC: 객체의 생성과 관리를 개발자가 하는 것이 아니라 프레임워크가 대신
- DI: 외부에서 객체를 주입받아 사용
- AOP: 프로그래밍할 때 핵심 관점과 부가 관점을 나누어 개발
- PSA: 어느 기술을 사용하던 일관된 방식으로 처리하도록
- POJO: IoC/DI, AOP, PSA로 객체지향적인 프로그래밍을 가능하게 하는 개념

## References.
- https://shinsunyoung.tistory.com/133
- https://ittrue.tistory.com/211
