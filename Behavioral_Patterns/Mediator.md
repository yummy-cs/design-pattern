# 🚕 Mediator

> 💡 Mediator란?
> 
> 조정자, 중개자 → 그룹에 찾아온, 의지할 수 있는 중재자!

곤란한 일이 있거나 그룹 전체로 파급될 사건이 일어나면 중재자에게 알리고, 중재자가 지시한대로 일을 처리한다.

그룹의 각 멤버가 마음대로 다른 멤버와 소통하고 판단하는 것이 아니라, 항상 중재자를 통해서 행동한다.

중재자는 그룹 멤버로부터 올라오는 보고를 바탕으로 전체적으로 판단한 후 각 멤버에게 지시한다.

→ 복잡하게 얽힌 객체간의 통신을 중지하고 mediator에게 정보를 집중시켜 처리를 정리한다.

<br>

### ✅ 통신 경로의 증가

A와 B라는 인스턴스가 2개 있고 서로 통신(메서드를 호출)한다면, 이때의 통신 경로는 A→B, A←B 2가지이다.

인스턴스가 3개이면 A→B, A←B, A→C, A←C, B→C, B←C로 6가지가 된다.

마찬가지로 인스턴스가 4개가 되면 통신 경로는 12개가 되고, 5개가 되면 20개, 6개가 되면 30개로 늘어난다.

Mediator 패턴은 이러한 통신 경로의 가파른 증가를 억제하는 효과가 있다.

<br> 

### ✅ 패턴 사용 시기

- 일부 클래스들이 다른 클래스들과 단단하게 결합하여 변경하기 어려울 때
- 타 컴포넌트들에 너무 의존하기 때문에 다른 프로그램에서 컴포넌트를 재사용할 수 없는 경우
- 몇 가지 기본 행동을 다양한 콘텍스트들에서 재사용하기 위해 수많은 컴포트 자식 클래스들을 만들었을 때

<br> 

### ✅ 패턴 장점

- 프로그램의 다양한 컴포넌트 간의 결합도를 줄일 수 있다.
- 개별 컴포넌트들을 더 쉽게 재사용할 수 있다

<br>

### ✅ 패턴 단점

- 중재자는 전지전능한 객체로 발전할지도 모른다.
    - 전지전능한 객체: 너무 많은 데이터를 가지고 너무 많은 메서드를 필요로 하는 객체

<br> 

### ✅ 예시

guest 로그인일 때

- 사용자 이름과 패스워드가 비활성화되어 있음

