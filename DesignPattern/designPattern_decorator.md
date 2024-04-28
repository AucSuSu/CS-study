## Decorator Pattern

객체에 동적으로 기능을 추가하여 확장할 수 있는 구조 패턴

상속을 통해 클래스를 확장(x)

객체를 감싸는 방식을 사용하여 기능을 추가 및 변경(o)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcocran%2FbtsGZAMVSJI%2F0SvTKLf8Aejgx5R3vs5hXK%2Fimg.png)

<br/>

> ### Decorator Pattern 특징

#### **장점**

- **단일 책임 원칙(SRP) 준수** : 하나의 클래스는 하나의 기능을 담당
- **개방 폐쇄 원칙(OCP) 준수** : 기존의 코드를 변경하지 않으면서, 기능을 추가 가능
- **의존성 역전 원칙(DIP) 준수** : Class를 직접 참조하지 않고 상위요소(인터페이스)를 참조

#### **단점**

- **복잡성 증가** : 객체를 여러층으로 감싸고 클래스의 수가 많아지면 복잡성이 증가
- **순서 의존성** : 데코레이터를 적용하는 순서에 따라 객체의 동작이나 상태가 달라질 수 있다.
- **불필요한 복사** : 여러 레벨의 데코레이터를 사용할 경우 불필요한 객체 복사

<br/>

> ### Decorator Pattern 예제

```java
// Coffee.java

public interface Coffee {
	String getCoffee();
	int  Price
}
```

```java
// Americano.java

public class Americano implements Coffee {

    @Override
    public String getCoffee() {
        return "아메리카노";
    }

    @Override
    public int price() {
        return 2000;
    }
}
```

<br/>

#### 데코레이터 패턴을 사용하지 않는 경우

```java
// AmericanoAddShot.java

public class AmericanoAddShot implements Coffee {
    private Coffee americano;

    public AmericanoAddShot(Coffee americano) {
        this.americano = americano;
    }

    @Override
    public String getCoffee() {
        return americano.getCoffee() + " 샷추가";
    }

    @Override
    public int getPrice() {
        return americano.getPrice() + 500;
    }
}
```

```java
// AmericanoAddSugar.java

public class AmericanoAddSugar implements Coffee {
    private Coffee americano;

    public AmericanoAddSugar(Coffee americano) {
        this.americano = americano;
    }

    @Override
    public String getCoffee() {
        return americano.getCoffee() + " 설탕추가";
    }

    @Override
    public int getPrice() {
        return americano.getPrice() + 500;
    }
}
```

```java
public class DecoratorMain {
    public static void main(String[] args) {
        Coffee americano = new Americano();

        // 아메리카노 샷 추가
        Coffee americanoAddShot = new AmericanoAddShot(americano);
        System.out.println(americanoAddShot.getCoffee());
        System.out.println(americanoAddShot.getPrice());

        // 아메리카노 설탕 추가
        Coffee americanoAddSugar = new AmericanoAddSugar(americano);
        System.out.println(americanoAddSugar.getCoffee());
        System.out.println(americanoAddSugar.getPrice());
    }
}

// 출력
아메리카노 샷추가
2500
아메리카노 설탕추가
2500
```

<br/>

#### 데코레이터 패턴을 사용하는 경우

```java
// CoffeeDecorator.java

public abstract class CoffeeDecorator implements Coffee {
    private Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public String getCoffee() {
        return coffee.getCoffee();
    }

    @Override
    public int getPrice() {
        return coffee.getPrice();
    }
}
```

```java
// ShotDecorator.java

public class ShotDecorator extends CoffeeDecorator {
    public ShotDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getCoffee() {
        return super.getCoffee() + " 샷추가";
    }

    @Override
    public int getPrice() {
        return super.getPrice() + 500;
    }
}
```

```java
// SugarDecorator.java

public class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public String getCoffee() {
        return super.getCoffee() + " 설탕추가";
    }

    @Override
    public int getPrice() {
        return super.getPrice() + 500;
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Coffee americano = new Americano();
        Coffee americanoShotAdd = new ShotDecorator(americano);
        Coffee americanoShotSugarAdd = new SugarDecorator(americanoShotAdd);

        // 아메리카노 샷 추가
        System.out.println(americanoShotAdd.getCoffee());
        System.out.println(americanoShotAdd.getPrice());

        // 아메리카노 샷, 설탕 추가
        System.out.println(americanoShotSugarAdd.getCoffee());
        System.out.println(americanoShotSugarAdd.getPrice());
    }
}

// 출력
아메리카노 샷추가
2500
아메리카노 샷추가 설탕추가
3000
```

<br/>

> ### Decorator Pattern 언제 쓰나요?

- 객체 책임과 행동이 동적으로 상황에 따라 다양한 기능이 빈번하게 추가/삭제되는 경우
- 객체의 결합을 통해 기능이 생성될 수 있는 경우
- 객체를 사용하는 코드를 손상시키지 않고 런타임에 객체에 추가 동작을 할당할 수 있어야 하는 경우
- 상속을 통해 서브클래싱으로 객체의 동작을 확장하는 것이 어색하거나 불가능 할 때

<br/>

<br/>

## 참고 자료

https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0Decorator-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90

https://ittrue.tistory.com/558

https://www.youtube.com/watch?v=VL9OAohbjzI&t=2s

https://www.geeksforgeeks.org/decorator-design-pattern-in-java-with-example/?ref=gcse
