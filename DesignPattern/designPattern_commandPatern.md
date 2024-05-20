## Command Pattern

- 명령을 캡슐화 해서 처리

- 구성요소 : invoker, command, receiver, client



![](https://upload.wikimedia.org/wikipedia/commons/8/8e/Command_Design_Pattern_Class_Diagram.png)

**Client** : 명령들을 생성 하고, 수신 담당 Receiver를 설정해주는 역할

**Receiver** : 명령이 주어졌을 때 , 그것을 수행하고 반영하는 역할

**Invoker** : 명령이 저장되어있고, 실행 업무를 전달 받을 시 명령 객체를 호출 하고 도와주는 역할

**Command** : Invoker에게 받은 명령을 전달하는 역할 

<br/>

### 예제 코드

#### 1. 'Command 인터페이스'

```java
public interface Command {
    void execute();
}
```

<br/>

#### 2. 'Light 클래스' (Receiver)

```java
public class Light {
    public Light() { }

    public void turnOn() {
        System.out.println("The light is on");
    }

    public void turnOff() {
        System.out.println("The light is off");
    }
}

```

실제 작업을 수행하는 객체

두가지 메소드, `turnOn()`과 `turnOff()`를 가지고 있음.

<br/>

#### 3. 'TurnOnLightCommand 클래스'

```java
public class TurnOnLightCommand implements Command {
    private Light theLight;

    public TurnOnLightCommand(Light light) {
        this.theLight = light;
    }

    public void execute() {
        theLight.turnOn();
    }
}
```

Command 인터페이스를 구현

`execute`메소드는 `theLight.turnOn()`을 호출하여 조명을 킴

<br/>

#### 4. 'TurnOffLightCommand 클래스'

```java
public class TurnOffLightCommand implements Command {
    private Light theLight;

    public TurnOffLightCommand(Light light) {
        this.theLight = light;
    }

    public void execute() {
        theLight.turnOff();
    }
}

```

Command 인터페이스를 구현

`execute`메소드는 `theLight.turnOff()`을 호출하여 조명을 끔

<br/>

#### 5. 'Switch 클래스' (Invoker)

```java
public class Switch {
    private Command flipUpCommand;
    private Command flipDownCommand;

    public Switch(Command flipUpCmd, Command flipDownCmd) {
        this.flipUpCommand = flipUpCmd;
        this.flipDownCommand = flipDownCmd;
    }

    public void flipUp() {
        flipUpCommand.execute();
    }

    public void flipDown() {
        flipDownCommand.execute();
    }
}
```

Switch 클래스는 Invoker 역할 

Command 객체를 통해 요청을 수행

`flipUp()`과 `flipDown()` 메소드를 통해 `execute()`를 호출

<br/>

#### 6. 'TestCommand 클래스' (Client)

```java
public class TestCommand {
    public static void main(String[] args) {
        Light light = new Light();
        Command switchUp = new TurnOnLightCommand(light);
        Command switchDown = new TurnOffLightCommand(light);

        Switch s = new Switch(switchUp, switchDown);

        s.flipUp();
        s.flipDown();
    }
}
```

<br/>

## 장/단점

#### 장점

- 새로운 명령을 쉽게 추가할 수 있음
- 의존성을 최소화 하는 장점

#### 단점

- 명령에 대한 각각의 클래스는 시스템이 커지거나 요구되는 로직이 많아질 시 늘어날 수 있음
- 코드가 다소 복잡한 측면이 있음

<br/>

## 참고자료

https://www.youtube.com/watch?v=r601hDellMs&t=145s

https://www.youtube.com/watch?v=bUULgkwaicQ

https://ko.wikipedia.org/wiki/%EC%BB%A4%EB%A7%A8%EB%93%9C_%ED%8C%A8%ED%84%B4
