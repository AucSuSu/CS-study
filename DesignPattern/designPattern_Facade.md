# 퍼사드 패턴(Facade Pattern)
![image](https://github.com/AucSuSu/CS-study/assets/109134365/9b9ee5da-0ed0-4d10-9a6c-cab142aec34e)
퍼사드의 직역은 건물의 정면 이다.

건물 내부에는 많은 구조물들이 있는데, 실제 외부에서 보이는 것은 외부 모습 뿐이다.

건물의 내부의 복잡한 클래스들(영화관, 수영장, 카페)등을 한번에 관리하는 건물 외부(인터페이스)를 만드는 것!

>퍼사드 패턴은 **구조 패턴**의 한 종류로써, 복잡한 서브 클래스들의 공통적인 기능을 정의하는 상위 수준의 인터페이스를 제공하는 패턴.
- Facade : 사용자의 요청을 서브시스템 객체에 전달하는 단순하고 일관된 통합 인터페이스
- Subsystem Classes : Facade에 대한 정보를 가지지 않고, 서브시스템의 기능을 구현하는 클래스

## 구조패턴??
구조 패턴이란 작은 클래스들의 **상속**과 **합성**을 통해 더 큰 클래스를 생성하는 방법을 제공하는 패턴으로, 독립적으로 개발한 클래스들을 마치 하나인 것처럼 사용할 수 있다.

# 퍼사드 패턴의 예시
## 상황
아침에 일어나서 출근하기까지 여러 과정들이 있다.
씻기, 아침먹기, 대중교통 타기

이러한 과정을 Facade Pattern을 이용해서 각각의 동작들을 서브클래스로 구현하고, 서브클래스들의 공통 기능을 정의하는 **'상위 수준의 인터페이스'**
를 정의할 수 있다.
![image](https://github.com/AucSuSu/CS-study/assets/109134365/2dd9df6e-0882-4d59-ac9f-d8c88a7ba522)

퍼사드와 서브시스템을 구현해보자.

**Facade**

씻기 클래스, 아침 클래스, 무브 클래스가 GoOffice라는 퍼사드 클래스에 들어있다.
```java
public class GoOffice {

    public void goToWork() {
        Wash wash = new Wash();
        Breakfast breakfast = new Breakfast();
        Move move = new Move();

        wash.brushTeeth();
        wash.shower();
        breakfast.eat();
        breakfast.water();
        move.bus();
    }
}
```

**Subsystem Classes**

Wash, Breakfast, Move 등으로 3가지 서브시스템 클래스를 만들었다.
```java
//Wash.class
public class Wash {

    public void brushTeeth() {
        System.out.println("Brush my teeth");
    }

    public void shower() {
        System.out.println("Take a shower");
    }
}

//Breakfast.class
public class Breakfast {

    public void eat() {
        System.out.println("Have breakfast");
    }

    public void water() {
        System.out.println("Drink water");
    }
}



//Move.class
public class Move {

    public void bus() {
        System.out.println("Take the bus");
    }
}
```

```java
public class Client {

    public static void main(String[] args) {
        GoOffice goOffice = new GoOffice();
        goOffice.goToWork();
    }
}
```

위의 코드를 실행하면, 아래처럼 나온다.

![image](https://github.com/AucSuSu/CS-study/assets/109134365/d4aa0e62-be5a-4453-86dc-6471650e6333)

## 장점
- 낮은 결합도 -> 클라이언트가 서브시스템의 코드를 몰라도 Facade 클래스를 통해 사용 가능
- 가독성 상승 -> 기존에는 Client에서 여러 서브 클래스들을 직접 호출해야 했지만 하나의 객체만을 호출하여 사용할 수 있고 간단명료하다.
- 서브 클래스 직접 접근이 가능 -> Facade 클래스를 통해 서브클래스를 사용할지, 서브클래스를 직접 사용할지 선택가능
## 단점
- 퍼사드가 너무 많은 책임을 가지게 되면, 퍼사드 자체가 복잡해진다.
- 시스템의 세부 기능을 제고앟지 못하므로, 복잡한 작업을 수행하려는 클라이언트에는 제한적이다.

## 참고
https://velog.io/@dabeen-jung/cs-Java-Facade-%ED%8C%A8%ED%84%B4-%ED%8D%BC%EC%82%AC%EB%93%9C-%ED%8C%A8%ED%84%B4
https://velog.io/@bagt/Design-Pattern-Facade-Pattern-%ED%8D%BC%EC%82%AC%EB%93%9C-%ED%8C%A8%ED%84%B4
https://leeheefull.tistory.com/13