![스크린샷 2023-10-10 20 38 57](https://github.com/yummy-cs/design-pattern/assets/67499154/2c7e2fc7-7a9e-4690-a41e-ddb8cf44e400)

사용자 로그인할 때

- username, password 입력 가능
- 사용자 이름에 문자가 하나라도 입력되면 패스워드는 활성화, OK 버튼은 비활성화
- 패스워드에 문자가 입력되면 OK 버튼도 활성화
  
![스크린샷 2023-10-10 20 40 00](https://github.com/yummy-cs/design-pattern/assets/67499154/f8e08884-0a52-467a-81c9-4c4371a48b32)

---

라디오 버튼(게스트 로그인, 사용자 로그인 선택)이나 텍스트 필드(사용자 이름, 패스워드 입력), 버튼(OK, Cancel)은 각각 다른 클래스로 되어 있다.

여기서 문제! ‘사용자 로그인을 선택하면 사용자 이름과 패스워드가 활성화된다. 사용자 이름과 패스워드 양쪽에 문자열이 있으면 OK 버튼을 활성화한다.’라는 코드를 어디에 작성해야 할까? 

라디오 버튼 클래스? 표시 컨트롤을 위한 비슷한 코드가 각 클래스에 제각각 기술되므로 프로그램 작성과 디버깅이 어려워진다. 또한 이메일 같은 새로운 인풋을 받는 경우 수정하기 어렵다.

→ 다수의 객체 사이에서 조정해야할 때 Mediator 패턴이 필요하다. 개별 객체가 서로 통신하는 것이 아니라 중재하고만 통신하도록 하고, 표시 컨트롤 로직을 중재자 안에만 기술한다.

![스크린샷 2023-10-10 21 10 53](https://github.com/yummy-cs/design-pattern/assets/67499154/fa17bc82-5205-4459-9c1a-fc824c43693f)

- Meiator.java
    
    ‘중재자’를 나타내는 인터페이스다. `LoginFrame` 클래스가 이 인터페이스를 구현한다.
    
    - `createColleages` 메서드는 이 중재자가 관리할 멤버인 Colleague를 생성하는 메서드다. 필요한 버튼이나 텍스트 필드 등을 작성한다.
    - `colleagueChanged` 메서드는 중재자에게 ‘상담’을 요청한다. 라디오 버튼이나 텍스트 필드 상태가 변했을 때 이 메서드가 호출된다.
      
    ```java
    public interface Mediator {
    
    	// Colleague 생성
    	void createColleagues();
    
    	// Colleague 상태가 변화했을 때 호출됨
    	void colleagueChanged();
    }
    ```
    
- Colleague.java
    
    중재자에게 상담을 의뢰할 ‘멤버’를 나타내는 인터세이스다. `ColleagueButton`, `ColleagueTextField`, `ColleagueCheckbox`가 이 인터페이스를 구현한다.
    
    - `setMediator` 메서드는 중재자를 구현한 LoginFrame 클래스가 ‘내가 중재자니까 기억해’라는 의미를 담아서 호출하는 메서드다. 나중에 상담이 필요할 때, 즉 colleagueChanged를 호출할 때 이 메서드의 인수인 mediator를 사용한다.
    - `setColleagueEnabled` 메서드는 중재자로부터 내려오는 ‘지시’에 해당한다. 이 메서드는 ‘활성’, ‘비활성’ 상태가 중재자의 판단에 따라 결정됨을 나타낸다.
    
    ```java
    public interface Colleague {
    
    	// Mediator 설정
    	void setMediator(Mediator mediator);
    
    	// Mediator에서 활성/비활성을 지시
    	void setColleagueEnabled(boolean enabled);
    }
    ```
    
- ColleagueButton.java
    
    ```java
    import java.awt.*;
    
    public class ColleagueButton extends Button implements Colleague {
    
    	private Mediator mediator;
    
    	public ColleagueButton(String caption) {
    		super(caption);
    	}
    
    	@Override
    	public void setMediator(Mediator mediator) {
    		this.mediator = mediator;
    	}
    
    	@Override
    	public void setColleagueEnabled(boolean enabled) {
    		setEnabled(enabled);
    	}
    }
    ```
    
- ColleagueTextField.java
    
    ```java
    import java.awt.*;
    import java.awt.event.TextEvent;
    import java.awt.event.TextListener;
    
    public class ColleagueTextField extends TextField implements TextListener, Colleague {
    
    	private Mediator mediator;
    
    	public ColleagueTextField(String text, int columns) {
    		super(text, columns);
    	}
    
    	@Override
    	public void setMediator(Mediator mediator) {
    		this.mediator = mediator;
    	}
    
    	@Override
    	public void setColleagueEnabled(boolean enabled) {
    		setEnabled(enabled);
    		// 활성/비활성에 맞게 배경색 변경
    		setBackground(enabled ? Color.white : Color.lightGray);
    	}
    
    	@Override
    	public void textValueChanged(TextEvent e) {
    		// 문자열이 바뀌면 Mediator에게 알림
    		mediator.colleagueChanged();
    	}
    }
    ```
    
- ColleagueCheckbox.java
    
    ```java
    import java.awt.*;
    import java.awt.event.ItemEvent;
    import java.awt.event.ItemListener;
    
    public class ColleagueCheckbox extends Checkbox implements ItemListener, Colleague {
    
    	private Mediator mediator;
    
    	public ColleagueCheckbox(String caption, CheckboxGroup group, boolean state) {
    		super(caption, group, state);
    	}
    
    	@Override
    	public void setMediator(Mediator mediator) {
    		this.mediator = mediator;
    	}
    
    	@Override
    	public void setColleagueEnabled(boolean enabled) {
    		setEnabled(enabled);
    	}
    
    	@Override
    	public void itemStateChanged(ItemEvent e) {
    		// 상태가 변화하면 Mediator에게 알림
    		mediator.colleagueChanged();
    	}
    }
    ```
    
- LoginFrame.java
    
    ```jsx
    import java.awt.*;
    import java.awt.event.ActionEvent;
    import java.awt.event.ActionListener;
    
    public class LoginFrame extends Frame implements ActionListener, Mediator {
    
    	private ColleagueCheckbox checkGuest;
    	private ColleagueCheckbox checkLogin;
    	private ColleagueTextField textUser;
    	private ColleagueTextField textPass;
    	private ColleagueButton buttonOk;
    	private ColleagueButton buttonCancel;
    
    	// Colleague 생성하고 배치한 후에 표시
    	public LoginFrame(String title) {
    		super(title);
    
    		setBackground(Color.lightGray);
    		setLayout(new GridLayout(4, 2));
    
    		// Colleague 생성
    		createColleagues();
    
    		add(checkGuest);
    		add(checkLogin);
    		add(new Label("Username: "));
    		add(textUser);
    		add(new Label("Password: " ));
    		add(textPass);
    		add(buttonOk);
    		add(buttonCancel);
    
    		// 활성/비활성 초기 설정
    		colleagueChanged();
    
    		pack();
    		setVisible(true);
    	}
    
    	@Override
    	public void createColleagues() {
    		CheckboxGroup group = new CheckboxGroup();
    		checkGuest = new ColleagueCheckbox("Guest", group, true);
    		checkLogin = new ColleagueCheckbox("Login", group, false);
    
    		textUser = new ColleagueTextField("", 10);
    		textPass = new ColleagueTextField("", 10);
    		textPass.setEchoChar('*');
    
    		buttonOk = new ColleagueButton("OK");
    		buttonCancel = new ColleagueButton("Cancel");
    
    		// Mediator 설정
    		checkGuest.setMediator(this);
    		checkLogin.setMediator(this);
    		textUser.setMediator(this);
    		textPass.setMediator(this);
    		buttonOk.setMediator(this);
    		buttonCancel.setMediator(this);
    
    		// Listener 설정
    		checkGuest.addItemListener(checkGuest);
    		checkLogin.addItemListener(checkLogin);
    		textUser.addTextListener(textUser);
    		textPass.addTextListener(textPass);
    		buttonOk.addActionListener(this);
    		buttonCancel.addActionListener(this);
    	}
    
    	// Colleague 상태가 바뀌면 호출됨
    	@Override
    	public void colleagueChanged() {
    		if (checkGuest.getState()) {
    			textUser.setColleagueEnabled(false);
    			textPass.setColleagueEnabled(false);
    			buttonOk.setColleagueEnabled(true);
    		} else {
    			textUser.setColleagueEnabled(true);
    			userpassChanged();
    		}
    	}
    
    	private void userpassChanged() {
    		if (!textUser.getText().isEmpty()) {
    			textPass.setColleagueEnabled(true);
    			if (!textPass.getText().isEmpty()) {
    				buttonOk.setColleagueEnabled(true);
    			} else {
    				buttonOk.setColleagueEnabled(false);
    			}
    		} else {
    			textPass.setColleagueEnabled(false);
    			buttonOk.setColleagueEnabled(false);
    		}
    	}
    
    	@Override
    	public void actionPerformed(ActionEvent e) {
    		System.out.println(e.toString());
    		System.exit(0);
    	}
    }
    ```

- Main.java
    
    ```java
    public class Main {
    
    	public static void main(String[] args) {
    
    		new LoginFrame("Mediator Sample");
    	}
    }
    ```
