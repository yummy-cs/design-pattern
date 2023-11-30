# 😎 옵저버 패턴 (Observer Pattern)

### 1. 개념
- 옵저버(관찰자)들이 관찰하고 있는 대상자의 상태가 변화가 있을 때마다 대상자는 직접 목록의 각 관찰자들에게 통지하고, 관찰자들은 알림을 받아 조치를 취하는 행동 패턴
- 다른 디자인 패턴들과 다르게 일대다 의존성을 가짐, 주로 분산 이벤트 핸들링 시스템을 구현하는데 사용

### 2. 예시
- 유튜브 채널은 발행자(Subject)가 되고 구독자들은 관찰자가 되는 구조, 유튜버가 영상을 올리면 여러명의 구독자들은 영상이 올라왔다는 알림을 받음
- 우유배달이나 잡지 구독도 비슷

### 3. 구조
![image](https://github.com/jooh9992/CodingTest/assets/54580802/c69fc3a0-eb8e-4b15-8862-360aed8d9948)

[출처: 옵저버(Observer) 패턴 - 완벽 마스터하기 ](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%98%B5%EC%A0%80%EB%B2%84Observer-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90)
- ISubject: 관찰 대상자를 정의하는 인터페이스
- ConcreteSubject: 관찰 당하는 대상자 / 발행자 / 게시자
    - Observer들을 리스트(List, Map, Set ..등)로 모아 합성(compositoin)하여 가지고 있음
- IObserver: 구독자들을 묶는 인터페이스
- Observer: 관찰자 / 구독자 / 알림 수신

### 4. Java 로 패턴 이해하기


```java
import java.util.ArrayList;
import java.util.List;

// 공지사항을 받았을 때 알림을 출력하는 함수
class Observer {
    public String msg;

    public void receive(String msg){
        System.out.println(this.msg + "에서 메시지를 받음 : " + msg);
    }
}

class User1 extends Observer{

    public User1(String msg){
        this.msg = msg;
    }
}

class User2 extends Observer{

    public User2(String msg) {
        this.msg = msg;
    }
}

// 옵저버에 공지사항을 받을 유저를 추가, 삭제, 전파하는 기능을 가진 클래스
class Notice {
    private List<Observer> observers = new ArrayList<Observer>();

    // 옵저버에 추가
    public void attach(Observer observer){
        observers.add(observer);
    }

    // 옵저버에서 제거
    public void detach(Observer observer){
        observers.remove(observer);
    }

    // 옵저버들에게 알림
    public void notifyObservers(String msg){
        for (Observer o:observers) {
            o.receive(msg);
        }
    }
}
``` 

```java
public class Client {
    public static void main(String[] args) {
        Notice notice = new Notice();
        User1 user1 = new User1("유저1");
        User2 user2 = new User2("유저2");

        notice.attach(user1);
        notice.attach(user2);

        String msg = "첫번째 메세지";
        notice.notifyObservers(msg);

        notice.detach(user1); // user1 공지사항 받는 대상에서 제거
        msg = "두번째 메세지";
        notice.notifyObservers(msg);
    }
}
``` 
![image](https://github.com/jooh9992/CodingTest/assets/54580802/290d9e66-fbb3-4844-8317-43243c4a618d)

### 5. 사용 시기
- 앱이 한정된 시간, 특정한 케이스에만 다른 객체를 관찰해야 하는 경우
- 대상 객체의 상태가 변경될 때마다 다른 객체의 동작을 트리거해야 할 때
- MVC 패턴에서도 사용됨

### 6. 장단점
- **장점**
    - Subject의 상태 변경을 주기적으로 조회하지 않고 자동으로 감지할 수 있음
    - 발행자의 코드를 변경하지 않고도 새 구독자 클래스를 도입할 수 있어 개방 폐쇄 원칙을 지킴
- **단점**
    - 옵저버 패턴을 자주 구성하면 구조와 동작을 알아보기 힘들어져 코드 복잡도가 증가
    - 다수의 옵저버 객체를 등록 이후 해지하지 않는다면 메모리 누수가 발생할 수 있음
