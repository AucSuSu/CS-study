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

### 실행 결과
```
Shape : Circle<br>
Inside Circle::draw() method.<br>
Shape : Rectangle<br>
Inside Rectangle::draw() method.<br>
```
