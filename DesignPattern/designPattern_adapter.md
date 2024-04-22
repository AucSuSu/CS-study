# 어탭터 패턴(Adapter pattern)
호환성이 없는 인터페이스 때문에 함께 동작할 수 없는 클래스들을 함께 작동해주도록 변환해주는 패턴

 - 서로 다른 인터페이스를 가지는 두 객체를 연결하여 사용할 수 있도록 하는 구조적인 패턴이다.
 - 기존에 작성된 코드를 재사용할 수 있으며, 객체 간의 결합도를 낮출 수 있다.
 - 어댑터 패턴 적용 시 추가적인 객체 생성이 필요하므로 메모리 사용량이 증가할 수 있으며, 인터페이스를 변환하기 위한 코드가 추가된다.

## 어탭터 패턴 예시

1. 원래 객체와 호환되지 않는 외부 라이브러리나 API를 사용해야 하는 경우
    -  이런 경우, 어댑터 패턴을 이용하여 기존 코드를 재사용하면서 외부 라이브러리나 API를 사용할 수 있다.

2. MVC 디자인 패턴.
    -  MVC 디자인 패턴에서 모델과 뷰 사이에 컨트롤러를 두어 모델과 뷰를 연결한다.
    -  이때, 어댑터 패턴을 이용하여 모델과 뷰의 인터페이스를 변환하면, 컨트롤러에서 모델과 뷰를 쉽게 연결할 수 있다.


## 어댑터 패턴의 종류
어댑터 패턴은 크게 객체 어댑터 패턴과 클래스 어댑터 패턴으로 구분된다.


1. 객체 어댑터 패턴(Object Adapter Pattern)

 - 객체 어댑터 패턴은 객체를 이용하여 인터페이스를 연결하는 방식이다. 
 - 어댑터 클래스는 두 개의 인터페이스를 구현하며, 하나의 인터페이스를 다른 인터페이스로 변환하는 역할을 한다.
 -  객체 어댑터 패턴을 이용하면, 기존 클래스의 코드 변경 없이도 인터페이스를 변환하여 사용할 수 있다.

```
interface Basic {
  print();
}

interface Color {
  printColor();
}

class BasicPrinter implements Basic {
  print() {
    console.log('기본 프린터 출력');
  }
}

class ColorPrinter implements Color {
  printColor() {
    console.log('컬러 프린터 출력');
  }
}

class Adapter implements Basic {
  private colorPrinter: Color;

  constructor(colorPrinter: Color) {
    this.colorPrinter = colorPrinter;
  }

  print() {
    this.colorPrinter.printColor();
  }
}

const basicPrinter = new BasicPrinter();
const colorPrinter = new ColorPrinter();

// 객체 어댑터를 사용하여 기본 프린터를 컬러 프린터처럼 사용
const adapter = new Adapter(colorPrinter);

const printers: Basic[] = [
  basicPrinter,
  adapter,
];

for (const printer of printers) {
  printer.print();
}

```
 
2. 클래스 어댑터 패턴(Class Adapter Pattern)

- 클래스 어댑터 패턴은 상속을 이용하여 인터페이스를 연결하는 방식이다.
- 어댑터 클래스는 기존 클래스를 상속받아 새로운 클래스를 만들며, 상속을 통해 인터페이스를 연결한다.
- 클래스 어댑터 패턴을 이용하면, 새로운 클래스를 만들어 사용해야 하므로, 객체 어댑터 패턴보다는 코드의 복잡도가 높아진다.

```
interface Basic {
  print();
}

interface Color {
  printColor();
}

// 기본 프린터는 print()만 가능
class BasicPrinter implements Basic {
  print() {
    console.log('기본 프린터 출력');
  }
}

// 컬러 프린터는 printColor()만 가능
class ColorPrinter implements Color {
  printColor() {
    console.log('컬러 프린터 출력');
  }
}

// 어탭터를 사용해 colorPrint도 print()를 가능하게 함
class Adapter implements Basic {
  constructor(private colorPrinter: Color) { }

  print() {
    this.colorPrinter.printColor();
  }
}

<!--  -->

const printers: Basic[] = [
  new BasicPrinter(),
  new Adapter(new ColorPrinter()),
]

for (const printer of printers) {
  printer.print();
}
```

