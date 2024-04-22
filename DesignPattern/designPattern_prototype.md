## 프로토타입(Prototype)
- Prototype Pattern은 객체 Cloning을 가능하게 하는 생성 패턴
- 객체를 생성하는데 비용이 많이 들고, 비슷한 객체가 이미 있는 경우에 사용되는 생성 패턴 중 하나.
-  원본 객체를 새로운 객체에 복사하여 필요에 따라 수정하는 메커니즘을 제공.
-  **clone**을 사용하여 복잡한 인스턴스 생성 로직을 필요로 하지 않음!!
- Prototype, ConcretePrototype, Client로 구성되어 있다.
![image](https://github.com/AucSuSu/CS-study/assets/64372881/9e09ae57-fa73-4dad-960f-75c7a8fe0d4f)

## Cloneable? clone?
- Java 언어에서는 인스턴스의 복사를 실행하는 도구로 clone 메소드 사용
- clone 메소드를 실행할 경우, 복사 대상이 되는 클래스는 java.lang.Cloneable 인터페이스 구현 필수!!
- 복사 대상이 되는 클래스가 직접 java.lang.Cloneable 인터페이스를 구현해도 상관 없고, 하위 클래스의 어딘가에서 Clonable 인터페이스를 구현해도 상관 없음. 

- Cloneable 인터페이스를 구현한 클래스의 인스턴스
  - clone 메소드를 호출하면 복사
  - 그 후, clone 메소드의 반환값은 복사해서 만들어진 인스턴스가 됨.
- clone 내부에선, 원래의 인스턴스와 같은 크기의 메모리를 확보한 뒤, 그 인스턴스의 필드 내용을 복사.
- 만약 Cloneable 인터페이스를 구현하지 않는 클래스가 clone 메소드를 호출하면 예외 **CloneNotSupportedException** 이 발생

### Cloneable이 요구하는 메소드는?
- 'Cloneable 인터페이스' 라고 하면 그 내부에 clone 메소드가 선언되어 있는 것처럼 생각하기 쉽지만...
- But!! Cloneable 인터페이스에는 메소드가 하나도 선언되어 있지 않음.
- 이 인터페이스는 단지 'clone 에 의해 복사할 수 있다' 라는 표시로서 사용되고 있음.
  - 이와 같은 표시를 하는 인터페이스를 marker interface라고 함.
 
### clone 메소드는 피상적인 복사를 실행함.
- clone 메소드는 필드의 내용을 그대로 복사하는 동작.
  - 즉, 필드 앞에 있는 인스턴스의 내용까지는 고려하지 않음! <br>
    (e.g. 필드 앞에 배열이 있었다고 할 경우, clone 메소드를 사용해서 복사를 한 경우)
  - 그 배열에 대한 참조만 복사될 뿐이고 배열의 요소 하나하나가 복사되는 것은 아님.
- 이와 같은 필드 대 필드의 복사(field-for-field copy)를 '피상적인 복사(shallow copy)'라고 함.
  - clone 메소드가 실행하는 것은 피상적인 복사.
  - 클래스 설계자가 clone 메소드를 오버라이드해서 자신이 필요한 '복사'를 정의 가능!<br>
    (clone 메소드를 오버라이드하는 경우 super.clone()을 사용한 상위 클래스의 clone 메소드의 호출을 잊지 말아야 함.)
- clone은 복사를 할 뿐이며 생성자를 호출하는 것이 아니라는 점을 주의!
- 또한 인스턴스 생성 시, 특수한 초기화를 필요로 하는 클래스에서는 clone 메소드 안에서 처리할 필요가 있음. 


## 구현 예제
- Java에서 Prototype 패턴을 구현하는 방법
  - `Cloneable` 인터페이스를 구현하고
  - `clone()` 메서드를 오버라이드하여 구현
 
![image](https://github.com/AucSuSu/CS-study/assets/64372881/34a3f842-fa69-421e-ab58-b93ce80fdcc8)

### 코드
``` java
// Shape: 추상 클래스
// `Cloneable` 인터페이스를 구현하고, `clone()` 메서드를 오버라이드하여 복제 가능한 객체로 만듦
// Shape 클래스를 상속받은 클래스들은 각각 다른 type을 가지고 있음.
public abstract class Shape implements Cloneable {
    private String id;
    protected String type;
    
    public String getType() {
        return type;
    }
    
    public String getId() {
        return id;
    }
    
    public void setId(String id) {
        this.id = id;
    }
    
    public abstract void draw();
    
    @Override
    public Object clone() {
        Object clone = null;
        
        try {
            clone = super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        
        return clone;
    }
}

public class Rectangle extends Shape {
    public Rectangle() {
        type = "Rectangle";
    }
    
    @Override
    public void draw() {
        System.out.println("Inside Rectangle::draw() method.");
    }
}

public class Circle extends Shape {
    public Circle() {
        type = "Circle";
    }
    
    @Override
    public void draw() {
        System.out.println("Inside Circle::draw() method.");
    }
}

// `shapeMap`이라는 `HashMap` 객체를 사용하여 `Shape` 객체를 캐싱
// `loadCache()` 메서드에서는 `Circle` 객체와 `Rectangle` 객체를 생성하여 `shapeMap`에 추가
// `getShape()` 메서드는 `shapeMap`에서 해당 `shapeId`에 해당하는 `Shape` 객체를 가져와 복제(clone)한 후 반환
public class ShapeCache {
    private static Map<String, Shape> shapeMap = new HashMap<>();
    
    public static Shape getShape(String shapeId) {
        Shape cachedShape = shapeMap.get(shapeId);
        return (Shape) cachedShape.clone();
    }
    
    public static void loadCache() {
        Circle circle = new Circle();
        circle.setId("1");
        shapeMap.put(circle.getId(), circle);
        
        Rectangle rectangle = new Rectangle();
        rectangle.setId("2");
        shapeMap.put(rectangle.getId(), rectangle);
    }
}

// `Main` 클래스에서는 `ShapeCache` 클래스의 `loadCache()` 메서드를 호출한 후,
// `ShapeCache` 클래스의 `getShape()` 메서드를 사용하여 `Shape` 객체를 가져와서 복제한 후,
// `getType()` 메서드를 호출하여 객체의 `type` 속성을 출력
// 이를 통해 `Shape` 객체가 복제(clone)되어 객체의 내용이 변경되지 않음을 확인 가능
public class Main {
    public static void main(String[] args) {
        ShapeCache.loadCache();

        Shape clonedShape = ShapeCache.getShape("1");
        System.out.println("Shape : " + clonedShape.getType());
        clonedShape.draw();

        Shape clonedShape2 = ShapeCache.getShape("2");
        System.out.println("Shape : " + clonedShape2.getType());
        clonedShape2.draw();
    }
}
```

### 실행 결과
```
Shape : Circle<br>
Inside Circle::draw() method.<br>
Shape : Rectangle<br>
Inside Rectangle::draw() method.<br>
```

## 프로토타입의 장점
### 1️⃣ 점진적 학습
> 클래스와 상속, 프로토타입과 위임은 “구체적인 상황에서 얻은 지식을 일반화하는 방식”의 두 가지 메커니즘이다.
> <br>하나의 예시를 통해 두 가지를 비교해보자.

- 나에게 ferrari 라는 자동차가 있다.
- 이는 새로운 자동차인 volvo 을 만났을 때 ferrari 에 대한 지식을 활용할 수 있다.
- 둘은 모두 자동차이기 때문에 공통점을 가지고 있을 것이고 이를 일반화 할 수 있다.

**클래스**
- 클래스에서는 Vehicle 이라는 클래스(집합)을 생성할 수 있다.
- 이러한 Vehicle 클래스는 나에게 있는 ferrari 와 충분히 유사한 모든 자동차에 대해 사실이라고 믿는 것을 추상화한 집합이다.
- ferrari 는 Vehicle 의 인스턴스(구성원)으로 볼 수 있다.
> 클래스(집합)는 모든 인스턴스(구성원)에 대해 사실을 나타내므로 이후에 자동차인 volvo 을 만나면 Vehicle 을 통해 volvo 를 설명할 수 있다.
> <br>그리고 각자가 다른 Vehicle 과 공유하지 않는 특성을 가지고 있다면 SportsCar , Truck 과 같은 서브클래스를 통해 구현할 수 있다.

**프로토타입**
> 프로토타입은 지식의 일반화.
- 프로토타입에서는 ferrari 를 자동차의 프로토타입(원형)을 나타내는 것으로 간주할 수 있다.
- 나에게 가장 친숙한 자동차가 ferrari 라면, 자동차는 ferrari 자체의 이미지일 수 있다.

![image](https://github.com/AucSuSu/CS-study/assets/64372881/741b782e-eaf9-4b23-9394-e646fafeb105)

- *“자동차의 바퀴가 몇 개인가?”* 라는 질문에 달리 생각해야할 타당한 이유가 없는 한, 질문에 대답하는 방식은 ferrari 의 바퀴가 몇 개인지와(4개 라고 가정) 답이 같다고 가정한다.
<br/>그리고 volvo 에 대해 설명하기 위해 프로토타입에서는 ferrari 를 volvo 의 프로토타입(원형)으로 지식을 활용할 수 있다.

- *“volvo 의 바퀴가 몇 개인가?”* 라는 질문에는 반대 증거가 없는 이상 ferrari 와 같다고 대답한다.
<br/>그러나, 이후 volvo 의 바퀴가 8개라는 사실을 알게 되면, 이는 volvo 에 대한 지식으로 저장되고, 프로토타입에 대한 참조를 확인하기 전에 검색된다.
- 또한 번거롭게 기존 수퍼클래스를 수정하거나, 서브클래스를 생성할 필요도 없다 .


**점진적 학습이란?**
> 위에서 살펴본 바에 의하면, 프로토타입 접근 방식은 어떤 면에서 사람들이 구체적인 상황에서 지식을 습득하는 것처럼 보이는 방식에 더 가깝다.

- 사람들은 일반적인 추상 원리를 먼저 흡수하고 나중에 특정 사례에 적용하는 것보다 구체적인 예를 먼저 다룬 다음 그로부터 일반화하는 것을 훨씬 더 잘하는 것 같다.
- 그러나, 클래스(집합)에서는 개별 인스턴스(구성원)을 생성하기 전에 먼저 집합에 대한 추상화를 해야한다. 수학에서 집합은 구성원을 열거하거나, 집합의 구성원을 식별하는 통합 원칙을 설명해 정의한다.
- 그러나 우리는 모든 자동차를 열거할 수 없다. 10억번째까지 자동차의 바퀴는 4개였지만, 10억1번째 자동차의 바퀴는 8개일 수도 있다.
- 하지만 클래스(집합)의 개념은 이와 위배된다. 이와 같이 프로토타입은 사람들이 더욱 편하게 생각하는 방식인 구체적인 예시들을 통해 개념에 대한 “점진적 학습”을 하는 데에 유리하다.

### 2️⃣ 메모리
> 프로토타입은 클래스 방식에 비해 “메모리”를 아낄 수 있다는 장점이 있다.
> <br>클래스는 새로운 인스턴스를 만들 때 “복사”를 하지만, 프로토타입은 객체와 객체를 “연결”한다.

- 하나의 클래스에서 100개 인스턴스를 만든다면, 클래스의 메서드를 100개 복사한다.
  <br/>그렇지만 프로토타입에서는 프로토타입 객체에 있는 메서드를 참조하고 있을 뿐임으로, 메서드 1개만 존재하게 된다.
- 그렇기 때문에 자바와 같은 클래스 기반 언어에서는 메모리를 아끼기 위해 하나의 클래스에서 하나의 인스턴스만 생성하는 싱글톤 패턴 등을 사용한다.


## 단점
### 1️⃣ 속도
> 프로토타입은 위임을 통해서 객체를 생성할 때 상위 객체의 메서드를 “연결” 하므로 객체의 크기를 줄여 메모리를 아낄 수 있다.
> <br>대신, 메서드를 검색하기 위해 상위 프로토타입 체인을 따라 검색해야한다.
- 이는 속도 저하로 이어질 수 있다는 문제가 있다.
- 다행히, 검색 시간을 줄이는 방식으로 조회 결과를 “캐싱”하는 방법이 있다! 크롬의 V8 엔진은 “히든 클래스”라는 개념을 통해 이러한 속도 문제를 해결했다.

## 결론
- 장점: 구체적 예시를 통한 개념의 점진적 학습, 메모리를 효율적으로 사용
- 단점: 속도가 느림
> 프로토타입이 무조건 클래스보다 좋다거나, 그 반대라거나 한 것은 아니다. 아직까지도 논의가 활발하다.

