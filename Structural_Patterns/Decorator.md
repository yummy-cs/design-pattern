# Decorator Pattern

  래퍼 객체를 이용해 모듈과 비슷한 방식으로 기존 객체에 기능을 추가하고 조합하여 사용할 수 있는 디자인 패턴
  - 기존 기능 및 다른 객체에 영향을 주지 않고 개별 객체에 새로운 책임을 추가하는 용도로 사용 가능하다.
- 런타임에 객체에 '행위' 혹은 '기능'을 추가할 수 있게 해준다.
- `기존 객체`를 `'행위'를 가진 특별한 래퍼 객체 (데코레이터)`에 넣어서 객체가 그 '행위'를 할 수 있게 만든다.
- 캐싱, 로깅, 검증과 같은 기능에 쓰일 수 있다.

---

## (예제) 피자만들기로 살펴보는 Decorator Pattern의 필요성!

### 일반 피자

```java
public class Pizza {
    protected String pizzaName() {
        return "피자";
    }
}
```
치즈 피자와 불고기 피자를 만들어야 하는 요구사항이 들어왔다!
→ 상속을 이용해 클래스로 간단 구현 가능

```java
public class CheesePizza extends Pizza {
    @Override
    protected String pizzaName() {
        return "치즈 " + super.pizzaName();
    }
}
```

```java
public class BulgogiPizza extends Pizza {
    @Override
    protected String pizzaName() {
        return "불고기 " + super.pizzaName();
    }
}
```

- 손님들의 반응이 좋아서 `치즈 불고기 피자` 를 만들어야겠다!
    - 기존의 상속 방식을 그대로 이용하려면, `치즈 피자`를 상속해서 만들거나 `불고기 피자` 를 상속해서 `치즈 불고기 피자` 클래스를 만들 수는 있다.
    - 장사가 잘돼서 나중에 `씬도우`, `치즈 크러스트` 등의 피자가 추가되면, `치즈크러스트 치즈 불고기 피자`, `씬도우 치즈크러스트 치즈 불고기 피자`, `씬도우 불고기 피자`, `씬도우 치즈 피자` 등 수 없이 많은 종류의 피자들에 대해 새로운 상속 클래스를 만들면 구조가 너무 복잡해지지 않을까?

⇒ 이 문제를 해결하기 위해 데코레이터 패턴을 적용해보자.

## (예제) 데코레이터 패턴으로 구현하는 다양한 조합의 피자!

### 피자서비스 인터페이스 생성

```java
public interface PizzaService {
    String pizzaName();
}
```

### 기본 피자 생성

```java
public class DefaultPizza implements PizzaService{
    @Override
    public String pizzaName() {
        return "피자";
    }
}
```

### 피자 데코레이터 추상 클래스 생성

```java
public abstract class PizzaDecorator implements PizzaService {
    PizzaService pizzaService;

    public PizzaDecorator(PizzaService pizzaService) {
        this.pizzaService = pizzaService;
    }

    @Override
    public String pizzaName() {
        return pizzaService.pizzaName();
    }
}
```

- 피자를 꾸며주는 추상 클래스
    - 추상 클래스인만큼 자체적으로 인스턴스가 될 수는 없다.

### 치즈 데코레이터 클래스 생성

```java
public class CheeseDecorator extends PizzaDecorator{
    public CheeseDecorator(PizzaService pizzaService) {
        super(pizzaService);
    }

    @Override
    public String pizzaName() {
        return "치즈 " + super.pizzaName();
    }
}
```

### 불고기 데코레이터 클래스 생성

```java
public class BulgogiDecorator extends PizzaDecorator{
    public BulgogiDecorator(PizzaService pizzaService) {
        super(pizzaService);
    }

    @Override
    public String pizzaName() {
        return "불고기 " + super.pizzaName();
    }
}
```

### 치즈 불고기 피자를 만들어보자!