## 어댑터 패턴 사용 이유

 - 변경할 수 없는 내부 구현, 라이브러리 등에 추가적인 기능을 만들고 싶을 때 유용하게 활용할 수 있다.
 - 특히 기존에 사용하던 라이브러리의 동작을 바꿔야 하는 상황에서, 라이브러리의 구현을 바꾸는 것은 꽤나 위험한 선택이다. 어떤 곳에서 사이드 이펙트가 발생할 지 모르기 때문이다.
 - 따라서, 어댑터 패턴을 활용하여 기존 코드를 건드리지 않고 새로운 동작을 구현하면 훨씬 안정적이고 재활용성이 우수하다. 또한 폭넓은 확장 가능성을 고려해볼 수 있다.

 ## 참고

 [https://blog.naver.com/mycho/221850541665]

 ![adapter](https://github.com/AucSuSu/CS-study/assets/139415941/5bde177e-4e34-4492-b083-e933a8ce50f0)

````
function Interface() {

  this.implements = function(obj) {

    // this는 인터페이스, obj는 인터페이스를 implements한 클래스

    var notImplementMethod = [];

    for(var method in this) {

      if(method !== 'implements') {

        // obj.__proto__ 객체가 method를 가지고 있지 않으면

        if(!Object.hasOwnProperty.call(obj.__proto__, method)) {

          notImplementMethod.push(method);

        }

      }

    }

    if(notImplementMethod.length > 0) {

      throw new Error(obj.__proto__.constructor.name + " 클래스의 "

      + notImplementMethod.join() + " 메서드가 구현되지 않았습니다.");

    }

  }

}

​

// 타입이 1이고 사이즈가 5인 스피커는 호환이 되는 

// SamsungTV 클래스 정의

function SamsungTV() {

  this.speakerType = 1;

  this.speakerSize = 5;

}

SamsungTV.prototype.isCompatable = function(speaker) {

  var ret = false;

  try {

    if(this.speakerType === speaker.getType()

      && this.speakerSize === speaker.getSize()) {

        ret = true;

    }

  } catch(e) {

    ret = false;

  }

  return ret;

};

​

// SamsungSpeaker 클래스 정의

function SamsungSpeaker(size) {

  this.type = 1;

  this.size = size;

}

SamsungSpeaker.prototype.getType = function() {

  return this.type;

}

SamsungSpeaker.prototype.getSize = function() {

  return this.size;

}

​

// LgSpeaker 클래스 정의

// 타입에 대한 정의가 없음.

function LgSpeaker(size) {

  this.size = size;

}

LgSpeaker.prototype.getSize = function() {

  return this.size;

}

​

// SamsungSpeakerIF 인터페이스 선언

function SamsungSpeakerIF(){

  if (this.constructor === SamsungSpeakerIF){ 

    throw new Error(this.constructor.name 

      + ' 인터페이스는 객체를 생성할 수 없습니다.');

  }

  return (function(){

    // 인터페이스 정의 메서드

    var method = {

      getType : function(){},

      getSize : function(){},

    };

    // 인터페이스의 implements 메서드를 method객체에 이식한다.

    Interface.call(method);

    return method;

  })();

};

​

// SamsungSpeakerIF 인터페이스를 구현하는

// LgSpeakerAdapter 클래스 정의

function LgSpeakerAdapter(lgSpeaker) {

  SamsungSpeakerIF().implements(this);

  this.speaker = lgSpeaker;

}

LgSpeakerAdapter.prototype.getType = function() {

  // SamsungSpeaker는 타입이 1이기 때문에

  return 1;

}

LgSpeakerAdapter.prototype.getSize = function() {

  return this.speaker.getSize();

}

​

function Client() {}

Client.prototype.test = function() {

  var samsungTV = new SamsungTV();

  var samsungSpeaker1 = new SamsungSpeaker(5);

  var samsungSpeaker2 = new SamsungSpeaker(10);

  var lgSpeaker1 = new LgSpeaker(5);

  console.log(samsungTV.isCompatable(samsungSpeaker1)); // true

  console.log(samsungTV.isCompatable(samsungSpeaker2)); // false

  console.log(samsungTV.isCompatable(lgSpeaker1)); // false

​

  var adaptedLgSpeaker1 = new LgSpeakerAdapter(lgSpeaker1);

  console.log(samsungTV.isCompatable(adaptedLgSpeaker1)); // true

  var lgSpeaker2 = new LgSpeaker(10);

  var adaptedLgSpeaker2 = new LgSpeakerAdapter(lgSpeaker2);

  console.log(samsungTV.isCompatable(adaptedLgSpeaker2)); // false

}


new Client().test();
```
