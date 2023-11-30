# 🎁 Adapter

> 💡 사이에 끼워 재사용하기!

![Untitled](https://github.com/kkkwp/CS-study/assets/67499154/ab1bdaa1-93d3-4d6f-83c6-51bd0f5c7ebd)

이미 제공된 코드를 그대로 사용할 수 없을 때, 필요한 형태로 변환한 후 이용하는 방법이다.

‘이미 제공된 것’과 ‘필요한 것’ 사이의 차이를 메우는 디자인 패턴이라고 할 수 있다. 

무엇인가를 포장해서 다른 용도로 사용할 수 있도록 변환하는 방법이기 때문에 wrapper 패턴이라고도 부른다. 

<br>

## ✅ 언제 적용할까?

- 레거시 코드를 사용하고 싶지만 새로운 인터페이스가 레거시 코드와 호환되지 않을 때
    
    Adapter 패턴을 이용하여 기존 클래스에 한겹 덧씌워 필요한 클래스를 만들 수 있다. 이를 사용하면 필요한 메서드 군을 빠르게 만들 수 있다.

    만약 버그가 발생하더라도 기존 클래스는 버그가 없다는 것을 알고 있기 때문에 Adapter 클래스를 중심으로 살펴봄으로써 검사가 매우 편해진다.
    
- 이미 만들어진 라이브러리를 재사용하고 싶지만 이 라이브러리를 수정할 수 없을 때
  
- 기존 클래스를 새로운 인터페이스(API)에 맞게 개조할 때

- 소프트웨어의 구버전과 신버전을 공존시키고 싶을 때

<br>

## ✅ 장단점

### 장점

- SRP 준수 : 프로그램의 기본 비즈니스 로직에서 인터페이스 또는 데이터 변환 코드를 분리할 수 있다.

- OCP 준수 : 클라이언트가 인터페이스를 통해 어댑터와 작동하므로 기존의 코드를 손상시키지 않고 새로운 어댑터들을 프로그램에 도입할 수 있다.

### 단점

- 다수의 새로운 인터페이스와 클래스들을 도입해야 하므로 코드의 전반적인 복잡성이 증가
  
    → 때로는 서비스 클래스를 변경하는 것이 더 간단하다.

<br>

## ✅ 상속과 위임

> [참고(인파 블로그)](https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5%EC%9D%98-%EC%83%81%EC%86%8D-%EB%AC%B8%EC%A0%9C%EC%A0%90%EA%B3%BC-%ED%95%A9%EC%84%B1Composition-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

### 상속(is-a 관계)

Customer 라는 최상위 클래스를 만들고, 이를 각각 상속 받아 VIPCustomer, GoldCustomer 등으로 자식 클래스를 구현

![스크린샷 2023-09-05 21 19 21](https://github.com/kkkwp/CS-study/assets/67499154/9e29ea7f-995b-4058-99eb-80021056a476)

<br>

### 위임(has-a 관계)

> 💡 위임이란?
>
> 누군가에게 맡긴다 → 어떤 메소드의 실제 처리를 다른 인스턴스의 메서드에게 맡기는 것!

기존 클래스를 상속을 통한 확장하는 대신에, **필드로 클래스의 인스턴스를 참조**

→ 객체 간의 관계가 수직관계가 아닌 수평 관계

![스크린샷 2023-09-05 21 20 39](https://github.com/kkkwp/CS-study/assets/67499154/9257b1ec-c8d4-494d-baf4-cb91a8b8dc9b)

<br>

## ✅ 예시

```java
(Hello)
*Hello*
```

- 이미 제공된 것 → `Banner`(showWithParen, showWithAster)
  
- 어댑터 → `PrintBanner`
  
- 필요한 것 → `Print`(printWeak, printString)

<br>

### 1. 상속(Class)을 이용한 adapter

adapter가 동시에 두 객체의 인터페이스를 상속한다.

따라서 다중 상속을 지원하는 프로그래밍 언어에서만 구현할 수 있다.

![Untitled](https://github.com/kkkwp/CS-study/assets/67499154/becf233e-c70d-4d9d-a6ae-ffff8a15b458)

`Print.java`
    
```java
public interface Print {
    
  public abstract void printWeak();
    
  public abstract void printStrong();
}
```
    
`PrintBanner.java`
    
```java
public class PrintBanner extends Banner implements Print {
    
  public PrintBanner(String string) {
    super(string);
  }
    
  @Override
  public void printWeak() {
    showWithParen();
  }
    
  @Override
  public void printStrong() {
    showWithAster();
  }
}
```
    
`Banner.java`
    
 ```java
public class Banner {
    
  private final String string;
    
  public Banner(String string) {
    this.string = string;
  }

  public void showWithParen() {
    System.out.println("(" + string + ")");
  }
    
  public void showWithAster() {
    System.out.println("*" + string + "*");
  }
}
```
    
`Main.java`
    
```java
public class Main {
    
  public static void main(String[] args) {
    Print p = new PrintBanner("Hello");
    p.printWeak();
    p.printStrong();
  }
}
```

- 제공된 Banner 클래스를 상속(extends) 받아, 필요한 Print 인터페이스를 구현(implements)한다.

- PrintBanner 인스턴스를 Print 인터페이스형 변수에 대입한다. 즉 Banner 클래스는 Main 클래스에서 완전히 숨겨져 있다.

- PrintBanner 클래스가 어떻게 구현됐는지 Main 클래스는 모른다. 따라서 Main 클래스를 전혀 변경하지 않고도 PrintBanner 클래스의 구현을 바꿀 수 있다.

<br>

### 2. 위임(Instance)을 이용한 adapter

adapter가 한 객체의 인터페이스를 구현하고 다른 객체는 래핑한다.

모든 프로그래밍 언어로 구현할 수 있다.

![Untitled](https://github.com/kkkwp/CS-study/assets/67499154/66b7503c-f0d3-4242-a0f5-91e871743eb5)

`Print.java`
    
```java
public abstract class Print {
    	
  public abstract void printWeak();

  public abstract void printStrong();
}
```

`PrintBanner.java`
    
```java
public class PrintBanner extends Print {
    
  private final Banner banner;
    
  public PrintBanner(String string) {
    this.banner = new Banner(string);
  }
    
  @Override
  public void printWeak() {
    banner.showWithParen();
  }
    
  @Override
  public void printStrong() {
    banner.showWithAster();
  }
}
```

- Print는 인터페이스가 아니고 클래스!

    Banner 클래스를 이용하여 Print 클래스와 같은 메서드를 가지는 클래스를 실현하는 방법

- JAVA는 다중 상속을 지원하지 않는다!

    PrintBanner 클래스를 Print와 Banner 양쪽의 하위 클래스로 정의할 수 없다. 따라서 banner 필드를 경유하여 호출하는 방법을 통해 PrintBanner 클래스의 printWeak 메서드가 호출되었을 때 자신이 처리하지 않고 다른 인스턴스(Banner의 인스턴스)인 showWithParen에게 맡긴다.

<br>

## ✅ 비교

일반적으로 상속보다는 위임을 사용하는 것이 문제가 적다. 

- 상위 클래스의 내부 동작을 자세히 모르면 상속을 효과적으로 사용하기 어렵다.
  
- JAVA는 다중 상속을 지원하지 않는다.
