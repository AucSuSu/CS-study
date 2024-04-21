# SingleTon

![](https://refactoring.guru/images/patterns/content/singleton/singleton-2x.png?id=accb2cc7594f7a491ce01dddf0d2f876)

## 0. Singleton pattern의 개념

#### 객체의 인스턴스가 오직 1개만 생성되는 패턴

- 생성자가 여러번 호출되도, 실제로 생성되는 객체는 **하나** 이며 최초로 생성된 이후에 호출된 생성자는 이미 생성한 객체를 반환

### 싱글톤 패턴을 사용하는 이유

1. 메모리 측면
   - 이미 생성된 인스턴스를 활용하기 때문에 메모리 절약
2. 다른 클래스 간에 데이터 공유가 쉽다
   - 싱글톤 인스턴스는 모두 전역임

### 언제 사용하나요?

- 인스턴스가 절대적으로 한 개만 존재하는 것을 보증해야할 때
- 리소스를 많이 차지하는 역할의 인스턴스를 만들때 (메모리 낭비가 적다고 했죠?!)

#### 예시 > 데이터베이스에서 커넥션풀, 스레드풀, 캐시, 로그 기록 객체

- :star: 데이터베이스에 접속하는 작업(I/O 바운드)은 그 자체로 무거운 작업에 속하며 또한 한번만 객체를 생성하고 돌려쓰면 되는 인스턴스임
- :star: 실제로 안드로이드 스튜디오 자바 SDK에서 각 액티비티 들이나, 클래스마다 주요 클래스들을 하나하나 전달하는게 번거롭기 때문에 싱글톤 클래스를 만들어 어디서든 접근하도록 설계

## 1. 구현

- SingleTon의 구현 방법은 여러개임

### (1) 정적 변수 선언과 동시에 인스턴스 생성하기 (eager initialization)

- 기본 SingleTon 생성 방식
- 해당 리소스를 사용하지 않는다면 자원 낭비된다는 문제

```java
public class Singleton {
    private static Singleton instance = new Singleton();

    private Singleton() {
    }

    public static Singleton getInstance() {
        return instance;
    }
}
```

### (2) Lazy initalization

- 처음에 필드를 생성할때는 초기화하지 않고, getInstance() 메소드를 호출하게 되면 그때 생성

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {
    }


    public static Singleton getInstance() {
        // instance 가 null 일 때만 생성
        if (instance == null) {
            instance = new Singleton();
        }
        // null이 아니면 기존 static instance 반환
        return instance;
    }
}
```

```java
// 사용할때
Singleton singleton = Singleton.getInstance();
```

#### 위와같이 (2번같이) 구현할 경우에 어떤 문제점이 생길 수 있을까?

- Multi-thread환경에서 동시에 getInstance()를 호출할 경우 ? Singleton의 인스턴스가 한개가 되어야 한다는 보증이 깨짐 => **Thread Safe하지 않음**

### (3) synchronzied 사용하기

- 최초로 접근한 쓰레드가 해당 메소드 호출을 종료할 때까지 다른 쓰레드가 접근하지 못하도록 lock -> 호출할때마다 synchronzied
- getInstance() 메소드를 호출할 때마다 lock이 걸려 성능 저하가 발생한다는 문제 존재 O => 오버헤드

```java
public class Singleton {
    public class Singleton {
        private static Singleton instance;

        private Singleton() {}

        public static synchronzied Singleton getInstance() {
            if(instance  == null) {
                instance  = new Singleton();
            }
            return instance;
        }
    }
}
```

### (4)Double checked locking

- 초기화 할 때만 동기화 작업을 수행(기존 synchronized보다 나음)

```java
public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}


```

#### 위와같이 (4번같이) 구현할 경우에 어떤 문제점이 생길 수 있을까?

- 가시성(visibility)이 보장되지 않음
  - 변수의 변경이 즉시 다른 스레드에 반영되지 않는다면, 다른 스레드는 첫 번째 null 체크에서 false를 반환하여 인스턴스를 중복 생성할 수도 있음
  - 가시성 보장 X = 한 스레드에서 변수의 변경이 다른 스레드에 즉시 반영되지 않을 수 있다는 것을 의미
    ![](https://nesoy.github.io/assets/posts/20180609/1.png)
    ![](https://nesoy.github.io/assets/posts/20180609/2.png)

### (4) Double checked locking + volatile

#### <u>volatile(발러틸)</u>

- Java 변수를 Main Memory에 저장하겠다라는 것을 명시 **(가시성 보장)**
- 매번 변수의 값을 Read할 때마다 CPU cache에 저장된 값이 아닌 Main Memory에서 가져옴
- 기본적으로, 각 스레드는 메인 메모리 로부터 값을 복사해 CPU 캐시 에 저장하여 작동 -> 각 스레드가 같은 변수에 대해 읽기, 쓰기 동작을 수행할 시 각자의 CPU 캐시 에 메인 메모리 의 값과 다른 값을 갖고 있을 수 있게 된다. => 이에 <u>**동시성 문제**</u>가 생김
- Main Memory에서 가져오게 하여 해결하고자함!

#### volatile과 synchronized를 같이 쓰자!

- ㄱ. volatile 키워드를 사용하여 모든 스레드가 항상 같은 공유 변수의 값을 읽어올 수 있도록 보장
- ㄴ. synchronized 키워드를 사용한 getInstance 메서드 (하나의 스레드만이 접근할 수 있는 메서드) 로 공유 변수에 인스턴스를 할당
- ㄷ. 메인 메모리 에 해당 인스턴스의 값이 갱신되고 이로써 다른 모든 스레드들은 null 이 아닌 공유 변수에 할당된 인스턴스를 메인 메모리로부터 바로 읽어 올 수 있게 됨

#### 그래서

- 초기화 할 때만 동기화 작업을 수행(기본 synchronized보다 나음) + 가시성 보장
- (단점) CPU Cache보다 Main Memory가 비용이 더 크다.
- volatile 키워드를 사용하기 위해 JVM 버전이 1.5 이상이어야함
  - JVM에 따라 Thread Safe하지 않을 수 있는 문제점

```java
public class Singleton {
    // volatile 키워드 사용
    private static volatile Singleton singletonObject;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (singletonObject == null) {
            // if 문 진입 시에만 Singleton 클래스에 대한 동기화 작업 수행
            synchronized (Singleton.class) {
                if (singletonObject == null) {
                    singletonObject = new Singleton();
                }
            }
        }

