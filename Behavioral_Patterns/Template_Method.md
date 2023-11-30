# Template Method


> 💡 Template이란?
>
> 모양자를 생각하면 된다~
> 
> ![스크린샷 2023-10-17 17 28 09](https://github.com/kkkwp/CS-study/assets/67499154/1555fdca-3115-49a8-9086-87ec62a01750)


부모 클래스에서 뼈대(템플릿)를 결정하고, 자식 클래스에서 그 구체적인 내용을 결정한다.

- 부모 클래스에서 알고리즘의 골격을 정의한다. 부모 클래스의 코드만 봐서는 최종적으로 어떻게 처리되는지 알 수 없다.
- 자식 클래스들이 알고리즘을 실제로 구현한다. 알고리즘의 구조를 변경하지 않고 특정 단계들을 오버라이드(재정의)할 수 있다.

### ✅ **패턴 사용 시기**

- 전체 알고리즘이나 알고리즘 구조는 확장하지 못하고 특정 단계들만 확장할 수 있도록 하고 싶을 때
- 약간의 차이가 있지만 거의 같은 알고리즘들을 포함하는 여러 클래스가 있을 때

### ✅ **패턴 장점**

- 알고리즘의 특정 부분만 재정의하므로 다른 부분에 발생하는 변경에 대해 영향을 덜 받을 수 있다.
- 중복 코드를 부모 클래스로 가져올 수 있다.

### ✅ **패턴 단점**

- 부모 클래스에 의해 제공된 알고리즘 골격에 의해 클라이언트들은 제한될 수 있다.
- 자식 클래스를 통해 디폴트 단계 구현을 억제하여 *리스코프 치환 원칙*을 위반할 수 있다.
- 템플릿 메서드들은 단계들이 많을수록 유지가 더 어려운 경향이 있다.

### ✅ 예시

문자나 문자열을 5번 반복해서 표시하는 간단한 프로그램을 만들어보자!

![스크린샷 2023-10-17 17 43 03](https://github.com/kkkwp/CS-study/assets/67499154/b4436f9e-a75b-4ed7-b192-9e8a3dfc19cc)

`AbstractDisplay` 메서드 display만 구현된 추상 클래스

`CharDisplay` 메서드 open, print, close를 구현하는 클래스

`StringDisplay` 메서드 open, print, close를 구현하는 클래스

---

- AbstractDisplay.java
    
    open, print, close는 추상 메서드이고, display 메서드만 구현되어 있다.
    
    display가 하는 일이 무엇일까? open 메서드를 호출하기, print 메서드를 5회 호출하기, close 메서드를 호출하기를 수행하고 있다.
    
    open, print, close가 각각 하는 일이 무엇일까? 여기서는 알 수 없다! AbstractDisplay를 구현하는 하위 클래스를 확인해 봐야 알 수 있다!
    
    ```java
    public abstract class AbstractDisplay {
    
    	public abstract void open();
    	public abstract void print();
    	public abstract void close();
    
    	public final void display() {
    		open();
    		for (int i = 0; i < 5; i++) {
    			print();
    		}
    		close();
    	}
    }
    ```
    
- CharDisplay.java
    
    생성자에 문자(char)가 전달될 때의 open, print, close 메서드를 구현한다.
    
    ```java
    public class CharDisplay extends AbstractDisplay {
    
    	private char ch;
    
    	public CharDisplay(char ch) {
    		this.ch = ch;
    	}
    
    	@Override
    	public void open() {
    		System.out.print("<<");
    	}
    
    	@Override
    	public void print() {
    		System.out.print(ch);
    	}
    
    	@Override
    	public void close() {
    		System.out.println(">>");
    	}
    
    ```
    
- StringDisplay.java
    
    생성자에 문자열(string)이 전달될 때의 open, print, close 메서드를 구현한다.
    
    ```java
    public class StringDisplay extends AbstractDisplay {
    
    	private String string;
    	private int width;
    
    	public StringDisplay(String string) {
    		this.string = string;
    		this.width = string.length();
    	}
    
    	@Override
    	public void open() {
    		printLine();
    	}
    
    	@Override
    	public void print() {
    		System.out.println("|" + string + "|");
    	}
    
    	@Override
    	public void close() {
    		printLine();
    	}
    
    	private void printLine() {
    		System.out.print("+");
    		for (int i = 0; i < width; i++) {
    			System.out.print("-");
    		}
    		System.out.println("+");
    	}
    }
    ```
    
- Main.java
    
    ```java
    public class Main {
    
    	public static void main(String[] args) {
    
    		AbstractDisplay d1 = new CharDisplay('H');
    		AbstractDisplay d2 = new StringDisplay("Hello, world!");
    
    		d1.display();
    		d2.display();
    	}
    }
    ```
-  결과

   ![스크린샷 2023-10-17 01 33 53](https://github.com/kkkwp/CS-study/assets/67499154/f5ffde81-b85b-4195-9b1d-fb5a212e0481)


### ⚠️ strategy(전략) 패턴과 비교

- 템플릿 메서드는 상속을 기반으로 한다. 자식 클래스들에서 알고리즘의 부분들을 확장하여 변경할 수 있다.
- 전략 패턴은 합성을 기반으로 한다. 객체 행동의 일부분에 대하여 다양한 전략들을 제공하여 변경할 수 있다.

---

- 템플릿 메서드는 클래스 수준에서 작동하므로 정적이다.
- 전략 패턴은 객체 수준에서 작동하므로 런타임에 행동들을 전환할 수 있도록 한다.