```java
public class Client {
    private static boolean enabledBulgogi = true;
    private static boolean enabledCheese = true;

    public static void main(String[] args) {
        PizzaService pizza = new DefaultPizza();

        if (enabledBulgogi) {
            pizza = new BulgogiDecorator(pizza);
        }

        if(enabledCheese) {
            pizza = new CheeseDecorator(pizza);
        }

        System.out.println(pizza.pizzaName());
    }
}

//실행 결과: 치즈 불고기 피자

```

⇒ (치즈(불고기(피자)))

## 데코레이터 패턴 구조

![Decorator](https://github.com/kkkwp/CS-study/assets/113974911/7816e653-7f2b-40a9-bce1-b46006be0f57)


피자 예제를 기반으로 표현해보자면,

    PizzaService(=Component): 피자에 대한 추상적인 설계 정의

    Default Pizza(=Concret Component): 피자를 구체적으로 구현하는 구체 클래스 

    PizzaDecorator(=Decorator): 피자에 토핑을 올릴 수 있게하는 추상 클래스 

    CheeseDecorator, BulgogiDecorator(=Concrete Decoator 1,2): PizzaDecorator를 상속하여 실제로 토핑을 추가하는 데코레이터 클래스

---

## Decorator Pattern의 장점

- 상속의 치명적 단점(강한 결합이 강제 되는 것, 복잡한 구조)들을 데코레이터의 합성(=조합)을 통해 피할 수 있음 → 유연한 설계 가능

## Decorator Pattern의 단점

데코레이터를 조합하는 코드가 조금 복잡할 수 있다.

- 작은 객체들이 많이 늘어난다.
- 객체들이 상속보다 많이 늘어나진 않아서 큰 단점은 아니지만, Component가 무거워지면 Decorator도 무거워져 새로운 Decorator를 추가하기 어려워진다. **만약 Component가 본질적으로 매우 복잡하고 무거운 특성을 갖는다면 Strategy 패턴이 더 좋은 해결 방안일 수 있다.**

## 데코레이터 패턴이 지키는 SOLID

**개방/폐쇄 원칙**

기존 코드를 수정하지 않고도 새로운 기능을 추가

**단일 책임 원칙**

각 데코레이터 클래스는 특정한 기능 또는 책임을 담당하도록 설계

코드의 모듈화와 유지보수를 향상시키며 복잡성을 낮추는 데 도움을 준다.

## 스프링에서 사용되는 데코레이터 패턴

### HandlerAdapter
- 스프링에서의 `HandlerAdapter` 를 데코레이터 패턴의 예로 볼 수 있다.
- 데코레이터 패턴의 특징은 래퍼 객체를 이용해 기존 객체에 영향을 미치지 않고 새로운 기능을 추가하는 것이라 볼 수 있다.
- `HandlerAdapter` 는 스프링에서 특정한 타입의 컨트롤러를 디스패처 서블릿에 사용할 수 있게 해준다.
    - `SimpleControllerHandlerAdapter` 는 `Controller` 인터페이스를 구현한 컨트롤러를 사용할 수 있게 해준다.
    - `RequestMappingHandlerAdapter` 는 `@RequestMapping` 주석이 달린 컨트롤러를 사용할 수 있게 해준다.
- 각 `HandlerAdapter` 들은 기존의 동작을 해치지 않고, 새로운 종류의 컨트롤러가 동작할 수 있도록 도와준다.

### HandlerAdapter는 어댑터 패턴 아니야?

- `HandlerAdapter` 는 코드를 보는 시선에 따라 어댑터 패턴과 데코레이터 패턴 둘 다 될 수 있다.
- `HandlerAdapter` 의 의도를 `서로 다른 코드와의 호환` 이라고 본다면, 어댑터 패턴이 된다.
- `HandlerAdapter` 에 어댑터를 추가하여 `기존 객체에 영향을 미치지 않고 새로운 기능을 추가하는 것` 이라고 본다면, 데코레이터 패턴이 된다.

---
#### reference
https://jake-seo-dev.tistory.com/406
