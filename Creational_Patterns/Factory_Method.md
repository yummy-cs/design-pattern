# 🏭 Factory Method

## ✅ SOLID

> 💡 객체 지향 프로그래밍 및 설계의 5가지 기본 원칙!

유지보수와 확장이 쉬운 시스템을 만들고자 할 때

소스 코드를 리팩토링하여 코드 냄새(코드에서 문제를 일으킬 가능성이 있는 프로그램 소스 코드)를 제거하기 위해 적용할 수 있는 지침

#### SRP(Single Responsibility Principle)

    한 클래스는 하나의 책임만 가져야 한다.

#### OCP(Open/Closed Principle)

    확장에는 열려 있으나, 변경에는 닫혀 있어야 한다.

#### LSP(Liskov Substitution Principle)

    객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.

#### ISP(Interface Segregation Principle)

    큰 덩어리의 인터페이스들을 구체적이고 작은 단위들로 분리시킴으로써 클라이언트들이 꼭 필요한 메서드들만 이용할 수 있게 한다.

#### DIP(Dependency Inversion Principle)

    첫째, 상위 모듈은 하위 모듈에 의존해서는 안된다. 상위 모듈과 하위 모듈 모두 추상화에 의존해야 한다.
    둘째, 추상화는 세부 사항에 의존해서는 안된다. 세부사항이 추상화에 의존해야 한다.

---

## ✅ Template Method

> 💡 하위 클래스에서 구체적으로 처리!

![Untitled1](https://github.com/kkkwp/CS-study/assets/67499154/8a74178a-1f59-435c-94a2-418ea5a430b4)

상위 클래스에서 처리의 뼈대를 결정하고, 하위 클래스에서 그 구체적 내용을 결정한다.

상위 클래스의 코드만으로는 최종적으로 어떻게 처리되는지 알 수 없다!

추상 클래스를 실제로 구현하는 것은 하위 클래스

- 하위 클래스에서 메서드를 구현해야 구체적인 처리 방식이 정해진다.
- 다른 하위 클래스에서 구현을 다르게 하면? 처리도 다르게 이루어진다.

---

## ✅ Factory Method

> 💡 인스턴스를 하위 클래스에서 만들기!

앞서 설명한 ***Template Method 패턴을 인스턴스 생성에 적용*** 한 것이 바로 Factory Method이다.

Factory Method에서는 인스턴스 생성 방법을 상위 클래스에서 결정하되, 구체적인 것들은 하위 클래스에서 결정한다. = 위임

→ 인스턴스 생성을 위한 뼈대(프레임워크)와 실제 인스턴스를 생성하는 클래스를 나누어 생각!

새로운 구현 클래스가 추가되어도, 기존 Factory를 수정하는 대신 새로운 Factory를 추가하면 된다.

#### 언제 적용할까?

- 함께 작동해야 하는 객체들의 정확한 유형과 의존 관계를 모를 때
- 내부 컴포넌트를 확장할 때
- 기존 객체들을 매번 재구축하는 대신 이들을 재사용하여 시스템 리소스를 절약하고 싶을 때

#### 장점

- 생성자(creator)와 구현 객체(product)의 ***단단한 결합을 피할 수 있다.***
- ***캡슐화, 추상화***를 통해 ***정보를 은닉***할 수 있다.
- ***SRP*** 준수: 객체 생성 코드를 한 곳으로 이동하여 코드를 더 쉽게 유지관리할 수 있다.
- ***OCP*** 준수: 기존 코드를 수정하지 않고 새로운 유형의 제품 인스턴스를 도입할 수 있다.

#### 단점

- 제품 구현체마다 팩토리 객체를 구현해야 하므로 구현체가 늘어날 때마다 하위 클래스 수가 폭발 → 코드의 복잡성이 증가!

---

## ✅ Class Diagram

![Untitled2](https://github.com/kkkwp/CS-study/assets/67499154/32d1f1bc-008e-4edd-9165-61451d1293bf)

상위 클래스(뼈대) 쪽에 있는 `Factory`와 `Product`의 관계가 

하위 클래스(구체적인 내용) 쪽에 있는 `IDCardFactory`와 `IDCard`의 관계와 병행하고 있다.

**Product**

    인스턴스가 가져야할 인터페이스(API)를 결정하는 추상 클래스
    구체적인 내용은 `IDCard`에서 결정

**Factory**

    Product를 생성하는 추상 클래스
    구체적인 내용은 `IDCardFactory`에서 결정
    `new`를 사용해 실제 인스턴스를 생성하는 대신에, 인스턴스를 생성하는 메서드를 호출함으로써 구체적인 클래스 이름에 의한 속박에서 상위 클래스를 자유롭게 함

**IDCard**

    구체적인 제품을 결정

**IDCardFactory**

    구체적인 제품을 만들 클래스를 결정

---

## ✅ Code

framework/Product.java
    
```java
package framework;
    
public abstract class Product {
    public abstract void use();
}
```
    
framework/Factory.java
    
```java
package framework;
    
public abstract class Factory {
    // protected: 같은 클래스, 하위 클래스 및 같은 패키지 내의 클래스만 접근 가능
    protected abstract Product createProduct(String owner);
    protected abstract void registerProduct(Product product);
    	
    // final: IDCardFactory에서 상속 받아도 수정은 불가능
    public final Product create(String owner) {
        Product p = createProduct(owner);
        registerProduct(p);
        return p;
    }
}
```
    
idcard/IDCard.java
    
```java
package factoryMethod.idcard;
    
import factoryMethod.framework.Product;
    
public class IDCard extends Product {
    private final String owner;

    IDCard(String owner) {
        System.out.println(owner + "의 카드를 만듭니다.");
        this.owner = owner;
    }
    
    @Override
    public void use() {
        System.out.println(this + "을 사용합니다.");
    }
    
    @Override
    public String toString() {
        return "IDCard{" +
            "owner='" + owner + '\'' +
            '}';
    }

    public String getOwner() {
        return owner;
    }
}
```
    
idcard/IDCardFactory.java
    
```java
package factoryMethod.idcard;
    
import factoryMethod.framework.Factory;
import factoryMethod.framework.Product;
    
public class IDCardFactory extends Factory {
    @Override
        protected Product createProduct(String owner) {
            return new IDCard(owner);
    }
    
    @Override
    protected void registerProduct(Product product) {
        System.out.println(product + "을 등록했습니다.");
    }
}
```

---

## ✅ 추가

### 상위 클래스(추상)가 하위 클래스(구현)에 의존하지 않는다

위와 같은 프레임워크를 사용해서 IDCard가 아니라 TV ‘제품’과 ‘공장’을 만든다면?

framework 패키지 내용을 수정하지 않고도 TV로 전환할 수 있다.

→ framework 패키지는 idcard 패키지에 의존하지 않는다!

<br>

### Factory의 메서드를 구현하는 방법

**1. 추상 메서드로 기술**
    
추상 메서드로 기술하면, 하위 클래스에서는 반드시 이 메서드를 구현해야만 한다.
    
```java
abstract class Factory {
    public abstract Product createProduct(String name);
    ...
}
```

**2. 디폴트 구현을 준비**
    
디폴트 구현을 준비해 두면, 하위 클래스에서 구현하지 않은 경우에 디폴트 구현이 사용된다.
    
단, 이 경우에는 `Product` 클래스에 대해 직접 `new`를 실행하므로 Product 클래스를 추상 클래스로 둘 수는 없다.
    
```java
class Factory {
    public Product createProduct(String name) {
        return new Product(name);
    }
    ...
}
```
