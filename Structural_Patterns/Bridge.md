# Bridge

## Structure Pattern

- 작은 클래스들을 `상속` , `합성` 을 이용해  더 큰 클래스를 생성하는 방법을 제공하는 패턴이다.
- 해당 그룹에 속한 패턴들을 사용하면 서로 독립적으로 개발된 클래스 라이브러리들에 대해 인터페이스를 `합성`, `상속` 하여 마치 하나인 것 처럼 사용할 수 있게 된다.
    - 어댑터 / 브릿지 / 컴포즈 / 데코레이터 / 퍼사드 / 플라이트 / 프록시

## Bridge Pattern 이란?

- `추상화(기능)을 처리하는 클래스` , `구현을 담당하는 추상 클래스`로 구별하여 이 둘 이 서로 독립적으로 변화에 대응할 수 있도록 해주는 패턴
- 구현부에서 추상층을 분리해 각자 독립적으로 변형이 가능하고 확장이 가능하도록 하여 결합도를 낮출 수 있는 패턴
- 색을 가진 도형을 그리는 프로그램을 `만들 때`, 아래 그림의 전자와 같이 구현체 클래스에서 색깔과 자신의 모양을 결정하는 기능과 자신의 색에 대한 구현을 담당하는 코드가 한 클래스에 있을 때, 이는 도형에 삼각형, 색에 노랑 만 추가 되어도, 아래와 같이 클래스가 기하급수적으로 늘어난다.
    - 노랑 - 삼각형(+) / 구체(+) / 사각형(+)
    - 빨강 - 삼각형(+) / 구체 / 사각형
    - 파랑- 삼각형(+) / 구체 / 사각형
    