        return singletonObject;
    }
}
```

### (5) Bill Pugh Solution (빌 푸 솔루션)

- private static inner class(내부 정적 클래스)를 사용
  - 내부 정적 클래스는 외부에서 접근할 수 없음
  - 내부 정적 클래스는 사용될 때 초기화됨
  - Singleton 클래스의 getInstance() 메소드가 호출될 때만 로드 (필요없는데 처음부터 초기화되는 부분을 방지 => Lazy Initialization 보장 )
- Double checked locking보다 단순하면서 JVM 버전에 제약 없이 지연 초기화(Lazy Initialization)와 Thread Safe 보장

```java
public class Singleton {

    private Singleton() {
    }

    // 내부 정적 클래스
    private static class SingletonHolder {
        // 정적 변수 (Singleton객체)가 여기있음
        // final -> 한번만 초기화됨
        private static final Singleton SINGLETON_OBJECT = new Singleton();
    }
    // 외부에서 접근 가능
    public static Singleton getInstance() {
        return SingletonHolder.SINGLETON_OBJECT;
    }
}
```

### (6) Enum (권장)

- Enum 클래스는 생성자를 private으로 갖게 만들고, 상수만 갖는 클래스
- 클래스 로딩 시점에 내부적으로 스레드 안전성을 보장
- Enum은 클래스를 만드는 동시에 인스턴스가 생성되지만, 인스턴스의 초기화는 해당 Enum 상수가 처음으로 참조될 때 이루어짐 ! (필요없는데 처음부터 초기화되는 부분을 방지 => Lazy Initialization 보장 )

```java
public enum Singleton {
    SINGLETON_OBJECT
}
```

```java
// 사용할때
  Singleton singleton = Singleton.SINGLETON_OBJECT;
```

## 2. 특징(장-단점)

### 장점

- 유일한 인스턴스 : 싱글톤 패턴이 적용된 클래스의 인스턴스는 애플리케이션 전역에서 단 하나만 존재하도록 보장한다. 이는 객체의 일관된 상태를 유지하고 전역에서 접근 가능하도록 한다.

- 메모리 절약 : 인스턴스가 단 하나뿐이므로 메모리를 절약할 수 있다. 생성자를 여러 번 호출하더라도 새로운 인스턴스를 생성하지 않아 메모리 점유 및 해제에 대한 오버헤드를 줄인다.

### 단점

- 결합도 증가 : 싱글톤 패턴은 전역에서 접근을 허용하기 때문에 해당 인스턴스에 의존하는 경우 결합도가 증가할 수 있다.

- 상태 관리의 어려움 : 만약, 싱글톤 클래스가 상태를 가지고 있는 경우 전역에서 사용되어 변경될 수 있다. 이로 인해 예상치 못한 동작이 발생할 수 있음

- 전역에서 접근 가능 : 애플리케이션 내 어디서든 접근이 가능한 경우, 무분별한 사용을 막기 힘들다. 이로 인해 변경에 대한 복잡성이 증가할 수 있다.

-  테스트의 어려움 :  단위 테스트는 테스트가 서로 독립적이어야 하며 테스트를 어떤 순서로든 실행할 수 있어야 한다. 하지만 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 '독립적인' 인스턴스를 만들기 어렵다.

-  
### 정리

> :baby: : 싱글톤 디자인 패턴은 클래스의 인스턴스를 오직 하나만 생성하고, 어플리케이션 전역에서 접근 가능하게 하는 디자인 패턴입니다. 전역적인 접근과 메모리 절약을 제공한다는 장점이 있지만, 결합도를 증가 시킨다는 점에서 단점이 존재하고, 멀티 스레스 문제를 유의해야합니다.

#### 참고 블로그

---

[참고 블로그(싱글톤)](https://velog.io/@naneun/Java-Volatile-%ED%82%A4%EC%9B%8C%EB%93%9C%EB%9E%80)

[참고 블로그 (volatile)](https://nesoy.github.io/articles/2018-06/Java-volatile)

[참고 블로그 (내부 클래스)](https://velog.io/@skyepodium/%ED%81%B4%EB%9E%98%EC%8A%A4%EB%8A%94-%EC%96%B8%EC%A0%9C-%EB%A1%9C%EB%94%A9%EB%90%98%EA%B3%A0-%EC%B4%88%EA%B8%B0%ED%99%94%EB%90%98%EB%8A%94%EA%B0%80)

[님들 제 노션도 보세요(도움안됨)](https://cedar-bath-611.notion.site/e5dd1ec5fc8b4edf82dd57d16e28d004)
