# 프록시 패턴(Proxy Pattern)이란?
대상 원본 객체를 **대리하여 대신 처리**하게 함으로써 로직의 흐름을 제어하는 행동 패턴


![image](https://github.com/AucSuSu/CS-study/assets/75782242/794c6ed1-3374-491b-8c0f-e997675429ed)

- 클라이언트가 대상 객체를 직접 쓰는 것이 아니라 중간에 프록시를 거쳐서 쓰는 코드 패턴
- 대상 객체(subject)의 메소드를 직접 실행하는 것이 아니라, 대상 객체에 접근하기 전에 프록시 객체의 메서드를 접근한 후 추가적인 로직을 처리한 뒤 접근



### 사용하는 이유
> 대상 클래스(subject)가 민감한 정보를 가지고 있거나,<br>
> 인스턴스화하기에 무겁거나,<br>
> 기능을 추가하고 싶은데 원본 객체를 수정할 수 없는 경우


위와 같은 상황을 극복하기 위해 사용한다.



### 프록시 사용의 효과
1. **보안**: 프록시가 클라이언트의 권한을 확인하여 클라이언트의 요청을 대상(subject)으로 전달
2. **캐싱**: 프록시가 내부 캐시를 유지하여 데이터가 캐시에 아직 존재하지 않는 경우에만 대상에서 작업이 실행되도록
3. **데이터 유효성 검사**: 프록시에서 클라이언트의 입력의 유효성을 검사하여 대상으로 전달
4. **지연 초기화**: 대상의 생성 비용이 비싸다면 프록시는 필요할 때까지 연기할 수 있: 다
5. **로깅**: 메소드 호출과 상대 매개 변수를 인터셉트하고 기록
6. **원격 객체**: 원격 위치에 있는 객체를 가져와서 로컬처럼 보이도록



# 프록시 패턴의 구현
<img width="787" alt="image" src="https://github.com/AucSuSu/CS-study/assets/75782242/376dfb28-8788-4dca-9ac8-3c6ca6290d41">


- **Subject**: RealSubject와 Proxy를 통해 구현할 수 있는 인터페이스
- **RealSubject**: 원본 대상 객체
- **Proxy**: 실제 객체와 동일한 인터페이스를 구현하지만, 추가적인 작업(접근 제어, 지연 로딩) 수행
  - 프록시는 흐름제어만 할 뿐, 결과값을 조작하거나 변경하면 안됨
  - RealSubject를 중계하는 대리자 역할
- **Client**: Subject 인터페이스를 이용해 프록시 객체 생성해 이용
    - 프록시를 통해서 RealSubject와 데이터를 주고 받는다




# 프록시 디자인 패턴의 대표적인 세 가지 유형

## 가상 프록시
생성 비용이 많이 드는 객체가 실제로 필요할 때까지 **객체 생성을 지연**하는 데 사용
- 실제 객체의 생성에 많은 자원이 소모되지만 사용 빈도가 낮을 때 사용하는 방식
> ex) 매우 큰 이미지 파일 처리할 때, 이 이미지 파일을 실제로 사용자가 요청할 때까지 로딩하지 않고, 가상 프록시를 통해 먼저 가벼운 객체를 로드해 시스템 자원 사용 최적화


```java
interface ISubject {
    void action();
}

class RealSubject implements ISubject {
    public void action() {
        System.out.println("원본 객체 액션 !!");
    }
}

class Proxy implements ISubject {
    private RealSubject subject; // 대상 객체를 composition

    Proxy() {
    }

    public void action() {
    	// 프록시 객체는 실제 요청(action(메소드 호출)이 들어 왔을 때 실제 객체를 생성한다.
        if(subject == null){
            subject = new RealSubject();
        }
        subject.action(); // 위임
        /* do something */
        System.out.println("프록시 객체 액션 !!");
    }
}

class Client {
    public static void main(String[] args) {
        ISubject sub = new Proxy();
        sub.action();
    }
}
```



## 보호 프록시
객체에 대한 **접근 제어**하는 데 사용
- 객체의 메서드에 대한 접근 권한을 검사 -> 사용자의 권한에 따라 메서드 실행 여부 결정
- 클라이언트의 자격 증명이 기준과 일치하는 경우에만 서비스 객체에 요청 전달
> ex) 특정 사용자가 비밀번호를 변경할 권한이 있는지 검사 후 접근을 허용하거나 차단 가능

```java
interface ISubject {
    void action();
}

class RealSubject implements ISubject {
    public void action() {
        System.out.println("원본 객체 액션 !!");
    }
}

class Proxy implements ISubject {
    private RealSubject subject; // 대상 객체를 composition
    boolean access; // 접근 권한

    Proxy(RealSubject subject, boolean access) {
        this.subject = subject;
        this.access = access;
    }

    public void action() {
        if(access) { // 접근 제한 !
            subject.action(); // 위임
            /* do something */
            System.out.println("프록시 객체 액션 !!");
        }
    }
}

class Client {
    public static void main(String[] args) {
        ISubject sub = new Proxy(new RealSubject(), false);
        sub.action();
    }
}
```



## 원격 프록시
프록시 객체는 로컬에, 대상 객체(RealSubject)는 원격 서버에 존재하는 경우
- 프록시 객체는 네크워크를 통해 클라이언트의 요청을 전달해, 네트워크와 관련된 불필요한 작업 처리 후 결과값만 반환
- 클라이언트 입장에서는 프록시를 통해 객체를 이용하는 것이기 때문에 원격이든 로컬이든 신경 쓸 필요가 없으며, 프록시가 진짜 객체와 통신 대리
> ex) 다른 서버에 있는 데이터베이스에 대한 요청을 중계하고 결과를 클라이언트에 전달
<img width="635" alt="image" src="https://github.com/AucSuSu/CS-study/assets/75782242/f2bcd674-438e-4fd1-b538-61bae1a02f94">


> - 프록시 == 스터브<br>
> - 스켈레톤: 프록시로부터 전달된 명령을 이해하고 적합한 메소드를 호출해주는 역할을 하는 보조 객체


# 장단점
### 장점
- 개방 폐쇄 원칙 준수: **기존 대상 객체의 코드를 변경하지 않고** 새로운 기능 추가 가능
- 단일 책임 원칙 준수: 대상 객체는 자신의 기능에만 집중하고, **부가 기능을 제공하는 역할을 프록시 객체에 위임** -> 다중 책임 회피 가능
- 원래 하려던 기능을 수행하며 부가적인 작업(로깅, 인증, 네트워크 통신) 수행에 유용
- 클라이언트는 객체를 신경쓰지 않고, 서비스 객체 제어하거나 생명 주기 관리 가능
- 사용자 입장에서는 프록시 객체나 실제 객체나 사용법 유사 -> 사용성에 문제 X

### 단점
- 많은 프록시 클래스 도입 -> 코드 복잡도 증가
  - 각 클래스에 해당되는 프록시 클래스 만들어서 적용해야 하기 때매
  - 자바에서는 동적 프록시 기법으로 해결 가능
- 프록시 클래스 자체에 들어가는 자원이 많다면 서비스로부터 응답이 늦어질 수 있다.


# Reference
- https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%ED%94%84%EB%A1%9D%EC%8B%9CProxy-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90
