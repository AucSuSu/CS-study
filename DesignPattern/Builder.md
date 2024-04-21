# Builder Pattern
빌터 패턴은 복잡한 객체의 생성 과정과 표현 방법을 분리하여 다양한 구성의 인스턴스를 만드는 **생성 패턴**이다.
생성자에 들어갈 **매개 변수를 메서드로** 하나하나 받아들이고 마지막에 통합 빌드해서 객체를 생성하는 방식.

유연하게 받아서 다양한 타입의 인스턴스를 생성할 수 있다.

>그럼 빌더 패턴은 왜 사용할까?? 그것을 학습하기 전에 기존의 패턴을 알아보자.

## 점층적 생성자 패턴
점층적 생성자 패턴은 필수 매개변수와 함께 선택 매개변수 0개,1개,2개,....를 받는 형태로 생성자를 **오버로딩** 하는 방식

### 자바코드 ㅈㅅ; 어쩔수없음 내가 스프링밖에 모름..
```java
class Hamburger {
    // 필수 매개변수
    private int bun;
    private int patty;

    // 선택 매개변수
    private int cheese;
    private int lettuce;
    private int tomato;
    private int bacon;

    public Hamburger(int bun, int patty, int cheese, int lettuce, int tomato, int bacon) {
        this.bun = bun;
        this.patty = patty;
        this.cheese = cheese;
        this.lettuce = lettuce;
        this.tomato = tomato;
        this.bacon = bacon;
    }

    public Hamburger(int bun, int patty, int cheese, int lettuce, int tomato) {
        this.bun = bun;
        this.patty = patty;
        this.cheese = cheese;
        this.lettuce = lettuce;
        this.tomato = tomato;
    }
    

    public Hamburger(int bun, int patty, int cheese, int lettuce) {
        this.bun = bun;
        this.patty = patty;
        this.cheese = cheese;
        this.lettuce = lettuce;
    }

    public Hamburger(int bun, int patty, int cheese) {
        this.bun = bun;
        this.patty = patty;
        this.cheese = cheese;
    }

    ...
}
```
```java
public static void main(String[] args) {
    // 모든 재료가 있는 햄버거
    Hamburger hamburger1 = new Hamburger(2, 1, 2, 4, 6, 8);

    // 빵과 패티 치즈만 있는 햄버거
    Hamburger hamburger2 = new Hamburger(2, 1, 1);

    // 빵과 패티 베이컨만 있는 햄버거
    Hamburger hamburger3 = new Hamburger(2, 0, 0, 0, 0, 6);
}
```

하지만 이러한 방식은 클래스의 필드가 많아지면 생성자의 인자수가 늘어나서 헷갈릴 수 있다.
또한, 이러면 **생성자의 몇번째 인수가 뭐인지** 파악해야한다.
위의 경우에는 3번째 인자가 토마토인지 패티인지 알야아함.

또한, 매개변수 특성상 빵과 베이컨만 있는 햄버거를 원할경우 억지로 파라미터에 0을 전달해야한다.
타입이 다양할 수록 생성자 메서드 수가 매우매우매우 늘어나서 **가독성**이랑 **유지보수** 측면에서 좋지않다.

## 자바 빈 패턴
이러한 단점을 보완하기 위해 Setter 메소드를 사용한 자바 빈(Bean) 패턴이 고안되었다.
매개변수 없이 Setter 메소드로 필드의 초깃값을 설정한다.
```java
class Hamburger {
    // 필수 매개변수
    private int bun;
    private int patty;

    // 선택 매개변수
    private int cheese;
    private int lettuce;
    private int tomato;
    private int bacon;
    
    public Hamburger() {}

    public void setBun(int bun) {
        this.bun = bun;
    }

    public void setPatty(int patty) {
        this.patty = patty;
    }

    public void setCheese(int cheese) {
        this.cheese = cheese;
    }

    public void setLettuce(int lettuce) {
        this.lettuce = lettuce;
    }

    public void setTomato(int tomato) {
        this.tomato = tomato;
    }

    public void setBacon(int bacon) {
        this.bacon = bacon;
    }
}
```

```java
public static void main(String[] args) {
    // 모든 재료가 있는 햄버거
    Hamburger hamburger1 = new Hamburger();
    hamburger1.setBun(2);
    hamburger1.setPatty(1);
    hamburger1.setCheese(2);
    hamburger1.setLettuce(4);
    hamburger1.setTomato(6);
    hamburger1.setBacon(8);

    // 빵과 패티 치즈만 있는 햄버거
    Hamburger hamburger2 = new Hamburger();
    hamburger2.setBun(2);
    hamburger2.setPatty(1);
    hamburger2.setCheese(2);

    // 빵과 패티 베이컨만 있는 햄버거
    Hamburger hamburger3 = new Hamburger();
    hamburger3.setBun(2);
    hamburger2.setPatty(1);
    hamburger3.setBacon(8);
}
```
자바 빈 패턴은 가독성 문제점이 사라지고 Setter 메서드로 유연적으로 객체 생성이 가능하다.
하지만, 두 문제점이 있다.

