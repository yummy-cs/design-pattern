# 🎞 Memento Pattern

## 행동패턴(Behavioral Patterns)이란?

- 객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴.
- 한 객체가 수행할 수 없는 작업을 여러 개의 객체로 어떻게 분배하며 객체 사이의 결합도 최소화에 중점을 둔다.
- 패턴을 주로 클래스에 적용하는지 아니면 객체에 적용하는지에 따라 구분되는 패턴이다.
- `행동 패턴은 객체나 클래스에 대한 패턴을 정의하는 것이 아닌, 그들 간의 교류 방법에 대해 정의하는 것이다.`
- 행동패턴으로 하여 객체 간에 제어 구조보다는 객체들을 어떻게 연결시킬 것인지에 좀더 집중할 수 있다.

---

## Memento

- 단어의 뜻 그대로는 기념품, 유품, 추억거리 등을 의미하고 있다.

## Memento Pattern

- 객체의 상태 정보를 `저장` 하고 사용자의 필요에 의해 `원하는 시점의 데이터를 복원` 하는 것이 가능하도록 하는 디자인 패턴이다.

## Memento Pattern 의 효과

- 상태 저장 및 복원
    - 객체의 상태를 저장하고 나중에 필요한 경우 복원할 수 있다.
    - 이는 객체의 상태가 변경되었을 때 이전 상태로 돌아갈 필요가 있는 경우 유용하다.
- 캡슐화 및 무결성
    - Memento 패턴은 상태 정보를 캡슐화 하므로, 상태 정보에 대한 접근을 제한하고 보호할 수 있으며 이는 객체의 무결성을 유지하는 데 도움이 된다.

## Memento Pattern 의 구조

- Memento Pattern 구조도
    
<img width="460" alt="image" src="https://github.com/kkkwp/CS-study/assets/97209766/5205a18d-f763-491e-ab98-d2b36801c9e6">
    
- `originator`
    - 사용자가 저장할 객체로 `state` 를 `createMemento()` 로 현재 객체의 상태 정보를 저장하고 그 상태를 반환한다.
    - 또한 Memento 객체로 전달된 상태를 복구하기 위해서 `restore(Memmento)` 를 구현한다.
    - 즉, 자신의 상태에 대한 스냅샷을 생성할 수 있고 복원할 수 있게 된다.

- `memento`
    - `originator` 의 `내부 정보` 를 추상화한 클래스로 `care taker(Client Code)`은  `originator` 객체의 디테일한 정보를 직접적으로 가지는 것이 아니라 `memento`타입으로 가진다.
    
- `care taker(Client Code)`
    - 기존의 디자인 패턴의 구조에서 Client 코드와 같은 역할이지만 주로, `memento`의 상태 관리를 하기 때문에 별칭하는 것 같다.
    - `originator` 객체의 현재 내부 상태를 저장하는 Memento 객체를 생성하고 돌려주는 역할을 주로 하며, `originator` 의 정보를 `memento` 타입으로 접근한다.

## Memento Pattern 코드 예제

- red team과 blue team이 점수를 가리는 게임을 제작할 때, 이를 중단하고 다시 실행해도 중지 이전까지 점수는 기록되어야 하는 경우를 생각해보자.,.,.!
- Memento Pattern 적용 이전
    - Client Code 가 일일히 Game의 score에 대해 알고 있어야 한다.
    - 현재 상태이자 `originator` 역할을 하게될 Game 클래스
        
        ```java
        @Getter
        @Setter
        // care taker(Client Code)
        public class Game {
        	private int redTeamScore;
        	private int blueTeamScore;
        }
        ```
        
    
    - `care taker(Client Code)` 역할의 Client 클래스
        
        ```java
        public class Client {
        	public static void main(Stiring[] args) {
        		Game game = new Game();
        		game.setRedTeamScore(10);
        		game.seBlueTeamScore(20);
        		
        		int blueTeamScore = game.getBlueTeamScore();
        		int redTeamScore = game.getRedTeamScore();
        		
        		Game restoredGame = new Game();
        		resotredGame = setBlueTeamScore(blueTeamScore);
        		resotredGame = setRedTeamScore(redTeamScore);
        ```
        
- Memento Pattern 적용 이후
    - `memento` 역할의 GameSave 클래스
        - Memento는 GameSave가 가지고 있는 필드를 모두 가지고 있고, 불변 객체이다.
        - 즉 score 정보들은 한번 생성자를 통해 한번 생성되면 절대 변하지 않는다.
        
        ```java
        @Getter
        public final class GameSave {
        	// final 키워드로 내부 값을 불변하도록, why -> 메멘토는 정확한 상태를 기록해야하기 때문이다.
        	private final int blueTeamScore;
        	private final int redTeamScore;
        
        	public GameSave(int redTeamScore, int blueTeamScore) {
        		this.redTeamScore = redTeamScore;
        		this.blueTeamScore = blueTeamScore;
        	}
        }
        ```
        
    - `Originator` 의 역할 Game 객체의 상태를 GameSave() 를 통해 저장하는 save 메서드와 GameSave로 부터 이전 상태를 되돌릴 수 있는 `restore()` 메서드가 있다.
        
        ```java
        @Getter
        @Setter
        public class Game {
        	private int redTeamSocre;
        	pivate int blueTeamScore;
        
        	// 현재 상태 저장
        	public GameSave save(){
        		return new GameSave(this.redTeamScore, this.blueTeamScore);
        	}
        
        	// 이전상태 복구
        	public void resotre(Game gameSave) {
        		this.redTeamScore = gameSave.getRedSocre();
        		this.blueTeamScore = gameSave.getBlueScore();
        	}
        }
        ```
        
    
    - `memento` 역할의 GameSave 클래스
        
        ```java
        public class Client {
        	public static void main(String[] args) {
        		GAme game = new Game();
        		game.setBlueTeamSocre(10);
        		game.setBlueTeamSocre(20);
        
        		// 게임의 state 정보를 GameSave에 저장한다.
        		GameSave memento = game.save();
        		
        		// game의 정보 수정
        		game.setRedTeamScore(12);
        		game.setBlueTeamScore(22);
        		
        		// game을 memento에 저장된 정보로 resotre;
        		game.resotre(memento);
        }
        ```
        

## Memento Pattern 의 장/단점

- 캡슐화를 지키면서 상태 객체의 상태 스냅샷을 만들 수 있다.
- 객체 상태를 저장하고 또는 복원하는 역할을 Care Taker에게 위임할 수 있다.
- 객체 상태가 바뀌어도 클라이언트 코드는 바뀌지 않는다.
- 많은 정보를 저장하는 Mementor를 자주 생성하는 경우 메모리 사용량에 많은 영향을 줄 수 있다.
