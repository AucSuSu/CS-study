# Abstract Factory
> 24년도 1회 정처기 실기 20번 문제로 나옴... 저는 틀렸습니다 하하 😂

## Abstract Factory란?
- 구체적인 클래스에 의존하지 않고 **서로 연관되거나 의존적인 객체들의 조합**을 만드는 인터페이스를 제공
- 동일한 주제의 다른 팩토리를 묶음
- 디자인 패턴 중, '생성 패턴'에 해당

### Factory Method과의 차이점
- Factory Method: **어떤 객체를 생성** 할 지에 집중
- Abstract Factory: 연관된 **객체를 그룹화** (연관된 객체들의 생성을 **하나의 팩토리**에서 담당)
> 참고: [Factory Method 설명](https://bcp0109.tistory.com/367)

## Example
### Background
- 우리는 나이키 신발 공장이다!
- 스타일과 신발 이름을 입력 받아서 제작, 준비, 포장해서 반환해야 한다.
- 세계 각국에 제공해야 하며, 신발 종류도 나라마다 다 다른 총체적 난국 상황!

### Problem(구현 클래스)
![image](https://github.com/AucSuSu/CS-study/assets/64372881/7ba4cb14-a7ff-4add-85a7-54f2e94001ae)

- 구두를 만드는 **스토어 객체**: 고수준 컴포넌트
- 구두 **객체**: 저수준 컴포넌트
  > 고수준 컴포넌트(스토어)가 저수준 컴포넌트(구두)를 사용할 수 있다.

- 이때, 고수준 컴포넌트가 저수준 컴포넌트에 엄청난 의존을 하게 된다.
- 의존: 새 구두가 추가되면 스토어 객체까지 손 봐야 함!! (의존 관계 반전 필요)
- 객체지향 설계 5대 원칙(SOLID) 중, DIP을 따르는 설계가 필요.
  > DIP(Dependency-Inversion Principle): 구상 클래스에 의존하지 X, 추상화 된 것에 의존하도록 설계 해야함.

### Solution(추상 클래스)
![image](https://github.com/AucSuSu/CS-study/assets/64372881/da84a2b1-3802-4b6e-b18e-163371967683)
- 의존 관계 역전 원칙 적용

**지역 별 소규모 신발 재료 공장을 나누어, 신발을 생성**
``` java
interface ShoesIngredientFactory {
    public Bottom makeBottom();
    public Leather makeLeather();
    public boolean hasPattern();
}
```
- 공통 기능을 제공할 신발 재료 공장 인터페이스 생성
- JPShoesIngredientFactory.class (일본 공장)
  ``` java
    class JPShoesIngredientFactory implements ShoesIngredientFactory {
        @Override
        public Bottom makeBottom() return new RubberBottom();
        @Override
        public Leather makeLeather() return new LeatherOfCows();
        @Override
        public boolean hasPattern() return false;
    }
  ```
- FRShoesIngredientFactory.class (프랑스 공장)
  ``` java
    class FRShoesIngredientFactory implements ShoesIngredientFactory {
        @Override
        public Bottom makeBottom() return new PlasticAndRubberBottom();
        @Override
        public Leather makeLeather() return new LeatherOfSheeps();
        @Override
        public boolean hasPattern() return true;
    }
  ```

각 공장에서 메소드들이 return 해주는 **신발 재료들이 구현해야 하는 인터페이스**는 다음과 같다.
  ``` java
    interface Bottom {
      public String getName();
    }
  ```
  ``` java
    interface Leather {
      public String getName();
    }
  ```

**재료 인터페이스를 구현한 클래스**
- RubberBottom.class (고무 밑창)
  ``` java
    class RubberBottom implements Bottom {
        @Override
        public String getName() return "고무";
    }
  ```
- PlasticAndRubberBottom.class (에어맥스 밑창)
  ``` java
    class RubberBottom implements Bottom {
        @Override
        public String getName() return "플라스틱, 고무";
    }
  ```

**공장에서 만드는 신발 클래스**
- Shoes.class
  ``` java
    abstract class Shoes {
        String name;
        Bottom bottom;
        Leather leather;
        boolean hasPattern;
     
        abstract void assembling(); // 신발을 조립하는 추상 메소드
     
        void prepare() {
            System.out.println("완성된 신발을 준비 중입니다.");
        }
     
        void packing() {
            System.out.println("준비된 신발을 포장 중입니다.");
        }
     
        public String getName(){
            return name;
        }
     
        public void setName(String name) {
            this.name = name;
        }
    }
  ```
- 여기서 중요한 Point ➡ 원재료를 조립하는 **assembling** 추상 메소드!
  ``` java
    class BlackShoes extends Shoes {
        ShoesIngredientFactory shoesIngredientFactory;
     
        public BlackShoes(factory_abstract_factory.ShoesIngredientFactory shoesIngredientFactory) {
            this.shoesIngredientFactory = shoesIngredientFactory;
        }
     
        @Override
        void assembling() { 
            System.out.println("신발을 제작중입니다. " + name);
            leather = shoesIngredientFactory.makeLeather();
            bottom = shoesIngredientFactory.makeBottom();
            System.out.println("신발 정보 : 밑창은 " + bottom.getName() + " 사용 하였으며, 가죽은 " + leather.getName() + " 사용하였습니다.");
        }
     
    }
  ```
- 위 클래스는 Shoes 추상 클래스를 구현한 검은 신발의 클래스다.
- 국가 별로 다 다른 타입의 신발을 만들어주지 않아도 된다.
  - ShoesIgredientFactory 인스턴스를 생성자로 받아서 이 인스턴스로 부터 원재료를 받기 때문
- 오버라이딩하여 구현한 assembling 메소드를 사용
  - 가죽과 밑창을 각 공장 인스턴스에서 받아 조립
- Shoes 클래스는 공장에서 주는 재료로 신발을 **조립**하기 때문에, **어떤 지역의 팩토리를 사용해도 Shoes 클래스를 재활용 가능!**

### 사용(Store)
- 고객에게 주문을 받기 위한 Store 클래스
- ShoesStore.class
  ``` java
    abstract class ShoesStore {
        public Shoes orderShoes(String name) {
            Shoes shoes;
     
            shoes = makeShoes(name);
            shoes.assembling();
            shoes.prepare();
            shoes.packing();
     
            return shoes;
        }
     
        abstract Shoes makeShoes(String name);
    }
  ```
- 각 나라의 스토어는 이 Store 추상 클래스를 상속 받아, 추상 메소드인 makeShoes 메소드를 각 나라에 맞게 오버라이드 하면 됨
- orderShoes는 전 세계 공통 프레임워크이며, orderShoes 안에 있는 makeShoes 단계만 각 나라의 특징에 맞게 바뀌는 것!

- JPShoesStore.class (일본 Store)
  ``` java
    class JPShoesStore extends ShoesStore {
        @Override
        Shoes makeShoes(String name) { 
            Shoes shoes = null;
            ShoesIngredientFactory shoesIngredientFactory = new JPShoesIngredientFactory();
            
            if(name.equals("blackShoes")) {
                shoes = new BlackShoes(shoesIngredientFactory);
                shoes.setName("일본 스타일의 검은 신발");
            }
            else if(name.equals("brownShoes")) {
                shoes = new BrownShoes(shoesIngredientFactory);
                shoes.setName("일본 스타일의 갈색 신발");
            }
            else if (name.equals("redShoes")) {
                shoes = new RedShoes(shoesIngredientFactory);
                shoes.setName("일본 스타일의 빨간 신발");
            }
     
            return shoes;
        }
    }
  ```

- FRShoesStore.class (프랑스 Store)
  ``` java
    class FRShoesStore extends ShoesStore {
        @Override
        Shoes makeShoes(String name) { 
            Shoes shoes = null;
            ShoesIngredientFactory shoesIngredientFactory = new FRShoesIngredientFactory();
            
            if(name.equals("blackShoes")) {
                shoes = new BlackShoes(shoesIngredientFactory);
                shoes.setName("프랑스 스타일의 검은 신발");
            }
            else if(name.equals("brownShoes")) {
                shoes = new BrownShoes(shoesIngredientFactory);
                shoes.setName("프랑스 스타일의 갈색 신발");
            }
            else if(name.equals("redShoes")) {
                shoes = new RedShoes(shoesIngredientFactory);
                shoes.setName("프랑스 스타일의 빨간 신발");
            }
     
            return shoes;
        }
    }
  ```

**전 세계 공통 orderShoes 프레임워크**
- 각 매장은 makeShoes를 오버라이딩 하여, 현지 상황에 맞게 재정의 한다.
- makeShoes를 보면, 현지 공장 인스턴스를 생성한다.(ShoesIngredientFactory)
- 그 후, 매장에서 신발 재료 팩토리 인스턴스를 보내 원재료 공장으로부터 재료를 모두 받아온다.
- 매장에서 받은 재료를 조립하고, 준비, 포장하여 신발을 생성한다.

**실제 주문 과정**
- Main.class
  ``` java
    public class Main {
        public static void main(String[] args) {
            JPShoesStore jpStore = new JPShoesStore();
            jpStore.orderShoes("blackShoes");
     
            FRShoesStore frStore = new FRShoesStore();
            frStore.orderShoes("redShoes");
        }
    }
  ```

### 실행 결과
```
  > 신발을 제작중입니다. 일본 스타일의 검은 신발
  > 신발 정보 : 밑창은 고무 사용 하였으며, 가죽은 소가죽 사용하였습니다.
  > 완성된 신발을 준비 중입니다.
  > 준비된 신발을 포장 중입니다.
  > 신발을 제작중입니다. 프랑스 스타일의 검은 신발
  > 신발 정보 : 밑창은 플라스틱, 고무 사용 하였으며, 가죽은 양가죽 사용하였습니다.
  > 완성된 신발을 준비 중입니다.
  > 준비된 신발을 포장 중입니다.
```

- 과정 설명
  1. 일본 매장에서 검은 신발 주문, 매장에서 주문 받음(orderShoes)
  2. 주문 받은 직원이 일본 매장 담당 원재료 공장에 신발에 맞는 재료 요청(makingShoes)
  3. 원재료 공장 가동, 신발 재료 전송
  4. 매장에서 재료 조립, 신발 제작
  5. 준비, 포장하여 고객에게 제공
  6. (프랑스도 마찬가지!)
 
  ![image](https://github.com/AucSuSu/CS-study/assets/64372881/7274c47a-f048-45b6-af04-2327c16094ab)


### 기대 효과
- DIP 원칙 준수
- 객체 간의 결합도 낮아짐
- 유지 보수에 용이!


### 의존 관계 가이드라인(참고용)
- 가이드라인(참고하면 좋음 - 지키기 어려우니깐...)
  1. 어떤 변수라도 구상 클래스에 대한 레퍼런스 저장 금지
    - new 연산자 사용 시 구상 클래스를 저장하는 것 (팩토리 사용하기!)
  2. 구상 클래스에서 유도된 클래스 생성 금지
    - 이 경우, 특정 구상 클래스에 의존하게 됨
  3. 베이스 클래스에 이미 구현되어 있던 메소드 오버라이드 금지
    - 이 경우, 베이스 클래스가 추상화 되어 있는 것이 아님
    - 베이스 클래스에서 메소드 정의 시, 모든 서브 클래스에서 공유 가능한 것만 정의 해야 함

## Reference
- https://velog.io/@chrishan/abstract-factory-pattern
- https://bcp0109.tistory.com/368
- https://velog.io/@bae_mung/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%83%9D%EC%84%B1-Abstract-Factory-Pattern
