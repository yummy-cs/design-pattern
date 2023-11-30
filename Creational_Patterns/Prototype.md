# Prototype 패턴

<img width="562" alt="image" src="https://github.com/kkkwp/CS-study/assets/113974911/22977c84-e406-49a1-af23-f7937714e560">


클래스의 인스턴스 내용을 복사해서 다른 내용을 더 채워나가는 식으로 사용하는 디자인 패턴
메모리에 올라가있는 인스턴스를 복제하여 새로운 인스턴스를 만드는 방법.
클래스에 의존하지 않으면서도, 기존 객체를 복제하게 해준다.

객체 생성 비용이 크고, 객체 생성 과정이 복잡한 경우에 유용하다.

### 프로토 타입 디자인 패턴이 보장하는 SOLID 원칙
- 개방/폐쇄 원칙 (OCP, Open/Closed Principle)
	프로토타입 패턴은 기존 객체를 복제하여 새로운 객체를 생성하므로, 새로운 객체 타입을 추가하거나 확장할 때 기존 코드를 변경하지 않아도 된다. 이로써 기존 코드를 수정하지 않고도 새로운 객체 타입을 추가하거나 변경할 수 있다.
- 인터페이스 분리 원칙(ISP, Interface Segregation Principle)
	프로토타입 패턴에서는 복제할 객체의 원형을 가지고 있는 인터페이스나 추상 클래스를 정의하고, 실제 구현 클래스들이 해당 인터페이스나 추상 클래스를 구체화해 구현하게 된다. 클라이언트 코드는 복제를 위해 필요한 메서드만 사용하므로 인터페이스 분리 원칙을 준수한다.


### 현실 요구 사항으로 프로토타입 패턴 살펴보기
메이플 스토리와 비슷한 게임을 만든다고 가정했을 때, 
필드에 붉은 돼지 몬스터를 대량으로 출몰시키고 가끔 붉은 돼지 보스도 출몰시켜야 하는 상황

```java

@AllArgsConstructor
public class Field {
    private final String name;
    private final int height;
    private final int width;
}

@AllArgsConstructor
@Builder
public class Monster {
    private final Field appearIn;
    private final String name;
    private final int exp;
    private final int hp;
    private final String species;
    private final int speed;
    private final int power;
    private final int defense;
    private final int size;
}

public class App {
    public static void main(String[] args) {
        Field field10 = new Field("들판", 500, 2400);

        Monster redPig = Monster.builder()
                .name("붉은 돼지")
                .hp(100)
                .defense(5)
                .exp(10)
                .power(10)
                .species("돼지 종족")
                .speed(10)
                .size(5)
                .appearIn(field10)
                .build();
    }
}
// TODO: 붉은 돼지 몬스터 대량으로 출몰시키기.
// TODO: 붉은 돼지들은 각각 독립적인 객체여야 하니 clone != redPig 가 true 여야 한다.
// TODO: 그러나 각 붉은 돼지들의 내용만 보면 같은 객체기 때문에 clone.equals(redPig) 도 true 가 나와야 한다
```

매번 위의 빌드 과정을 다 거치기보다,
`.clone()` 메서드 한번으로 복사하는 프로토타입 패턴을 사용하면 어떨까?

### 객체 복사하는 건 쉬운데 굳이 프로토 타입 패턴이 왜 필요하지?
- 객체에 비공개 필드가 있거나, 많은 상속을 통해 다양한 필드를 늘려온 객체의 경우 복사 시 캡슐화를 위반할 수 있다. 프로토 타입 패턴을 사용하면 객체의 내부 구현을 알 필요 없이 복제할 수 있는 방법을 제공한다.
- 프로토타입 레지스트리를 사용하면, 설정이 복잡하지만 자주 사용되는 객체를 어디 저장해두고 계속 복사해서 사용할 수 있는 편리점이 있다.

### 프로토타입 구현 (Java - Object의 `clone()` 메서드)
- `clone()` 은 자바의 Object 가 기본으로 제공하는 메서드 중 하나. 
- protected 접근자를 가지고 있는데, 사실상 모든 자바 객체는 Object를 상속하니 모든 객체에서 사용이 가능하다. 
  단, Cloneable이라는 인터페이스를 상속하고 구현해야 사용 가능하다.

