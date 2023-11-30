# ⌨️ CommandPattern ⌨️

## 커맨드 패턴

실행될 기능을 캡슐화함으로써 주어진 여러 기능을 실행할 수 있는 재사용성이 높은 클래스를 설계하는 패턴이다.

### 커맨드 패턴의 주요 목적

요청과 요청을 처리하는 객체 사이의 결합을 줄이고, 요청의 내용을 캡슐화하여 요청을 동적으로 변경하거나 저장하고 실행을 보장하는 것. 

→ 유연성과 확장성이 향상

## 커맨드 패턴: 키오스크 사례로 쉽게 이해하기

개선 전

<img width="697" alt="Untitled" src="https://github.com/kkkwp/CS-study/assets/113974911/99a0f758-6759-4b11-9955-a4589d9b3761">


키오스크 도입 후


<img width="671" alt="Untitled 1" src="https://github.com/kkkwp/CS-study/assets/113974911/2c4d972c-5832-4cc4-8344-c80f2fc7e950">

**개선된 점**

- 주방 직원은 이제 온전히 요리와 부엌 관리에만 집중할 수 있다.
    
    = 비즈니스 로직은 비즈니스 로직에만 집중할 수 있다.
    
- 주문에 대한 관리를 주문서 객체에게 위임 할 수 있다.
    
    ex) 주문 취소를 주문서 객체에서 실행한다.
    
- 주문 관련 변경 사항이 생길 경우 주문(커맨드) 부분만 수정하면 된다.
    
    주문에 대한 변경 발생 시 손님, 주방에 영향을 주지 않는다.

### 커맨드를 구성하는 요소

1. **커맨드(Command)**: 실제 실행될 동작을 캡슐화한 객체. 명령을 수행하기 위한 **`execute()`** 메서드를 정의
2. **인보커(Invoker)**: 커맨드 객체를 호출하고 실행하는 역할. 인보커는 커맨드 객체를 갖고 있고, 필요할 때 커맨드의 **`execute()`** 메서드를 호출하여 요청을 수행
3. **수신자(Receiver)**: 실제 동작을 수행하는 객체로, 커맨드 객체에서 호출되는 동작을 수행

## 코드 예시로 직접 살펴보는 커맨드 패턴

램프를 껐다 켰다 할 수 있는 버튼을 제작해보자!

### Command

```java
public interface Command {
    public abstract void execute();
}

```

동작을 수행하는 execute 메서드가 포함된 Command 인터페이스

### Button

```java
public class Button {
    private Command command;
  
    public Button(Command command) {
        setCommand(command);
    }
  
    public void setCommand(Command command) {
        this.command = command;
    }
  
    public void pressed() {
        command.execute();
    }
}
```

Button 클래스의 pressed 메서드에서 구체적인 기능(램프 켜기, 끄기)을 직접 구현하는 대신 버튼을 눌렀을 때 실행될 기능을 Button 클래스 외부에서 제공받아 캡슐화해 pressed 메서드에서 호출한다.

### Lamp

```java

public class Lamp {
    public void turnOn() {
        System.out.println("Lamp On");
    }
  
    public void turnOff() {
        System.out.println("Lamp Off");
    }
}
```

### LampOnCommand

```java
public class LampOnCommand implements Command {
    private Lamp lamp;
  
    public LampOnCommand(Lamp lamp) {
        this.lamp = lamp;
    }
  
    public void execute() {
        lamp.turnOn();
    }
}
```

• LampOnCommand 클래스에서는 execute 메서드를 구현해 램프 켜는 기능을 구현한다.

### Client

```java
public class Client {
    public static void main(String[] args) {
        Lamp lamp = new Lamp();
        
        Command lampOnCommand = new LampOnCommand(lamp);
        Command lampOffCommand = new LampOffCommand(lamp);
      
        Button button = new Button(lampOnCommand);
        button.pressed();
      	
        button.setCommand(lampOffCommand);
        button.pressed();
    }
}

```

## 다이어그램

<img width="682" alt="Untitled 2" src="https://github.com/kkkwp/CS-study/assets/113974911/67465409-c58c-44f6-b61f-ea541d3d6c7a">
<br >

버튼: 인보커

커맨드: 인터페이스

램프 관련 커맨드: 커맨드

램프: 리시버

<img width="708" alt="Untitled 3" src="https://github.com/kkkwp/CS-study/assets/113974911/90f3ed88-aa76-41fa-a444-bd20342ae9aa">



## 커맨드 패턴이 유용한 상황

- **이벤트가 발생했을때 실행될 기능이 다양하면서도 변경이 필요한 경우**
- **커맨드 발생 시점을 사용자가 커스터마이징해야 할때**
    - 동작의 연기 및 저장: 요청을 나중에 실행하도록 연기하거나 저장할 수 있다.
- **여러 커맨드를 조합하여 하나의 커맨드처럼 사용할 필요가 있을때**
    - 요청의 큐화: 여러 요청을 큐에 넣어 순차적으로 처리 가능하다.
- **커맨드 실행 취소, 재실행 등의 기능을 구현해야 할때**

## 장점

인보커와 리시버, 커맨드가 각각 캡슐화 되어서 결합도가 낮아진다.

## 단점

리시버 객체의 동작이 늘어날 때 마다 커맨드 클래스가 늘어나기 때문에 클래스가 많아진다.

## 커맨드 패턴이 보장하는 SOLID원칙

- 작업을 수행하는 객체와 작업을 요청하는 객체를 분리하기 때문에 SRP 원칙을 잘 준수
- 기존 코드 수정 없이 새로운 리시버와 새로운 커맨드 추가가 가능하기 때문에 OCP 원칙을 준수

## 자바 스프링에서의 커맨드 패턴

### `ExecutorService` in 자바 

: Java의 다중 스레드 환경에서 스레드를 관리하고 작업을 실행하는 인터페이스로서, 스레드 관련 작업을 추상화하고 일반적인 스레드 관련 문제를 해결하기 위한 메서드와 기능을 제공

```java
public class CommandInJava {
    public static void main(String[] args) {
        Light light = new Light();
        Game game = new Game();
        ExecutorService executorService = Executors.newFixedThreadPool(4);
        executorService.submit(light::on);
        executorService.submit(game::start);
        executorService.submit(game::end);
        executorService.submit(light::off);
        executorService.shutdown();
    }
}
```

- 전달받은 `Runnable` 객체를 스레드에 올려준다.
- `ExecutorService` 는 세부사항을 모르고, 그냥 `receiver` 인 `thread` 에게 `Runnable` 혹은 `Callable` 타입의 객체를 주고, `thread` 는 이를 실제로 호출하여 명령을 수행한다.

## 다른 패턴과의 차이점

#### 전략 패턴 vs 커맨드 패턴
**전략 패턴**
같은 일을 하되 그 알고리즘이나 방식을 캡슐화 해서 갈아끼울 수 있는 것
**커맨드 패턴** 
전략패턴처럼 알고리즘이나 방식을 캡슐화해서 갈아끼우기보다는 기능 자체를 캡슐화해서 갈아끼울 수 있게 한다. 커맨드 명령마다 하는일 자체가 다르다. 그래서 여러 명령어를 목록으로 실어보내서 차례로 실행시키는 것이 가능한것이다!