1. 일관성 문제
**필수 매개변수(객체 초기화시 반드시 설정되어야 하는 값)**을 개발자가 Setter 메서드를 호출하지 않아서 설정하지 못할 수 있다.
EX) 햄버거의 번은 필수 필드인데 setter 안할수도있음. 그럼 이게 햄버거? 아니지..

2. 불변성 문제
자바 빈 패턴의 Setter 메서드는 생성할때 뿐아니라 언제든지 누군가가 외부에서 사용가능 하므로 불변성이 보장되지 않는다. (변할 수 있음)

### 그래서 빌더 패턴이 두두등장~!
![image](https://github.com/AucSuSu/CS-study/assets/109134365/9b17b501-6ba2-4d39-bf4f-ef6ea9511764)

## 빌더 패턴
별도의 Builder 클래스를 만들어 메서드를 통해 step-by-step으로 값을 입력받은 후에 최종적으로 build() 메서드로 하나의 인스턴스를 생성후 리턴한다.
```java
public static void main(String[] args) {

    // 생성자 방식
    Hamburger hamburger = new Hamburger(2, 3, 0, 3, 0, 0);

    // 빌더 방식
    Hamburger hamburger = new Hamburger.Builder(10)
        .bun(2)
        .patty(3)
        .lettuce(3)
        .build();
}
```

### 빌더 클래스 구현하기
아래의 클래스가 있다고 가정하자.
```java
class Student {
    private int id;
    private String name = "아무개";
    private String grade = "freshman";
    private String phoneNumber = "010-0000-0000";

    public Student(int id, String name, String grade, String phoneNumber) {
        this.id = id;
        this.name = name;
        this.grade = grade;
        this.phoneNumber = phoneNumber;
    }
    
    @Override
    public String toString() {
        return "Student { " +
                "id='" + id + '\'' +
                ", name=" + name +
                ", grade=" + grade +
                ", phoneNumber=" + phoneNumber +
                " }";
    }
}
```

Builder 클래스를 만들고 필드 멤버를 구성한다.
```java
class StudentBuilder {
    private int id;
    private String name;
    private String grade;
    private String phoneNumber;
    
}
```

```java
class StudentBuilder {
    private int id;
    private String name;
    private String grade;
    private String phoneNumber;

    public StudentBuilder id(int id) {
        this.id = id;
        return this;
    }

    public StudentBuilder name(String name) {
        this.name = name;
        return this;
    }

    public StudentBuilder grade(String grade) {
        this.grade = grade;
        return this;
    }

    public StudentBuilder phoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
        return this;
    }
}
```

여기서 보면 각 메서드의 반환에 **this**가 있다는 점이 특징이다.

>this가 뭔가요...?

this는 클래스의 객체 자신을 말한다. 그래서 **자신을 리턴함으로써 메소드 다음에 또 다른 메서드가 호출이 가능** -> **체이닝(Chaining)**
EX) new StudentBuilder().id(값).name(값)

```java
class StudentBuilder {
    private int id;
    private String name;
    private String grade;
    private String phoneNumber;

    public StudentBuilder id(int id) { ... }

    public StudentBuilder name(String name) { ... }

    public StudentBuilder grade(String grade) { ... }

    public StudentBuilder phoneNumber(String phoneNumber) { ... }

    public Student build() {
        return new Student(id, name, grade, phoneNumber); // Student 생성자 호출
    }
}
```

마지막으로, 빌더의 목표였던 StudentBuilder가 아닌, Student 객체를 만들어주는 build 메서드를 구성해준다.

저렇게하면 이렇게 가능!
```java
public static void main(String[] args) {

    Student student = new StudentBuilder()
                .id(2016120091)
                .name("임꺽정")
                .grade("Senior")
                .phoneNumber("010-5555-5555")
                .build();
}
```


