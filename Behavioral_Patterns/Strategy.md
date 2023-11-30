# 👓전략 패턴(Strategy Pattern)

출처
- [💠 전략(Strategy) 패턴 - 완벽 마스터하기 (tistory.com)](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%A0%84%EB%9E%B5Strategy-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90)
- [전략 패턴 (refactoring.guru)](https://refactoring.guru/ko/design-patterns/strategy)
- [[디자인패턴] 전략 패턴 ( Strategy Pattern ) :: victolee (tistory.com)](https://victorydntmd.tistory.com/292)

### 1. 개념

- 객체의 행위를 동적으로 바꾸고 싶은 경우 `직접 행위를 수정하지 않고 전략을 바꿔주기만 함으로써 행위를 유연하게 확장`하는 디자인패턴
- 즉, 객체가 할 수 있는 행위를 미리 만들어 두고, 행위의 수정이 필요할 때 전략의 이름만 바꾸는 것으로 행위의 수정이 가능하도록 만든 패턴

### 2. 구조

![1](https://github.com/hig0ni/CS-study/assets/111436454/849eb9fd-7707-4660-8ea8-fc4e49f77e3f)

- **Context**: 알고리즘을 실행해야 할 때마다 해당 알고리즘과 연결된 전략 객체의 메소드를 호출
- **Strategy** : 모든 전략 구현제에 대한 공용 인터페이스, 콘텍스트가 전략을 실행하는 데 사용하는 메서드를 선언
- **ConcreateStrategies**: Strategy 인터페이스로 부터 알고리즘, 행위, 동작을 정의한 구현체
- **Client**: 특정 전략 객체를 만들어 콘텍스트에 전달

### 3. Java 코드로 패턴 이해하기

![2](https://github.com/hig0ni/CS-study/assets/111436454/b7517a9a-5400-4297-9cad-878b82b676bf)

예를 들어, 운송수단에 기차와 버스가 있다고 가정하자.

운송수단에 따른 move()를 정의할때,

이 두 클래스는 “Movable” 라는 이름의 인터페이스를 구현했다고 가정하고 

그리고 Train과 Bus 객체를 사용하는 Client가 있다.

### **Strategy 패턴 적용 X**

```java
public interface Movable {
    public void move();
}

public class Train implements Movable{
    public void move(){
        System.out.println("선로를 통해 이동");
    }
}

public class Bus implements Movable{
    public void move(){
        System.out.println("도로를 통해 이동");
    }
}

public class Client {
    public static void main(String args[]){
        Movable train = new Train();
        Movable bus = new Bus();

        train.move();
        bus.move();
    }
}
```

기차는 선로를 따라 이동하고, 버스는 도로를 따라 이동한다.

그러다 시간이 흘러 선로를 따라 움직이는 버스가 개발되었다고 가정해보자.

그러면 Bus의 move() 메서드를 다음과 같이 바꿔주기면 하면 된다.

```java
public class Bus implements Movable{
    public void move(){
        System.out.println("선로를 통해 이동");
    }
}
```

그런데 이렇게 수정하는 방식은 **SOLID의 원칙 중 OCP( Open-Closed Principle )에 위배**된다.

`(OCP: 확장에는 열려 있거나 변경에는 닫혀 있어야한다.)`

OCP에 의하면 기존의 move()를 수정하지 않으면서 행위가 수정되어야 하지만, 지금은 Bus의 move() 메서드를 직접 수정했다.

또한 지금과 같은 방식의 변경은 시스템이 확장이 되었을 때 유지보수를 어렵게 한다. 

예를 들어, 버스와 같이 도로를 따라 움직이는 택시, 자가용, 고속버스, 오토바이 등이 추가된다고 할 때, 모두 버스와 같이 move() 메서드를 사용한다.

만약에 새로 개발된 선로를 따라 움직이는 버스와 같이, 선로를 따라 움직이는 택시, 자가용, 고속버스 ... 등이 생긴다면, 각 대중교통의 move() 메서드를 일일이 수정해야 할 뿐더러, 같은 메서드를 여러 클래스에서 똑같이 정의하고 있으므로 메서드의 중복이 발생하고 있다.

즉, 지금과 같은 수정 방식은 다음과 같은 문제점을 갖는다.

- OCP 위배
- 시스템이 커져서 확장이 될 경우 메서드의 중복문제 발생

따라서 이를 해결하고자 전략 패턴을 사용한다.

### **Strategy 패턴 적용 O**

![3](https://github.com/hig0ni/CS-study/assets/111436454/1b28be56-640b-4ef5-b656-90c43cc8fce9)

### 1) 먼저 전략을 생성한다.

운송수단은 움직이는 2가지 방법을 갖고있다.

1. 선로를 따라 움직인다.
2. 도로를 따라 움직인다.

따라서 움직이는 두 방식에 대해 Strategy를 생성한다. ( RailLoadStrategy, LoadStrategy )

그리고 두 클래스는 move() 메서드를 구현하여, 어떤 경로로 움직이는지에 대해 구현한다.

두 전략 클래스를 캡슐화 하기 위해 MovableStrategy 인터페이스를 생성하는데,

이렇게 캡슐화를 하는 이유는 운송수단에 대한 전략 뿐만 아니라,

다른 전략들( 예를 들어, 주유방식에 대한 전략, 돈을 지불하는 방식에 대한 전략 등)이 추가적으로 확장되는 경우를 고려한 설계이다.

```java
public interface MovableStrategy {
    public void move();
}

public class RailLoadStrategy implements MovableStrategy{
    public void move(){
        System.out.println("선로를 통해 이동");
    }
}

public class LoadStrategy implements MovableStrategy{
    public void move() {
        System.out.println("도로를 통해 이동");
    }
}
```

### 2) 운송 수단에 대한 클래스를 정의

![4](https://github.com/hig0ni/CS-study/assets/111436454/4cb3145c-060a-4939-9db9-42018aacc671)

다음으로 운송 수단에 대한 클래스를 정의한다.

기차와 버스는 move() 메서드를 통해 움직일 수 있다.

그런데 이동 방식을 직접 구현하지 않고, 어떻게 움직일 것인지에 대한 전략을 설정하여, 그 전략의 움직임 방식을 사용하여 움직이도록 한다. ⇒ setMovableStrategy() 사용

```java
public class Moving {
    private MovableStrategy movableStrategy;

    public void move(){
        movableStrategy.move();
    }

    public void setMovableStrategy(MovableStrategy movableStrategy){
        this.movableStrategy = movableStrategy;
    }
}

public class Bus extends Moving{

}

public class Train extends Moving{

}
```

### 3) Client 구현

이제 Train과 Bus 객체를 사용하는 Client를 구현해보자

Train과 Bus 객체를 생성한 후에, setMovableStrategy() 메서드를 호출하여 각 운송 수단이 어떤 방식으로 움직이는지 설정한다.

그리고 선로를 따라 움직이는 버스가 개발되었다는 상황을 만들어 버스의 이동 방식 전략을 수정했다.

```java
public class Client {
    public static void main(String args[]){
        Moving train = new Train();
        Moving bus = new Bus();

        /*
            기존의 기차와 버스의 이동 방식
            1) 기차 - 선로
            2) 버스 - 도로
         */
        train.setMovableStrategy(new RailLoadStrategy());
        bus.setMovableStrategy(new LoadStrategy());

        train.move();
        bus.move();

        /*
            선로를 따라 움직이는 버스가 개발
         */
        bus.setMovableStrategy(new RailLoadStrategy());
        bus.move();
    }
}
```

### 4. 장단점과 사용 시기

사용시기

- 전략 알고리즘의 여러 버전 또는 변형이 필요할 때 클래스화를 통해 관리
- 알고리즘 코드가 노출되어서는 안되는 데이터에 액세스 하거나 데이터를 활용할 때 (캡슐화)
- 알고리즘의 동작이 런타임(실행중)에 실시간으로 교체 되어야 할때

장점

- 알고리즘을 쉽게 변경 및 대체할 수 있으므로 **유연함**
- 알고리즘 추가 및 수정을 할 때 코드 수정이 최소화되므로 **확장성**이 높아짐
- 알고리즘을 캡슐화했기에 코드 **재사용성**이 좋음
- 각각 알고리즘을 독립적으로 테스트할 수 있음

단점

- 알고리즘이 많아질수록 관리해야할 객체의 수가 늘어난다.

```java
만일 어플리케이션 특성이 알고리즘이 많지 않고 자주 변경되지 않는다면, 
새로운 클래스와 인터페이스를 만들어 프로그램을 복잡하게 만들 이유가 없다.
```

- 적절한 전략을 선택하기 위해 전략 간의 차이점을 파악하고 있어야 한다. (복잡도 증가)

### 5. ****Strategy vs Temaplate Method****

유사점

- 알고리즘을 때에 따라 적용한다는 점
- 개방형 폐쇄 원칙(OCP)을 충족하고 코드를 변경하지 않고 소프트웨어 모듈을 쉽게 확장할 수 있다

차이점

- 전략 패턴은 합성(composition)을 통해 해결책을 강구하며, 템플릿 메서드 패턴은 상속(inheritance)을 통해 해결책을 제시
- 전략 패턴은 클라이언트와 객체 간의 결합이 느슨한 반면, 템플릿 메서드 패턴에서는 두 모듈이 더 밀접하게 결합(결합도가 높으면 안좋음)
- 전략 패턴에서는 대부분 인터페이스를 사용하지만, 템플릿 메서드 패턴에서는 주로 추상 클래스나 구체적인 클래스를 사용
- 전략 패턴에서는 전체 전략 알고리즘을 변경할 수 있지만, 템플릿 메서드 패턴에서는 알고리즘의 일부만 변경되고 나머지는 변경되지 않은 상태로 유지
- 단일 상속만이 가능한 자바에서는 상속 제한이 있는 `템플릿 메서드 패턴보다는`, 다양하게 많은 전략을 implements 할 수 있는 `전략 패턴이 협업에서 많이 사용`되는 편

### 6. ****실무에서 찾아보는 Strategy 패턴****

- Collections의 `sort()` 메서드에 의해 구현되는 `compare()` 메서드에 이용

자바에서 익명 클래스로 구현체를 그때그때 정의하고 안의 동작 메소드의 로직을 그때그때 만들어 할당하는 방식도 전략을 실시간으로 변경하여 지정하는 패턴과 유사하다고 볼 수 있다.

```java
class StrategyInJava {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>();
        numbers.add(2);
        numbers.add(1);
        numbers.add(3);
        numbers.add(5);
        numbers.add(4);

        // sort 메서드의 매개변수로 익명 클래스로 Comparator 객체를 인스턴스화하여
        // 그 안의 compare 메서드 동작 로직(ConcreteStrategy)를 직접 구현하여 할당하는 것을 볼 수 있다.
        Collections.sort(numbers, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2;
            }
        });

        System.out.println(numbers);
    }
}
```

- javax.servlet.http.HttpServlet에서 `service()` 메서드와 모든 `doXXX()` 메서드에 이용

```java
package javax.servlet.http;

import javax.servlet.Servlet;
import java.io.IOException;

public class HttpServlet implements Servlet {     
    @Override
    public void init() {

    }

    @Override
    public void service(HttpServletRequest request, HttpServletResponse response) throws IOException {    
        if ("GET".equals(request.getMethod())) {
            doGet(request, response);      
        } else {
            doPost(request, response);     
        }
    }
```

- javax.servlet.Filter의 `doFilter()` 메서드에 이용

doFilter()는 가장 핵심인 Filtering이 이루어지는 메소드로 필터에서 구현해야 하는 로직을 작성한다.

```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
//여기에 전처리
chain.doFilter(request, response);
//여기에 후처리
}
```
