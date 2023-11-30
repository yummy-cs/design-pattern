# ✈️ Flyweight

> 💡 **Flyweight이란?**
> 
> 복싱에서 가장 체중이 가벼운 체급! 이 디자인 패턴은 객체를 ‘가볍게’ 만들기 위한 것이다.

재사용 가능한 객체 인스턴스를 공유시켜 메모리 사용량을 최소화하는 구조 패턴이다. 

간단히 말하면 캐시(Cache) 개념을 코드로 패턴화 한것으로 보면 되는데, **자주 변하는 속성**(extrinsit)과 **변하지 않는 속성**(intrinsit)을 분리하고 변하지 않는 속성을 캐시하여 재사용해 메모리 사용을 줄이는 방식이다. 즉, 동일하거나 유사한 객체들 사이에 가능한 많은 데이터를 서로 공유하여 사용하도록 하여 최적화를 노리는 패턴이다.

예를 들어, JAVA에서 인스턴스를 만들 때 다음과 같이 만들 수 있다.

```java
new Something();
```

이 인스턴스를 유지하기 위해서는 메모리가 확보된다. 인스턴스가 많이 필요하다고 해서 `new`를 잔뜩 사용하면 메모리 사용량이 커진다. 따라서, **인스턴스를 최대한 공유하고, 쓸데없이 `new` 하지 말자!** 이미 만들어진 인스턴스를 이용할 수 있으면 그것을 공유해서 사용하자.


### ✅ intrinsic 와 extrinsic 상태

플라이웨이트 패턴에서 가장 주의 깊게 보아야 할 점이 바로 **Intrinsic와 Extrinsic의 상태를 구분**하는 것이다.

- **intrinsic**란 인스턴스가 어떠한 상황에서도 변하지 않는 정보를 말한다. 그래서 값이 고정되어 있기에 충분히 언제 어디서 공유해도 문제가 없게 된다.
- **extrinsic**이란 인스턴스를 두는 장소나 상황에 따라서 변화하는 정보를 말한다. 그래서 값이 언제 어디서 변화할지 모르기 때문에 이를 캐시해서 공유할수 는 없다.

예를 들어, 폭탄을 피하는 게임이 있다고 하자.

여기서 폭탄 모양이나 색깔 값은 본질적인 폭탄 상태를 나타나기 때문에 캐시하여 여러 곳에 공유할 수 있다. 반면 폭탄 객체의 x, y 좌표 값은 실시간으로 변화하기 때문에 이를 캐시하여 공유할 수는 없다.

따라서 폭탄 피하기 게임을 플라이웨이트 디자인 패턴으로 표현한다면, 폭탄의 형태나 색깔 같은 고정 정보를 포함하고 있는 객체는 `Flyweight`로 구현 된다. 그리고 이 폭탄 객체를 `FlyweightFactory`가 생성하고 캐싱하고 관리를 하는 것이다.


### ✅ 패턴 사용 시기

- 어플리케이션에 의해 생성되는 객체의 수가 많아 저장 비용이 높아질 때
- 생성된 객체가 오래도록 메모리에 상주하며 사용되는 횟수가 많을 때
- 공통적인 인스턴스를 많이 생성하는 로직이 포함되었을 때
- 임베디드와 같이 메모리를 최소한으로 사용해야할 때


### ✅ 패턴 장점

- 애플리케이션에서 사용하는 메모리를 줄일 수 있다.
- 프로그램 속도를 개선할 수 있다.
    
    `new`로 인스턴스화를 하면 데이터가 생성되고 메모리에 적재 되는 시간이 걸리게 된다. 객체를 공유하면 인스턴스를 가져오기만 하면 되기 때문에 메모리 뿐만 아니라 속도도 향상시킬 수 있게 되는 것이다.
    

### ✅ 패턴 단점

- 코드의 복잡도가 증가한다.


### ✅ 예시

`BigChar` 평소처럼 다루면 프로그램이 무거워지기 때문에 공유하는 편이 더 나은 객체다. → flyweight

`BigCharFactory` flyweight를 만드는 공장이다. 이 공장을 사용해서 Flyweight를 만들면 인스턴스가 공유된다.

`BigString` FlyweightFactory를 사용하여 Flyweight를 만들어내고 이용하는 클라이언트이다.