## 빌더 패턴의 장점
### 1. 객체 생성 과정을 일관되게 표현
### 2. 디폴트 매개변수 생략을 간접적으로 지원( 원래 자바에선 디폴트 매개변수를 지원안함 )
![image](https://github.com/AucSuSu/CS-study/assets/109134365/fe97c34f-7197-471c-abbc-21645b35c1a8)

### 3. 필수 멤버와 선택적 멤버를 분리 가능.
```java
class StudentBuilder {
    // 초기화 필수 멤버
    private int id;

    // 초기화 선택적 멤버
    private String name;
    private String grade;
    private String phoneNumber;

    // 필수 멤버는 빌더의 생성자를 통해 설정
    public StudentBuilder(int id) {
        this.id = id;
    }

    // 나머지 선택 멤버는 메서드로 설정
    public StudentBuilder name(String name) {
        this.name = name;
        return this;
    }

    public StudentBuilder grade(String grade) {
        this.grade = grade;
        return this;
    }

    public StudentBuilder phoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
        return this;
    }

    public Student build() {
        return new Student(id, name, grade, phoneNumber);
    }
}
```
![image](https://github.com/AucSuSu/CS-study/assets/109134365/fbcdbcda-9262-4618-8b84-b47cfbad7106)

### 4. 객체 생성 단계를 지연할 수 있음

객체 생성을 단계별로 구성하거나 구성 단계를 지연하거나 재귀적으로 생성을 처리할 수 있다.
즉, 빌더를 재사용 함으로써 객체 생성을 주도적으로 지연할 수 있다.
```java
// 1. 빌더 클래스 전용 리스트 생성
List<StudentBuilder> builders = new ArrayList<>();

// 2. 객체를 최종 생성 하지말고 초깃값만 세팅한 빌더만 생성
builders.add(
    new StudentBuilder(2016120091)
    .name("홍길동")
);

builders.add(
    new StudentBuilder(2016120092)
    .name("임꺽정")
    .grade("senior")
);

builders.add(
    new StudentBuilder(2016120093)
    .name("박혁거세")
    .grade("sophomore")
    .phoneNumber("010-5555-5555")
);

// 3. 나중에 빌더 리스트를 순회하여 최종 객체 생성을 주도
for(StudentBuilder b : builders) {
    Student student = b.build();
    System.out.println(student);
}
```
![image](https://github.com/AucSuSu/CS-study/assets/109134365/7d75ee7c-0890-44ea-a8fa-0f8d142c84f9)

### 5. 초기화 검증을 멤버별로 분리( 내가 이해하기론 유효성 검증 같음 )

아래를 보면 생성자에서 매개변수에 대한 유효성을 검사를 한번에 하고있다.
```java
class Student {
	...

	// 각 매개변수에 대한 검증을 하나의 생성자 모두 처리하고 있다
    public Student(int id, String name, String grade, String phoneNumber) {
        if (!grade.equals("freshman") && !grade.equals("sophomore") && !grade.equals("junior") && !grade.equals("senior")) {
            throw new IllegalArgumentException(grade);
        }

        if (!phoneNumber.startsWith("010")) {
            throw new IllegalArgumentException(phoneNumber);
        }

        this.id = id;
        this.name = name;
        this.grade = grade;
        this.phoneNumber = phoneNumber;
    }
}
```
이러한 것을 멤버변수(필드) 별로 분리해서 작성할 수 있다.
```java
class StudentBuilder {
	...

    public StudentBuilder(int id) {
        this.id = id;
    }

    public StudentBuilder name(String name) {
        this.name = name;
        return this;
    }

    public StudentBuilder grade(String grade) {
        if (!grade.equals("freshman") && !grade.equals("sophomore") && !grade.equals("junior") && !grade.equals("senior")) {
            throw new IllegalArgumentException(grade);
        }
        this.grade = grade;
        return this;
    }

    public StudentBuilder phoneNumber(String phoneNumber) {
        if (!phoneNumber.startsWith("010")) {
            throw new IllegalArgumentException(phoneNumber);
        }
        this.phoneNumber = phoneNumber;
        return this;
    }

    public Student build() {
        return new Student(id, name, grade, phoneNumber);
    }
}
```

### 6. 멤버에 대한 변경 가능성 최소화를 추구
**Setter를 통해서 초기화**하는건 너무너무 안좋은 방법이다.
**불변 객체**는 오로지 읽기(get) 메서드만을 제공하며 쓰기(set)는 제공하지 않는다. 대표적으로 final 키워드 붙인거.

자바 개발자들이 매우매우매우매우매우 좋아하는 불변 객체를 사용하는 이유는
1. Thread-Safe 하므로 동기화(동시성)을 고려하지 않아도 됨.
2. 가변 객체를 통해 작업하다가 예외(Exception)가 발생하면 해당 객체가 불안정한 상태가 될 수 있으므로 또 다른 에러 유발 가능.
3. 불변 객체로 구성하면 다른 사람이 개발한 함수를 위험없이 이용을 보장할 수 있어 협업,유지보수에 유용하다.

따라서 클래스들은 가변적이어야 하는 매우 타당한 이유가 없으면 반드시 불변으로 만들어야한다.
만약 불가능하다면 가능한 변경 가능성을 최소화 해야한다.

예를 들어 final 키워드를 붙일수 없는 상황에서 setter 메서드 자체를 구현하지 않음으로 간접적으로 불변 객체가 된다. 왜? setter 가 없스니까

즉, 빌더 패턴은 생성자 없이 어느 객체에 대해 변경 가능성을 최소화를 추구하여 불변성을 갖게 해준다. 최고인듯.


## 빌더 패턴의 단점
1. 코드 복잡성 증가 -> N개의 클래스에 대해 N개의 새로운 빌더 클래스를 만들어야한다.
2. 생성자보다는 성능이 떨어진다. -> 매번 메서드를 호출하여 빌더를 거쳐 인스턴스화 하기 때문. (어플리케이션의 성능을 극으로 중요시되는 상황이면 문제 발생할 수 있음)
3. 클래스의 필드가 4개보다 적고, 필드의 변경 가능성이 없는 경우라면 차라리 생성자나 정적 팩토리 메서드를 쓰는게 나을 수 있다.


## 출처
출처: https://inpa.tistory.com/entry/GOF-💠-빌더Builder-패턴-끝판왕-정리#빌더_패턴_탄생_배경 [Inpa Dev 👨‍💻:티스토리]
