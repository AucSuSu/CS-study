# Strategy Pattern 전략패턴

## 0. 개념

> 미리 여러가지 전략을 만들어두고, 상황에 맞게 택할 수 있도록 하는 알고리즘

-   행위 패턴
-   실행(=런타임) 중에 알고리즘 전략에 따라 객체의 동작을 실시간으로 바뀌도록함

![image](https://upload.wikimedia.org/wikipedia/commons/3/39/Strategy_Pattern_in_UML.png)

#### 전략 패턴은 OOP의 집합체이다

-   동일 계열의 알고리즘을 정하는 과정 => 추상화
-   각각의 알고리즘을 => 캡슐화
-   알고리즘을 상황에 따라 다양하게 변경할 수 있게 함 => 다형성

## 1. 구조

### Context 맥락

-   알고리즘을 실행해야할때 연결된 전략 객체의 메소드를 호출하는 역할

### 전략 (추상화)

-   전략 공통 인터페이스

### 전략 알고리즘 객체

-   알고리즘, 행위, 동작을 객체로 정의한 구현체

## 2. 구현

#### 전략

```java
// 전략(추상화된 알고리즘)
interface IStrategy {
    void doSomething();
}

// 전략 알고리즘 A
class ConcreteStrateyA implements IStrategy {
    public void doSomething() {}
}

// 전략 알고리즘 B
class ConcreteStrateyB implements IStrategy {
    public void doSomething() {}
}

```

#### Context 맥락

```java
class Context {
    IStrategy Strategy;

    // 전략 교체 메소드
    void setStrategy(IStrategy Strategy) {
        this.Strategy = Strategy;
    }

    // 전략 실행 메소드
    void doSomething() {
        this.Strategy.doSomething();
    }
}
```

#### 동작

```java
class Client {
    public static void main(String[] args) {
        // 1. 컨텍스트 생성
        Context c = new Context();

        // 2. 전략 설정
        c.setStrategy(new ConcreteStrateyA());

        // 3. 전략 실행
        c.doSomething();

        // 4. 다른 전략 설정 -> runtime
        c.setStrategy(new ConcreteStrateyB());

        // 5. 다른 전략 시행
        c.doSomething();
    }
}
```

## 3. 특징

### 언제 사용하면 좋을까?

-   전략이 여러버전 혹은 변형이 필요할 때
-   런타임에 실시간으로 교체되어야 할 때
-   전략이 노출되어서는 안되는 데이터에 엑세스 하거나 데이터를 활용할때
    -   전략 자체를 캡슐화 한다는 관점에서 필요

### 주의점

-   전략 구현체가 많아지면 관리해야할 객체수가 많아짐
-   복잡하므로 필요할때만 사용해야함
-   전략 간의 차이점이 명확해야함

## 4. 예시

-   계산기를 사용하려고 하는데 계산기는 총 2개이다.덧셈만 하는 계산기, 그리고 뺄셈만 하는 계산기
    ![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FUEDks%2FbtrpgzElygY%2FUnLgvbgB1NyHvBycTM394k%2Fimg.png)

### Strategy가 될 수 있는 것

-   두 개의 계산기 모두 두개의 수를 받아서 계산을 한다는 공통특성이 존재함

```java
public interface Calculator {
    double execute(double n1, double n2);
}
```

### Strategy 구현채가 될 수 있는 것

-   각각의 계산기는 구현체임

```java
public class PlusCalculator implements Calculator{
    @Override
    public double execute(double n1, double n2) {
        return n1+n2;
    }
}
```

```java
public class MinusCalculator implements Calculator{

    @Override
    public double execute(double n1, double n2) {
        return n1-n2;
    }
}
```

### Context가 될 수 있는 것

-   이때 사람이 계산기를 사용함.

```java
public class People {

    private Calculator calculator;
    private double n1;
    private double n2;

    public double operate(){
        return calculator.execute(n1,n2);
    }

    public void setCalculator(Calculator calculator){
        this.calculator=calculator;
    }

    void changeNumber(double n1, double n2) {
        this.n1 = n1;
        this.n2 = n2;
    }

}
```

```java
public class Main {
    public static void main(String[] args) {
        People people = new People();

        // 숫자 설정
        people.changeNumber(1,2);

        // Calculator 설정
        people.setCalculator(new PlusCalculator());
        double result1 = people.operate();
        System.out.println(result1);


        // 새로운 Calculator 설정 -> runtime에 전략변경
        people.setCalculator(new MinusCalculator());
        double result2 = people.operate();
        System.out.println(result2);
    }
}

```

## 5. JAVA 예시

> **정확하게 Strategy pattern이라기 보다는 변형에 가까움**

-   Collections의 sort() 메서드에 의해 구현되는 compare()
-   javas.servlet.Filter의 doFilter()

### compare()

-   자바에서 익명 클래스로 구현체를 그때그때 정의하고 안의 동작 메소드의 로직을 그때그때 만들어 할당하는 방식도 일종의 전략을 실시간으로 변경하여 지정하는 패턴

```java
Collections.sort(numbers, new Comparator<Integer>() {

            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2; // -> 전략 알고리즘 구현체
            }

});

```

### doFilter()

> 메서드는 필터가 요청 및 응답을 처리하는 로직
>
> -   여러개의 전략이 존재한다는 점에서 Strategy와 유사
> -   택하는게 아니라 조합한다는 점에서 차이가 있을 수 있음

-   Strategy

```java
public interface Filter {
    void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
        throws IOException, ServletException;
}
```

-   Strategy 구현체

```java
public class AuthenticationFilter implements Filter {
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
        throws IOException, ServletException {
        // 인증 로직
        chain.doFilter(request, response); // 다음 필터 호출
    }
}

public class LoggingFilter implements Filter {
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
        throws IOException, ServletException {
        // 로깅 로직
        chain.doFilter(request, response); // 다음 필터 호출
    }
}

public class CompressionFilter implements Filter {
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
        throws IOException, ServletException {
        // 압축 로직
        chain.doFilter(request, response); // 다음 필터 호출
    }
}

```

-   Main 에서 Context (FilterChain)

```java
public class Main {
    public static void main(String[] args) throws ServletException, IOException {

        // Context
        SimpleFilterChain filterChain = new SimpleFilterChain();

        // 전략을 조합함 -> 2개의 전략을 택함
        filterChain.addFilter(new AuthenticationFilter());
        filterChain.addFilter(new LoggingFilter());

        ServletRequest request = new MockServletRequest();
        ServletResponse response = new MockServletResponse();

        filterChain.doFilter(request, response);
    }
}
```
