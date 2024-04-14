## Factory Pattern

비슷한 객체를 반복적으로 생성해야 하는 경우 사용하는 패턴

객체를 생산하는 공장(Factory)을 구현하는 방법이

개발자가 컴파일 단계에서 어떤 객체를 생성해야될지 모르고,

런타임 환경에서 동적으로 객체를 생성해야 할 때도 사용

<br/>

#### 자동차 객체를 반복해서 생성해야된다고 가정

```js
const car1 = {
    name: "아반떼",
    price: "1,570 ~ 2,453만원",
    getInfo: function(){
    	return this.name+"의 가격은 "+this.price+" 입니다.";
    }
}
const car2 = {
    name: "쏘나타",
    price: "2,386 ~ 3,367만원",
    getInfo: function(){
    	return this.name+"의 가격은 "+this.price+" 입니다.";
    }
}
```

**위 코드의 단점** : car 객체의 수정사항이 생기면 모든 car 객체를 찾아 수정해 주어야 한다.

=> 이 단점을 없애기 위해 car 객체를 만들어 주는 factory 함수를 생성

<br/>

```js
const factory = function(param){
    return {
        name: param.name,
        price: param.price,
        getInfo: function(){
            return this.name+"의 가격은 "+this.price+" 입니다.";
        }
    }
}
const car1 = factory({name: "아반떼", price: "1,570 ~ 2,453만원"});
const car2 = factory({name: "쏘나타", price: "2,386 ~ 3,367만원"});
console.log(car1, car2);
```

factory 라는 함수에서 파라미터로 받은 정보로 새로운 Object를 리턴

이제 car 객체의 수정이 발생하게되면 factory 함수만 수정하면 해결

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnO4Qk%2FbtsGDKuyKHa%2F7xa5IZWYK3XRcFhNK7VBU1%2Fimg.png)

**위 코드의 단점** : 각 객체가 생성될 때마다 **`getInfo`** 메서드가 새로 생성되어 각 객체마다 중복

메모리를 비효율적으로 사용하고, 같은 기능을 하는 함수가 여러개 존재하게 됨

class 키워드를 활용한 **`factory pattern`**으로 작성

<br/>

```js
class Car{
    constructor(info){
        this.name = info.name;
        this.price = info.price;
    }
    getInfo(){
        return this.name+"의 가격은 "+this.price+" 입니다.";
    }
    static factory(name){
        switch (name) {
            case "Avante":
                return new Avante();
            case "Sonata":
                return new Sonata();
            default:
                throw new Error("지원하지 않는 차종입니다");
        }
	}
}
class Sonata extends Car{
    constructor(){
        super({name: "쏘나타", price: "2,386 ~ 3,367만원"});
    }
}
class Avante extends Car{
    constructor(){
        super({name: "아반테", price: "1,570 ~ 2,453만원"});
    }
}
const avante = Car.factory("Avante");
const sonata = Car.factory("Sonata");
console.log(avante, sonata);
```

**Car 클래스** : 기본 차량 클래스로, 생성자에서 차량의 이름(`name`)과 가격(`price`)을 초기화

 또한 `getInfo` 인스턴스 메서드를 포함하여 차량의 정보를 문자열 형태로 반환.

**factory 메서드** : `Car` 클래스의 정적 메서드로, 주어진 차종 이름에 따라 해당하는 차종의 객체를 생성하여 반환 이 메서드를 통해 `Avante`와 `Sonata` 클래스의 인스턴스를 생성

**Sonata와 Avante 클래스** : `Car`를 상속받아 각 차종에 특화된 정보를 초기화하는 클래스

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fo02xV%2FbtsGEtsz7lf%2F0uuW0cPLxZ2L4D43agqxt1%2Fimg.png)

각 Avante, Sonata 객체별로 정보를 가지고 있고 상속받은 부모객체인 Car의 메소드를 공유

개발자는 어떤 차종의 Class를 사용할지 빌드 단계에서 결정하지 않아도 되며

런타임 환경에서 원하는 차종의 Class를 동적으로 생성할 수 있음 (Factory의 파라메터로 전달)

<br/>

> ### factory pattern 요약


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FAX4pF%2FbtsGARvDsvB%2F0XxCkjj6K1teSM7BOrK651%2Fimg.png)

1. factory pattern은 생성자 카테고리 패턴중의 기본이 되며, class 객체의 생성을 factory interface, factory class 등을 통해 위임하여 처리
2. factory class는 조건 로직이 필요 그에 따라 어떤 객체를 생성할지 생성
3. class 간의 의존이 낮아 확장이 쉽고, 추후 유지 및 보수가 편리하다.





