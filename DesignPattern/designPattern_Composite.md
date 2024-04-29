# Composite 패턴
> 클라이언트가 개별 객체와 복합 객체를 **동일**하게 다룰 수 있도록 함.<br>
> 객체들을 트리 구조로 구성, 개별 객체와 복합 객체를 일관된 방식으로 처리 가능하게 함.<br>
> 전체-부분 관계를 갖는 객체 사이의 관계를 정의할 대 유용

## Composite 패턴의 구성 요소
**1. Component:**
- 클라이언트가 composition 내의 오브젝트를 다루기 위해 제공되는 인터페이스를 뜻함
    - 클라이언트는 Component 인터페이스를 통해 **모든 객체를 동일**하게 처리 가능
- 인터페이스 혹은 추상 클래스로 정의되며 모든 오브젝트들에게 공통되는 메소드를 정의해야 함
    - 개별 객체와 복합 객체에 대한 공통 인터페이스 정의<br>
    
**2. Leaf:**
- composition 내 오브젝트의 행동을 정의
  - 복합체를 구성하는데 중요한 요소이며, *베이스 컴포넌트를 구현(*재사용 가능한 기본 구성 요소)
- Leaf는 더 이상의 자식을 가지지 않음
  - 따라서, Leaf는 다른 컴포넌트에 대해 참조를 가져선 안됨
      - 가장 기본적인 요소, 단일 객체
      - 참조를 하게 되면 단순성과 일관성이 깨짐.(참조를 여러번 할 수도 있으니깐!)<br>
      
**3. Composite:**
- Leaf 객체로 구성되어 있으며, 컴포넌트 내 명령을 구현
    - 자식 Component를 관리할 수 있는 메소드를 제공 <br>

![image](https://github.com/AucSuSu/CS-study/assets/64372881/864e1c59-de3f-4a32-9296-c8121fd6b960)

## Composite 패턴의 목적 및 사용
- Composite 패턴은 클라이언트가 복잡한 트리 구조를 가진 객체를 단일 객체처럼 쉽게 다룰 수 있도록 함
- 객체의 추가, 제거 작업을 투명하게 처리 가능
- 파일 시스템에서 폴더와 파일을 동일한 방식으로 취급 가능.<br>(폴더는 Composite, 파일은 Leaf)<br>

## Composite 패턴 구현
예시를 들어 설명해보겠다.

그림판에 삼각형, 사각형, 동그라미를 만들고 삼각형과 사각형을 그룹화했다고 가정한다.<br>
이때, 만들어진 도형들을 모두 빨간색으로 색칠하려고 한다.<br>

우리가 채우기 버튼을 누를 때 선택하는 것이 어떤 도형인지, 혹은 그룹인지에 대해 구분하지 않아도 된다. <br>
그림판에는 도형 하나에 대한 채우기 기능과 그룹에 대한 채우기 기능이 같다.<br>

> Composite 패턴은 전체 도형을 하나의 도형으로 관리할 수 있다는 특징을 갖고 있다.<br>

### 1. Component
Component에서는 Leaf와 Composite의 공통이 되는 메소드를 정의한다.<br>
Shape 인터페이스 내 각 도형에 색을 입히는 draw()메소드를 정의해보자.

```java
// Shape.java
public interface Shape {
    public void draw(String paintColor);
}
```

### 2. Leaf
Leaf 객체는 복합체에 포함되는 요소로, Component를 구현한다.

```java
// Triangle.java
public class Triangle implements Shape {
    public void draw(String paintColor) {
    	System.out.println("삼각형이 다음 색상으로 색칠되었습니다. : " + paintColor);
    }
}
// Square.java
public class Square implements Shape {
    public void draw(String paintColor) {
    	System.out.println("사각형이 다음 색상으로 색칠되었습니다. : " + paintColor);
    }
}
// Circle.java
public class Circle implements Shape {
    public void draw(String paintColor) {
    	System.out.println("동그라미가 다음 색상으로 색칠되었습니다. : " + paintColor);
    }
}
```

### 3. Composite
Composite 객체는 Leaf 객체를 포함하고 있고, Component를 구현할 뿐만 아니라<br>
Leaf 그룹에 대해 **add와 remove를 할 수 있는 메소드**를 클라이언트에게 제공한다.

```java
// Drawing.java
public class Drawing implements Shape {
    private List<Shape> shapes = new ArrayList<>();
    
    @Override
    public void draw(String paintColor) {
    	for(Shape shape : shapes) {
        	shape.draw(paintColor);
        }
    }
    
    public void add(Shape s) {
    	this.shapes.add(s);
    ]
    
    public void remove(Shape s) {
    	this.shapes.remove(s);
    }
    
    public void clear() {
    	System.out.println("모든 도형을 제거합니다.");
        this.shapes.clear();
    }
}
```

> 위 코드에서 중요한 것은, Composite의 객체 또한 Component를 구현해야 한다는 것이다.<br>
> 그래야 클라이언트가 Composite 객체에 대해 다른 Leaf와 동일하게 취급할 수 있기 때문이다.

```java
// CompositePattern.java
public class CompositePattern {
    public static void main(String args[]) {
        Shape triangle = new Triangle();
        Shape circle = new Circle();
        Shape square = new Square();
        
        Drawing drawing = new Drawing();
        drawing.add(triangle);
        drawing.add(circle);
        drawing.add(square);
        
        drawing.draw("빨간색");
        
        List<Shape> shapes = new ArrayList<>();
        shapes.add(drawing);
        shapes.add(new Triangle());
        shapes.add(new Circle());
        
        for(Shape shape : shapes) {
        	shape.draw("초록색");
        }
    }
}
```

![image](https://github.com/AucSuSu/CS-study/assets/64372881/9999a3f5-e09e-483f-9874-a82162975f8e)

> drawing 객체를 통해 Triangle, Circle, Sqaure등 Leaf 객체를 그룹으로 묶어서 한번에 동작할 수 있다.<br>
> 그룹 뿐만 아니라 다른 도형들도 마찬가지로, Shape 인터페이스를 구현하고 있기 때문에 함께 취급되는 것을 확인할 수 있다.

## 장점
- **투명성**: 클라이언트는 개별 객체와 복합 객체를 구분하지 않고 동일한 인터페이스를 통해 다룸
- **단순성**: 트리 구조를 이용하여 복잡한 파트와 전체를 동일하게 관리

## 단점
- **디자인 복잡성**: 특정 구성요소에 특화된 기능을 구현할 때 제한적일 수 있음. <br>일부 작업은 Leaf 객체에만 의미가 있거나, 반대로 Composite 객체에만 필요할 수 있다.

## 사용
> 개발자는 복잡한 객체 계층을 보다 쉽게 관리하고, 확장할 수 있다.
- GUI 구성요소
- 그래픽 편집기
- 파일 시스템
