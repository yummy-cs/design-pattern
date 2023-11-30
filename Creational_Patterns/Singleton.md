# 🌍**Singleton Pattern**

---

# ※ 참고 자료

- 무니의 개발 로그 (티스토리) [[디자인 패턴] 싱글톤 패턴(Singleton Pattern) 정리 및 예제 - 생성 패턴 (tistory.com)](https://devmoony.tistory.com/43)
- seongwon97 (벨로그) [싱글톤(Singleton) 패턴이란? (velog.io)](https://velog.io/@seongwon97/%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4%EC%9D%B4%EB%9E%80)
- 코딩팩토리 (티스토리) [[Java] 자바 static의 의미와 사용법 (tistory.com)](https://coding-factory.tistory.com/524)
- Tech Interview [싱글톤 패턴(Singleton pattern) | 👨🏻‍💻 Tech Interview (gyoogle.dev)](https://gyoogle.dev/blog/design-pattern/Singleton%20Pattern.html)
- cjw-awdsd (티스토리) [[디자인 패턴] Singleton Pattern 개념/예제 (tistory.com)](https://cjw-awdsd.tistory.com/42)

---

# 1. 싱글톤 패턴이란?

- **싱글톤패턴이란 객체의 인스턴스를 한개만 생성되게 하는 패턴**
- 생성자가 여러 차례 호출되더라도 **실제로 생성되는 객체는 하나이고,** 
최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다.
- **인스턴스가 필요할 때, 똑같은 인스턴스를 만들지 않고 기존의 인스턴스를 활용하는 것!**

---

# 2. 언제 사용하는가?

- 프로그램 내에서 하나의 객체만 존재해야 할 때
- 프로그램 내에서 여러 부분에서 해당 객체를 공유하며 사용할 때

<aside>
💡 또한 싱글톤 패턴은 주로 공통된 객체를 여러 개 생성해서 사용하는 DBCP(DataBase Connection Pool)와 같은 상황에서 많이 사용된다.

DBCP(DataBase Connection Pool)란?
사용자로부터 요청이 들어올 때마다 데이터베이스 연결을 수립하고, 해제하는 비효율성을 해결하기 위해 **미리 여러 개의 데이터베이스 커넥션을 생성**해 놓고,  **필요할 때마다 꺼내 쓰도록** 하는 것 
DBCP에는 사전에 데이터베이스와 이미 연결이 수립된 **다수의 커넥션들이 존재하고 항상 연결을 열린 상태로 유지**한다. **결과적으로 데이터베이스 연결을 열고, 닫는 비용을 절약한다.**

</aside>

---

# 3. 사용하는 이유

- 한개의 인스턴스만을 고정 메모리 영역에 생성하고 사용하므로, 추후 해당 객체에 접근 할 때 **메모리 낭비를 방지**(원래는 객체를 생성할 때마다 메모리 영역을 할당받아야 한다.)
- 이미 생성된 인스턴스 활용하므로 **속도 측면에서 이점**
- 싱글톤 인스턴스 전역으로 사용하는 인스턴스이기 때문에 다른 여러 클래스에서 데이터를 공유하며 사용할 수 있어 **데이터 접근이 쉽다.**
- 도메인 관점에서 **인스턴스가 한 개만 존재하는 것을 보증**할 수 있다.

---

# 4. 문제점

- **싱글톤 인스턴스**가 너무 많은 일을 하거나 **많은 데이터를 공유시킬 경우에** 다른 인스턴스들 간에 결합도가 높아져 **“개방-폐쇄 원칙”을 위배**하게 된다. (결합도가 높아지면 유지보수가 힘들고 테스트도 원할하게 진행하기 힘들어진다.)
- **멀티 스레드 환경**에서 **동시에 Singleton객체에 접근**하면 **여러개의 객체가 생성**된다. 왜냐하면 모든 쓰레드가 거의 동시에 도착하고 이로 인해 서로가 null 객체를 바라보기 때문에 쓰레드들이 객체를 생성한다. 이것을 **동시성 문제**라 한다.

```java
public class Singleton {
    private static Singleton instance;
    private int msg;

    private Singleton(int msg) {
        try {
            Thread.sleep(100);
            this.msg = msg;
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static Singleton getInstance(int msg) {
        if(instance == null) {
            instance = new Singleton(msg);
        }
        return instance;
    }

    public int getMsg() {
        return msg;
    }
}

class Main {
    public static int num = 1;
    public static void main(String[] args) {
        Runnable run = () -> {
            num++;
            Singleton singleton = Singleton.getInstance(num);
            System.out.println("instance : " + singleton.getMsg());
        };
        for (int i = 0; i < 10; i++) {
            Thread thread = new Thread(run);
            thread.start();
        }
    }
}
//실행마다 달라짐
결과
instance : 6
instance : 5
instance : 4
instance : 8
instance : 2
instance : 11
instance : 3
instance : 7
instance : 9
instance : 10
```

---

# 5. 동시성 문제 해결 방법

<aside>
💡 Eager initailization(이른 초기화, Thread safe)
	
이 방법은 클래스의 static 특징을 이용해 클래스 로더가 초기화하는 시점에 인스턴스를 메모리에 등록하는 방법이다.
아래처럼 static 변수 선언과 동시에 초기화를 해주면 동시성 문제를 해결할 수 있다.

</aside>

```java
class Singleton {
    //선언과 동시에 초기화
    private static Singleton instance = new Singleton(0);
    private int msg;

    private Singleton(int msg) {
        try {
            Thread.sleep(100);
            this.msg = msg;
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    public static Singleton getInstance() {
        return instance;
    }
    public int getMsg() {
        return msg;
    }
}

public class Main {
    public static int num = 1;
    public static void main(String[] args) {
        Runnable run = () -> {
            num++;
            Singleton singleton = Singleton.getInstance();
            System.out.println("instance : " + singleton.getMsg());
        };
        for (int i = 0; i < 10; i++) {
            Thread thread = new Thread(run);
            thread.start();
        }
    }
}

결과
instance : 0
instance : 0
instance : 0
instance : 0
instance : 0
instance : 0
instance : 0
instance : 0
instance : 0
instance : 0
```

<aside>
💡 Lazy Initialization with synchronized(게으른 초기화, 동기화 블럭)

이 방법은 **synchronized** 키워드를 이용한 방식이다.
synchronized는 기본적으로 동기화를 보장해주는 키워드로 **일반 메소드에 synchronized를 선언하면 그 메소드의 인스턴스는 하나의 쓰레드만 접근**할 수 있다. 즉, synchronized가 선언된 메소드의 인스턴스가 2개가 있다면 **각 인스턴스마다 하나의 쓰레드가 접근**할 수 있다.

하지만 static에 synchronized를 선언하면 **클래스의 클래스 객체를 기준으로 동기화가 이루어진다**.
JVM에는 클래스당 하나의 클래스 객체가 적재된다.
즉, static synchronized는 같은 **클래스 객체당 하나의 쓰레드만 접근**할 수 있다.
그래서 아래의 예제의 경우 하나의 쓰레드만 접근할 수 있으므로 동시성 문제를 해결 할 수 있다.

하지만 **synchronized** 문제점은 성능이 좋지 않다.  즉, 인스턴스를 많이 가져오는 작업을 할 경우 성능이슈가 발생할 수 있다.

</aside>

```java
class Singleton {
    private static Singleton instance;
    private int msg;

    private Singleton(int msg) {
        try {
            Thread.sleep(100);
            this.msg = msg;
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    //synchronized 키워드 사용
    public static synchronized Singleton getInstance(int msg) {
        if(instance == null) {
            instance = new Singleton(msg);
        }
        return instance;
    }

    public int getMsg() {
        return msg;
    }
}

public class Main {
    public static int num = 1;
    public static void main(String[] args) {
        Runnable run = () -> {
            num++;
            Singleton singleton = Singleton.getInstance(num);
            System.out.println("instance : " + singleton.getMsg());
        };
        for (int i = 0; i < 10; i++) {
            Thread thread = new Thread(run);
            thread.start();
        }
    }
}

결과
instance : 2
instance : 2
instance : 2
instance : 2
instance : 2
instance : 2
instance : 2
instance : 2
instance : 2
instance : 2
```

<aside>
💡 Lazy Initialization. Double Checking Locking(DCL)

Double Checking Locking은 위에서 본 synchronized 키워드를 사용한 방법의 단점을 보완한 방법이다.
synchronized와는 달리, 먼저 조건문으로 **인스턴스의 존재 여부를 확인**한 다음 **두번째 조건문**에서 **synchronized를 통해 동기화**를 시켜 인스턴스를 생성하는 방법

처음 생성 이후에는 synchronized를 실행하지 않기 때문에 성능저하 완화가 가능함

</aside>

```java
class Singleton {
    private static Singleton instance;
    private int msg;

    private Singleton(int msg) {
        try {
            Thread.sleep(100);
            this.msg = msg;
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static Singleton getInstance(int msg) {
        if (instance == null) {
            //instance가 null인 경우 synchronized 블록 접근
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton(msg);
                }
            }
        }
        return instance;
    }

    public int getMsg() {
        return msg;
    }
}

public class Main {
    public static int num = 1;

    public static void main(String[] args) {
        Runnable run = () -> {
            num++;
            Singleton singleton = Singleton.getInstance(num);
            System.out.println("instance : " + singleton.getMsg());
        };
        for (int i = 0; i < 10; i++) {
            Thread thread = new Thread(run);
            thread.start();
        }
    }
}

결과
instance : 2
instance : 2
instance : 2
instance : 2
instance : 2
instance : 2
instance : 2
instance : 2
instance : 2
instance : 2
```

<aside>
💡 Lazy Initialization. LazyHolder
>> 가장 성능이 좋고 많이 쓰이는 방식

이 방법은 클래스안에 클래스를 두는 holder방법을 이용한 것이다. 
아래 코드에서 중첩 클래스의 instance는 getInstance()가 호출되기 전까지는 초기화 되지않는다. 또한 instance는 static이므로 클래스 로딩 시점에 한번만 호출되고 final을 사용해 다시 값이 할당되지 않도록 함으로써 동시성 문제를 해결할 수 있다.

클래스 안에 선언한 클래스인 holder에서 선언된 인스턴스는 static이기 때문에 클래스 로딩시점에서 한번만 호출된다. 또한 final을 사용해서 다시 값이 할당되지 않도록 만드는 방식을 사용한 것

</aside>

```java
class Singleton {
    private int msg;

    private Singleton(int msg) {
        try {
            Thread.sleep(100);
            this.msg = msg;
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    //static 클래스안에 static 멤버 변수 선언 및 초기화
    private static class Initial {
        private static final Singleton instance = new Singleton(101);
    }
    public static Singleton getInstance() {
        return Initial.instance;
    }

    public int getMsg() {
        return msg;
    }
}

public class Main {
    public static int num = 1;
    public static void main(String[] args) {
        Runnable run = () -> {
            num++;
            Singleton singleton = Singleton.getInstance();
            System.out.println("instance : " + singleton.getMsg());
        };
        for (int i = 0; i < 10; i++) {
            Thread thread = new Thread(run);
            thread.start();
        }
    }
}

결과
instance : 101
instance : 101
instance : 101
instance : 101
instance : 101
instance : 101
instance : 101
instance : 101
instance : 101
instance : 101
```

---

# 6. 결론

- **싱글톤 패턴은 남발하지 않고 꼭 필요한 경우에만 사용할 것**
- **싱글톤 패턴을 사용할 때는 동시성 문제를 생각하자**

---

### **싱글톤 패턴 구현 예제(1)**

```java
public class Singleton {
    // 단 1개만 존재해야 하는 객체의 인스턴스로 static 으로 선언
    private static Singleton instance;

    // private 생성자로 외부에서 객체 생성을 막아야 한다.
    private Singleton() {
    }

    // 외부에서는 getInstance() 로 instance 를 반환
		// 이렇게 만들면 getInstance 메서드 외에는 객체를 생성할 수 없게 됨
    public static Singleton getInstance() {
        // instance 가 null 일 때만 생성
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

### **싱글톤 패턴 구현 예제(2)**

```java
class CarClass {
    private static CarClass car = new CarClass();

    private CarClass() {}

    private static boolean isUse = false;

    // 차 사용 시작
    public static void drive() {
        if(isCarAvailable() == true) {
            isUse = true;
            System.out.println("운전을 시작합니다.");
        } else {
            System.out.println("자동차가 이미 사용중입니다.");
        }
        
    }

    // 차 사용 종료
    public static void parking() {
        isUse = false;
        System.out.println("운전을 종료합니다.");
    }

    public static boolean isCarAvailable() {
        return !isUse;
    }

    // 싱글톤 패턴을 사용하여 CarClass 인스턴스 가져오기
    public static CarClass getInstance() {
        return car;
    }
}

public class Main
{
    public static void main(String[] args) {
        // CarClass 인스턴스를 가져올 때 getInstance() 메서드를 사용해야 합니다.
        // CarClass car = CarClass.getInstance();
        CarClass.drive();   //운전을 시작합니다.
        CarClass.drive();   //자동차가 이미 사용중입니다.
        CarClass.parking(); //운전을 종료합니다.
        CarClass.drive();   //운전을 시작합니다.
        CarClass.parking(); //운전을 종료합니다.
    }
}

결과
//운전을 시작합니다.
//자동차가 이미 사용중입니다.
//운전을 종료합니다.
//운전을 시작합니다.
//운전을 종료합니다.
```

<aside>
💡 자동차를 운전하는 클래스(CarClass)가 있다고 가정해보자
생성자를 private로 만들어서 외부에서는 직접 인스턴스를 생성할 수 없게 하고
static 메소드를 만들어서 이 메서드를 통해서만 객체를 생성하거나 얻을 수 있다.

또한, static 메서드를 사용하므로 인스턴스를 생성하지 않고 바로 사용 가능하며 하나의 객체를 **공유하며 사용한다.**

</aside>

---

### Static 키워드에 대해서

<aside>
💡 Static 키워드를 사용하여 Static변수와 Static메소드를 만들 수 있는데 이 둘을 합쳐 정적 멤버(또는 클래스 멤버) 라고 한다.

정적 멤버는 객체(인스턴스)에 소속된 멤버가 아니라 클래스에 고정된 멤버이다. 

정적 멤버는 클래스 로더가 클래스를 로딩해서 메소드 메모리 영역에 적재할 때 클래스별로 관리한다. 따라서 **클래스의 로딩이 끝나는 즉시 바로 사용할 수 있다.**

</aside>
<img width="1024" alt="image" src="https://github.com/hig0ni/CS-study/assets/111436454/e9ae0096-58e3-4f3a-879b-3e5a0ef808ed">
<aside>
💡 Static 키워드를 통해 생성된 정적멤버들은 Heap영역이 아닌 Static영역에 할당된다. 

Static 영역에 할당된 메모리는 **모든 객체가 공유하여 하나의 멤버를 어디서든지 참조할 수 있는 장점**을 가지지만 Garbage Collector의 관리 영역 밖에 존재하여 **프로그램 종료시까지 메모리가 할당된 채로 존재하므로, Static을 남발하게 되면 시스템 성능에 악영향을 줄 수 있다.**

</aside>

### 정적(Static) 필드 사용 예시

```java
class Number{
    static int class_num = 0; //클래스 필드
    int instance_num = 0; //인스턴스 필드
}

public class Static_ex {
	
    public static void main(String[] args) {
    	Number number1 = new Number(); //첫번째 number
    	Number number2 = new Number(); //두번쨰 number
    	
    	number1.class_num++; //클래스 필드 class_num을 1증가시킴
    	number1.instance_num++; //인스턴스 필드 instance_num을 1증가시킴
    	System.out.println(number2.class_num); //두번째 number의 클래스 필드 출력
    	System.out.println(number2.instance_num); //두번째 number의 인스턴스 필드 출력
    }
}
```

Number 클래스안에 클래스 변수 class_num과 인스턴스 변수 instance_num가 있다.
두개의 Number 인스턴스 number1과 number2를 생성하였다.
먼저 number1에서 class_num과  instance_num을 각각 1씩 증가시킨 후, number2에서 class_num과  instance_num을 출력했을때 결과값은 class_num은 1, instance_num은 0이 출력된다.

이런 현상이 나타나는 이유는, 인스턴스 변수는 인스턴스가 생성될 때마다 생성되므로 인스턴스마다 각기 다른 값을 가지지만 정적 변수는 모든 인스턴스가 하나의 저장공간을 공유하기에 항상 같은 값을 가지기에 나타난다.

### 정적(Static) 메서드 사용 예시

```java
class Name{
    static void print() { //클래스 메소드
	System.out.println("내 이름은 홍길동입니다.");
    }

    void print2() { //인스턴스 메소드
	System.out.println("내 이름은 이순신입니다.");
    }
}

public class Static_ex {
	
    public static void main(String[] args) {
        Name.print(); //인스턴스를 생성하지 않아도 호출이 가능
    	
        Name name = new Name(); //인스턴스 생성
        name.print2(); //인스턴스를 생성하여야만 호출이 가능
    }
}
```

<aside>
💡 정적 메소드는 클래스가 메모리에 올라갈 때 자동적으로 생성된다. 따라서 정적 메소드는 인스턴스를 생성하지 않아도 호출을 할 수 있다.
</aside>

---

# 7. 스프링에서의 싱글톤 패턴

<aside>
💡 스프링 싱글톤은 클래스 자체에 의해서가 아니라, 스프링 컨테이너(Bean Factory/Application Context)에 의해 구현된다.

스프링에서는, 컨테이너 내에서 특정 클래스에 대해 **@Bean**이 정의되면, 스프링 컨테이너는 그 클래스에 대해 **딱 한 개의 인스턴스**를 만든다. 이 공유 인스턴스는 설정 정보에 의해 관리되고, bean이 호출될 때마다 스프링은 생성된 공유 인스턴스를 리턴 시킨다.

</aside>

<aside>
💡 공유인스턴스의 생성시점은 스프링 컨테이너에 따라 달라진다.
빈팩토리는 최초 호출시점에서 인스턴스를 생성하고,
애플리케이션 컨텍스트는 미리 모든 공유인스턴스를 다 초기화해 두었다가 호출될때 바로 리턴시켜준다.

</aside>

<img width="1024" alt="image" src="https://github.com/hig0ni/CS-study/assets/111436454/9dee01c6-15fe-4742-9286-46f7c5da2a62">

bean 관리의 주체인 **스프링 컨테이너**는 그 어떤 호출에도 **단일 공유 인스턴스를 리턴**시키므로 **thread safety도 자동으로 보장**된다.