- 하지만 브릿지 패턴을 이용해 `만들게 되면` 기능 추상 클래스인 모양이 구현을 담당하는 색 클래스를 필드로 가져가 합성하여 해당 패턴을 적용하면 `기능에 대한 추상화 클래스`에 해당되는 Shape 이 `구현을 담당하는 추상 클래스` Color 를 필드로 가져 합성이 이뤄지면, 클라이언트 코드에서 요구하고 있는 `기능에 대한 구현체` Circle, Square 과 `구현을 담당하는 구현체` Red, Blue 에 맞게 Shape 구현체의 인스턴스를 적절하게 제공할 수 있으며, 이는 도형 삼각형과 색깔 노랑이 추가 되어도 `기능` , `구현` 각 계층의 클래스를 작성해서 관리할 수 있도록 해준다.

  ![image](https://github.com/kkkwp/CS-study/assets/97209766/aad0e8e2-2cde-4d56-afa3-36a660981ed9)

# Bridge Pattern 구조

- Bridge Pattern 구조도
    
    ![image](https://github.com/kkkwp/CS-study/assets/97209766/f3cfd145-d2bb-449d-a32b-41593dbc2301)
    
- Abstraction
    - `기능` 혹은 `정의에 대한 추상화` 계층의 최상위 클래스로 추상 인터페이스를 정의하고 있다.
    - 구현 부분에 해당하는 클래스의 인스턴스를 가지고 해당 인스턴스를 통해 `구현 부분의 메서드를 호출`한다.

- RefinedAbstraction
    - Abstraction 클래스에 의해 정의된 인터페이스를 확장(재정의)하고 있다.
    - `기능` 계층에서 `새로운 부분을 확장 및 재정의 하게 되는 클래스` 이다.

- Implementor
    - `구현` 클래스를 위한 인터페이스를 정의하고 있는 클래스.
    - Abstraction 클래스의 `기능을 구현하기 위한 인터페이스를 정의`한다.

- ConcreteImplementor
    - Implementor 의 인터페이스에 대한 실제 기능과 정의에 대한 실제 부분을 구현하고 있다.

# Bridge Pattern 코드 예시

## 간단한 구현 예시

- Abstraction 에 해당되는 Shape 인터페이스
  
  추상 클래스를 사용하지 않고 인터페이스를 사용함으로써 RefinedAbstraction 에서 Abstraction 의 역할을 넘겨, 정의와 기능에 대한 추상화 계층 분리의 역할만을 남기고, Client 코드에서 브릿지 패턴 적용으로 구현과 추상의 기능이 분리된 인스턴스를 생성 및 호출할 수 있도록 한다.
    
    ```java
    
     public interface Shape {
    	 void draw();
    }
    ```

- RefinedAbstraction 에 해당되는 Rectangle 클래스
    
    ```java
    
    public class Rectangle implement Shape{
    	private final Color color;
    
      public Rectangle(Color color) {
        this.color = color;
      }
     
      @Override
      public void draw() {
        System.out.println(color.getColorCode() + " 색깔 사각형을 그립니다.");
      }
    }
    ```

- RefinedAbstraction 에 해당되는 Circle 클래스
    
    ```java
    public class Circle implement Shape{
    	private final Color color;
    
      public Circle(Color color) {
        this.color = color;
      }
     
      @Override
      public void draw() {
        System.out.println(color.getColorCode() + "색깔 원을 그립니다.");
      }
    }
    ```
    
- RefinedAbstraction 클래스들은 Abstraction 의 역할을 일부 받아 Implementor에 해당되는 Color 인터페이스를 정의 및 필드로 가지고(`합성`) Abstraction인 Shape의 기능에 해당되는 `draw()`를 재정의 하고 있다.

  또한 Abstraction의 역할로 정의된 것 중 하나인 Implementor인 Color 의 메서드 `getColorCode()` 를 호출하고 있다.
  
  이는 `Shape이 가질 기능이나 정의에 대해 추상화 및 정의`하고 있는 것으로 Shape 에 대한 기본적인 구조, 틀을 작성한 계층이다.

---

- Implementor 에 해당되는 Color 인터페이스

  Abstraction 인 Shape 인터페이스의 `기능을 구현하기 위한 인터페이스를 정의`
    
    ```java
    public interface Color{
      String getColorCode();
    }
    ```
    
- ConcreteImplementor 에 해당되는 Red 클래스
    
    ```java
    public class Red implements Color {
        @Override
        public String getColorCode() {
            return "FF0000";
        }
    }
    ```

- ConcreteImplementor 에 해당되는 Blue 클래스
    
    ```java
    public class Blue implements Color {
        @Override
        public String getColorCode() {
            return "0000FF";
        }
    }
    ```
    

- ConcreteImplementor 인 Red, Blue 클래스는 Implementor 에 해당되는 Color 인터페이스를 정의함으로 실제로 Client에서 필요한 X색깔 Y도형에 대한 인스턴스 `구현` 을 책임지는 계층이다.

  이로써 Shape 타입이 필요한 곳에서 Rectangle인지 Circle인지 무관하게 두 구현체 중 원하는 색으로 가져와 사용할 수 있다.

  RedRectangle, BlueRectangle, RedCircle, BlueCircle과 같은 개별 클래스의 생성을 방지할 수 있다.

# 장/단점

### 장점

- `기능` 을 담당하는 추상적인 코드에 대해 `구현` 계층에서 알 필요가 없으며 `기능` 반대의 경우에도 마찬가지이기 때문에 각자의 계층에서의 변경과 확장에 용이하다.
- 구조적인 틀(기능 및 추상화 계층) 과 구체적인 코드(구현 계층) 가 분리되어 있어 코드의 주입이 자유롭다.

### 단점

- 계층 구조가 늘어나 복잡도가 증가할 수 있다.
    - 단 하나의 클래스만 만들고 추후에 확장 가능성이 아예 존재하지 않는다면, 오히려 번거로운 작업이 될 수 있다.
    - 처음 보는 개발자는 코드를 파악하는데 어려움을 겪을 수도 있다.

# 적용 시기

- 부모 추상 클래스가 기본 규칙 세트를 정의하고 구체적인 클래스가 추가 규칙을 추가하고 싶은 경우
- 객체에 대한 참조가 있는 추상 클래스가 있고 각 구체적인 클래스에서 정의될 추상 메서드가 있는 경우

# 느낀점

## Object Adaptor Pattern 과의 유사함과 적용 시점에 대한 차이

- Object Adaptor Pattern
    - 객체 어댑터 패턴은 한 클래스의 인터페이스를 다른 클래스의 인터페이스로 변환하는 것이 주 목적으로 볼 수 있다.
    - 즉, 어떤 클래스의 인터페이스를 클라이언트가 원하는 인터페이스로 맞추는 역할이라고 생각한다.
    - `이미 존재하는 클래스들의 인터페이스를 변경하거나 호환`시키는데 사용되는 경우가 많다.
        - 예를 들어, 서드파티 라이브러리나 외부 컴포넌트를 현재 시스템에 통합할 때 유용하게 사용될 수 있다는 느낌이 강하다.

- Bridge Pattern
    - 브리지 패턴은 추상화와 구현을 분리하여 두 개의 독립적인 계층을 만들고, 이를 연결하여 유연한 디자인을 가능하게 하는 것이 주 목적이라 생각한다.
    - 즉, 브리지 패턴은 인터페이스와 구현부를 분리하는 것이 가장 주된 역할이라고 생각한다.
    - 주로 `추상화와 구현을 분리`하여 두 계층을 독립적으로 확장하고 변경해야 하는 경우의 프로그램을 설계하는 단계에서 주로 사용될 수 있다고 느꼈다.
        - 복잡한 클래스 계층 구조를 간단하게 유지하면서 새로운 기능을 추가하거나 변형해야 할 때 유용하기 때문이며, 이는 이미 어떤 구조로 프로그램을 작성할 지에 대한 고민이 충분히 이뤄진 후에 적용할 수 있다고 생각했기 때문이다.

- 앞서 도형과 색깔로 분리된 예시를 보면 자칫 이 두 가지 의 경우와 비슷하게  어떤 제품 군에 대한 기준으로 추상(기능) 과 구현을 분리하는 모습으로 보여질 수 있다고 생각한다, 실제로 내가 그랬다.
- 이러한 `제품 군에 대한 기준` 보다는 Bridge Pattern은 `클래스에 대한 기능이나 특징 등을 추상화 하는 부분`과 `실제로 동작할 내부 로직을 구현하는 부분`을 기준으로 계층화 하여 사용해 유연성을 증가시킨 것이라 생각하게 되었다.
- 늘, 디자인 패턴 혹은 Java 에서 중요하게 생각하는 SOLID, 다형성 등 의 객체지향 프로그래밍에 있어서, 어댑터 패턴과의 유사성과 차이점에 대한 의문을 생각하게 된 것과 같이, 현재 상황을 통찰하고 판단하기 위해서는 이와 같은 디자인 패턴과 더불어 도메인에 대한 공부와 경험이 필요하다고 다시 한 번 느끼게 되었다.

- 참조
    - [https://jake-seo-dev.tistory.com/397](https://jake-seo-dev.tistory.com/397)
    - [https://www.crocus.co.kr/1537](https://www.crocus.co.kr/1537)
    - [https://hirlawldo.tistory.com/169](https://hirlawldo.tistory.com/169)
    - [https://wellsw.tistory.com/226](https://wellsw.tistory.com/226)