```java
@AllArgsConstructor
@Builder
public class Monster implements Cloneable{
    private final Field appearIn;
    private final String name;
    private final int exp;
    private final int hp;
    private final String species;
    private final int speed;
    private final int power;
    private final int defense;
    private final int size;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Monster monster = (Monster) o;
        return exp == monster.exp && hp == monster.hp && speed == monster.speed && power == monster.power && defense == monster.defense && size == monster.size && Objects.equals(appearIn, monster.appearIn) && Objects.equals(name, monster.name) && Objects.equals(species, monster.species);
    }

    @Override
    public int hashCode() {
        return Objects.hash(appearIn, name, exp, hp, species, speed, power, defense, size);
    }
}
```
### 프로토타입 사용해보기
```java
public class App {
    public static void main(String[] args) throws CloneNotSupportedException {
        Field field10 = new Field("들판", 500, 2400);

        Monster redPig = Monster.builder()
                .name("붉은 돼지")
                .hp(100)
                .defense(5)
                .exp(10)
                .power(10)
                .species("돼지 종족")
                .speed(10)
                .size(5)
                .appearIn(field10)
                .build();

        Monster cloneRedPig = (Monster) redPig.clone();

        System.out.println(cloneRedPig == redPig); // false (서로 다른 객체 확인)
        System.out.println(cloneRedPig.equals(redPig)); // true (객체 내부 필드들의 동등성 확인)
        System.out.println(cloneRedPig.getClass() == redPig.getClass()); // true (같은 클래스에서 생성된 객체인지 확인)
    }
}
```
> 그런데 Object의 clone()에는 **치명적인 약점**이 하나 있다.
    deep copy 가 아닌 shallow copy 를 지원하기 때문에,
    객체 안의 객체가 가진 필드는 복사되지 않는다.

가령 붉은 돼지 처치 시 아이템 드롭 기능을 만드려고 할때, 문제가 발생한다.

```java
//Item 클래스 생성
public class Item {
    private final String name;
    private final String description;
}

//Monster에 dropItem 필드 추가
public class Monster implements Cloneable{
    // ...
    private final Item dropItem;
    // ...
}

public class App {
    public static void main(String[] args) throws CloneNotSupportedException {
        Field field10 = new Field("들판", 500, 2400);

        Item redPigHeart = Item.builder()
                .name("붉은 돼지의 심장")
                .description("붉은 돼지의 심장이다.")
                .build();

        Monster redPig = Monster.builder()
                .name("붉은 돼지")
                .hp(100)
                .defense(5)
                .exp(10)
                .power(10)
                .species("돼지 종족")
                .speed(10)
                .size(5)
                .appearIn(field10)
                .dropItem(redPigHeart)
                .build();

        Monster cloneRedPig = (Monster) redPig.clone();
        System.out.println(cloneRedPig.getDropItem() == redPig.getDropItem()); // true
    }
}

```
아이템과 아이템을 줍는 모든 유저의 인벤토리는 독립적이어야 하는데, 
clone()을 사용해 붉은 돼지 객체를 얕게 복제 해 사용했을 때, 그 안에 있는 아이템 필드의 참조값이 동일해 
충돌이 발생할 수 있다.

### deep copy를 위해 clone() 재구현

Item 클래스 내부에 clone() 메서드를 구현하고,
Monster 클래스의 clone() 메서드는 해당 객체에 dropItem 으로 설정된 것이 있다면, 
해당 dropItem 을 clone() 하여 할당하는 방식으로 수정해서 해당 문제를 해결한다.

```java
public class Item implements Cloneable {
    private final String name;
    private final String description;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

---
public class Monster implements Cloneable{
    private final Field appearIn;
    private final String name;
    private final int exp;
    private final int hp;
    private final String species;
    private final int speed;
    private final int power;
    private final int defense;
    private final int size;

@Override
protected Object clone() throws CloneNotSupportedException {
    Object clone = super.clone();
    Monster clonedMonster = (Monster) clone;

    if (this.dropItem != null) {
        clonedMonster.dropItem = (Item) this.dropItem.clone();
    }

    return clone;
  }
...
}
```

### 스프링에서의 프로토타입 패턴?

#### ModelMapper 라이브러리
- 객체의 필드를 그대로 다른 객체로 옮겨줄 때 사용하는 라이브러리
- 자바의 리플렉션을 이용해 구현한 라이브러리이며, 객체의 필드 정보를 복제해준다.
- 명확히 프로토타입 패턴이라고 말할 수는 없지만, 비슷한 역할을 한다.

### 프로토타입 패턴의 장점
- 복잡한 객체를 만드는 과정을 clone() 메서드 안에 숨길 수 있다.
- 기존 객체를 복제함으로써 코드의 양과 새로운 객체를 생성하는 처리 로직 수행 과정을 줄인다.
- 객체를 상탯값만 변경해서 사용하는 측면에서, new 키워드를 이용해 인스턴스화 작업을 하는 것보다 복제를 통해서 대량의 객체를 생산하는 게 더욱 효율적이다.
- 기존의 객체 생성 방법을 몰라도 복제해서 사용하면 실체 객체의 구체적인 형식을 몰라도 객체를 생성할 수 있다.

### 프로토타입 패턴의 단점
- 객체를 복제하는 과정 자체가 복잡할 수 있다. 순환 참조가 있는 경우 그렇다.
- 간단한 deep copy 문제에서도 여러 코드를 추가해줘야 했듯이, clone() 메서드를 구현하는 과정 자체가 매우 복잡해질 수 있다.



##### reference
https://jake-seo-dev.tistory.com/378