![스크린샷 2023-09-19 00 54 19](https://github.com/kkkwp/CS-study/assets/67499154/1781d84e-1ef7-474d-bd4c-686f41290027)

- big0.txt
    
    큰 문자
    
    ```
    ....######......
    ..##......##....
    ..##......##....
    ..##......##....
    ..##......##....
    ..##......##....
    ....######......
    ................
    ```
    
- BigChar.java
    
    ‘큰 문자’를 나타내는 클래스이다. 
    
    - 생성자에서는 인수로 주어진 문자의 ‘큰 문자’ 버전을 생성한다. 생성된 문자열은 `fontdata` 필드에 저장한다.
    - 파일에서 큰 문자의 텍스트를 읽어 메모리에 저장한 후, `print` 메서드로 표시한다.
    
    큰 문자는 메모리를 많이 소비한다. `BigChar` 인스턴스를 공유하는 방법을 사용해보자!
    
    (아직 이 클래스에서는 Flyweight 패턴의 ‘공유’와 관련된 코드가 등장하지 않는다. 공유를 제어하는 것은 `BigCharFactory` 클래스이다.)
    
    ```java
    import java.io.IOException;
    import java.nio.file.Files;
    import java.nio.file.Path;
    
    public class BigChar {
    
    	// 문자의 이름
    	private char charname;
    	// 큰 문자를 표현하는 문자열
    	private String fontdata;
    
    	// 생성자
    	public BigChar(char charname) {
    		this.charname = charname;
    		try {
    			String filename = "big" + charname + ".txt";
    			StringBuilder sb = new StringBuilder();
    			for (String line : Files.readAllLines(Path.of(filename))) {
    				sb.append(line);
    				sb.append("\n");
    			}
    			this.fontdata = sb.toString();
    		} catch (IOException e) {
    			this.fontdata = charname + "?";
    		}
    	}
    
    	// 큰 문자를 표시
    	public void print() {
    		System.out.print(fontdata);
    	}
    }
    ```
    
- BigCharFactory.java
    
    `BigChar`의 인스턴스를 만든다. 같은 문자에 해당하는 `BigChar` 클래스의 인스턴스가 이미 만들어져 있다면, 기존의 클래스를 사용하고 새 인스턴스를 만들지 않는다.
    
    지금까지 만들었던 모든 인스터스를 `pool`이라는 필드에 저장한다. 원하는 문자에 해당하는 인스턴스를 만들었는지 빠르게 판단할 수 있도록 `java.util.HashMap`을 사용한다.
    
    → `HashMap` 컬렉션을 통해 키(key) 와 모델 객체를 저장하는 캐시 저장소 역할을 한다.
    
    ```java
    import java.util.HashMap;
    import java.util.Map;
    
    public class BigCharFactory {
    
    	// 이미 만든 BigChar 인스턴스를 관리
    	private Map<String, BigChar> pool = new HashMap<>();
    	// singleton 패턴
    	private static BigCharFactory singleton = new BigCharFactory();
    
    	// 생성자
    	private BigCharFactory() {
    	}
    
    	// 유일한 인스턴스를 얻기
    	public static BigCharFactory getInstance() {
    		return singleton;
    	}
    
    	// BigChar 인스턴스 생성(공유)
    	public synchronized BigChar getBigChar(char charname) {
    		BigChar bc = pool.get(String.valueOf(charname));
    		if (bc == null) {
    			// 여기서 BigChar 인스턴스를 생성
    			bc = new BigChar(charname);
    			pool.put(String.valueOf(charname), bc);
    		}
    		return bc;
    	}
    }
    ```
    
- BigString.java
    
    `BigChar`를 모아서 만든 ‘큰 문자열’을 나타내는 클래스이다.
    
    생성자는 new를 사용하지 않고, getBigChar를 사용하여 작성했다.
    
    ```java
    public class BigString {
    
    	// 큰 문자의 배열
    	private BigChar[] bigChars;
    
    	// 생성자
    	public BigString(String string) {
    		BigCharFactory factory = BigCharFactory.getInstance();
    		bigChars = new BigChar[string.length()];
    		for (int i = 0; i < bigChars.length; i++) {
    			bigChars[i] = factory.getBigChar(string.charAt(i));
    		}
    	}
    
    	// 표시
    	public void print() {
    		for (BigChar bc : bigChars) {
    			bc.print();
    		}
    	}
    }
    ```
    
- Main.java
    
    ```java
    public class Main {
    
    	public static void main(String[] args) {
    
    		if (args.length == 0) {
    			System.out.println("Usage: java Main digits");
    			System.out.println("Example: java Main 1212123");
    			System.exit(0);
    		}
    		BigString bs = new BigString(args[0]);
    		bs.print();
    	}
    }
    ```


### ⚠️ 주의: Garbage Collection 처리

> 💡 **GC(Garbage Collection)이란?**
> 
> JVM이 메모리 공간(heap 영역)을 조사하여 사용되지 않는 인스턴스를 해제함으로써 메모리의 빈 영역을 늘리는 것! 즉, 사용하지 않게 된 메모리를 모아서 재사용하는 것을 말한다. 
>
> 가비지 컬렉션을 할 때는 각 인스턴스에 대해 가비지 여부를 판정하는데, 다른 곳에서 참조되고 있는 인스턴스는 ‘사용 중’으로 간주되어 가비지로 판정하지 않는다.

'인스턴스를 관리' 하는 기능을 JAVA에서 구현할 때에는 반드시 **'관리되고 있는 인스턴스는 GC(Garbage Collection)되지 않는다'** 라는 점을 주의해야 한다.

`BigCharFactory`에서는 `pool` 필드로 HashMap을 이용해 `BigChar`의 인스턴스를 관리하고 있다. `pool` 필드로 관리되고 있으므로 더 이상 `BigString` 인스턴스에서 `BigChar` 인스턴스를 참조하지 않더라도 가비지로 간주되지 않는다. 즉, 일단 한 번 만든 `BigChar` 클래스의 인스턴스는 계속 메모리에 남게 된다. 따라서 더이상 `BigChar`의 인스턴스를 생성할 일이 없다면, 반드시 `BigCharFactory`에 남아 있는 `Flyweight Pool`을 비워줄 필요가 있다. 

해결법: 관리되는 인스턴스를 가비지로 만들기 위해서, 인스턴스를 명시적으로 삭제할 수는 없지만 인스턴스에 대한 참조를 없앨 수 있다. 예시에서는 `HashMap`에서 해당 인스턴스를 포함하는 엔트리를 삭제하면 된다.
