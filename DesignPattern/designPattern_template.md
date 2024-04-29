# 템플릿 메서드 패턴 (Template Method Pattern)

> 디자인 패턴에서의 템플릿은 변하지 않는 것을 의미한다.

 - 어떤 모양이나 형식과 같은 틀만 정해져있고 어떻게가 비어있는 것을 템플릿이라고 한다.

 - 알고리즘의 기본적인 구조를 상위 클래스에 정의하고,

 - 부분적으로 필요한 구체적인 구현을 하위 클래스에서 처리하도록 디자인 된 패턴이다.

<br>

## 구현

Template Method를 구성하기 위해선 두 가지 객체가 필요하다.

1. 추상 클래스(Abstract Class)
 - Template Method를 구현하는 객체이며, 해당 템플릿 메소드에서 사용하는 추상 메소드를 선언한다.

2. 구현 클래스(Concrete Class)
 - 추상 클래스에서 정의한 추상 메소드를 구현하여, 템플릿 메서드의 실제 동작을 결정한다.

주어진 문자열을 n번 반복하여 출력하는 프로그램을 만든다고 가정하자.

<br>

### 추상 클래스

- open, print, close 3가지 추상 메소드를 가지고 이 메소드들의 호출 로직만을 정의하는 display를 구현하는 추상 클래스 AbstractDisplay를 만들 수 있다.

```
abstract class AbstractDisplay {
  abstract open(): void;
  abstract print(): void;
  abstract close(): void;

  display(): void {
    this.open();
    for (let i = 0; i < 5; i += 1) {
      this.print();
    }
    this.close();
  }
}
```

- display라는 코드가 구현되어있지만, 이 내용만 봐서는 이 메소드가 무엇을 출력하기 위함인지, 혹은 정말 이름 그대로 출력하는 메소드가 맞는지 아무것도 알 수가 없다.

따라서, 실제 동작을 구현하는 하위 클래스가 필요하다.

<br>

### 구현 클래스(1) - CharDisplay

- 단순히 하나의 문자를 받아서 처리하는 클래스를 만들 수 있다.

```
class CharDisplay extends AbstractDisplay {
  private line: string[] = [];
  constructor(
    private char: string
  ) {
    super();
    this.char = char;
  }

  open() {
    this.line.push('<<');
  }

  print() {
    this.line.push(this.char);
  }

  close() {
    this.line.push('>>');
    console.log(this.line.join(''));
  }
}
```

- CharDisplay는 처음 인스턴스화를 할 때 받은 문자 char를 바탕으로 이를 출력하거나 혹은 다른 문자를 출력하는 코드를 AbstractDisplay의 세 메소드 open, print, close에 구현을 했다.

```
const displayChar = new CharDisplay('H');
displayChar.display();

<<HHHHH>>
```

<br>

### 구현 클래스(2) - StringDisplay

- 문자 하나 대신 문자열을 넣고 문자열을 상자 형태로 출력하도록 구현할 수 있다.

```
class StringDisplay extends AbstractDisplay {
  constructor(
    private string: string,
    private width: number
  ) {
    super();
    this.string = string;
    this.width = width;
  }

  open() {
    this.printLine();
  }

  print() {
    console.log(`|${this.string}|`);
  }

  close() {
    this.printLine();
  }

  private printLine() {
    const line = ['+'];
    for (let i = 0; i < this.width; i += 1) {
      line.push('-');
    }
    line.push('+');
    console.log(line.join(''));
  }
}
```

```
const displayString = new StringDisplay('Hello, World', 10);
displayString.display();

+----------+
|Hello, World|
|Hello, World|
|Hello, World|
|Hello, World|
|Hello, World|
+----------+

```

<br>

## 템플릿 메서드 패턴의 특징

1. 패턴 사용 시기

 - 클라이언트가 알고리즘의 특정 단계만 확장하고, 전체 알고리즘이나 해당 구조는 확장하지 않도록 할 때

 - 동일한 기능은 상위 클래스에서 정의하면서 확장, 변화가 필요한 부분만 하위 클래스에서 구현할 때


2. 장점

 - 클라이언트가 대규모 알고리즘의 특정 부분만 재정의하도록 하여, 알고리즘의 다른 부분에 발생하는 변경 사항의 영향을 덜 받도록 한다.
 - 상위 추상클래스로 로직을 공통화 하여 코드의 중복을 줄일 수 있다.
 - 재사용성과 유연성 증가, 유지보수의 이점이 있다.

3. 단점

 - 알고리즘의 제공된 골격에 의해 유연성이 제한될 수 있다.
 - 알고리즘 구조가 복잡할수록 템플릿 로직 형태를 유지하기 어려워진다.
 - 추상 메소드가 많아지면서 클래스의 생성, 관리가 어려워질 수 있다.
 - 상위 클래스에서 선언된 추상 메소드를 하위 클래스에서 구현할 때, 그 메소드가 어느 타이밍에서 호출되는지 클래스 로직을 이해해야 할 필요가 있다.
 - 로직에 변화가 생겨 상위 클래스를 수정할 때, 모든 서브 클래스의 수정이 필요 할수도 있다.

<br>

## 결론
 - 개별 하위 클래스 구현의 유연성을 얻는 대신, 상위 클래스 알고리즘의 구조 변경에는 상대적으로 취약해진다.
 - 코드의 유연성과 구조적 복잡성 사이의 균형을 어떻게 맞출 것인지, 추상화의 수준을 어디까지 가져갈 것인지를 항상 고민해야 한다.

<br>

## 참고

https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%ED%85%9C%ED%94%8C%EB%A6%BF-%EB%A9%94%EC%86%8C%EB%93%9CTemplate-Method-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90

https://velog.io/@ninthsun91/Typescript%EB%A1%9C-%EB%8B%A4%EC%8B%9C-%EC%93%B0%EB%8A%94-GoF-Template-Method
