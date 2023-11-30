# 🏰퍼사드 패턴(Facade Pattern)

출처: [💠 퍼사드(Facade) 패턴 - 완벽 마스터하기 (tistory.com)](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%ED%8D%BC%EC%82%AC%EB%93%9CFacade-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90)

### 0. Facade란 ?

`Facade`라는 단어는 건축물의 정면을 의미한다. 

건물의 정면은 보통 건축물의 이미지와 건축 의도를 나타내기 때문에 오래 전부터 특별한 디자인을 적용하여 의미를 부여했다.

이처럼 건축물 정면만 봐도 이 건물이 어떤 목적을 하는지 단번에 알수 있다는 특징을 차용하여 명명 지은 것.

### 1. 개념

- 사용하기 복잡한 클래스 라이브러리에 대해 사용하기 편하게 `간편한 인터페이스(API)를 구성하기 위한 구조 패턴`
- 라이브러리의 각 클래스와 메서드들이 어떤 목적의 동작인지 세세하게 이해할 필요 없이, 잘 정리된 메인 API에서 쉽게 이해하고 사용할 수 있게 해줌

### 2. 예시

![1](https://github.com/hig0ni/CS-study/assets/111436454/0e32887a-3247-42c5-9d17-f97ca65fb549)

- 왼쪽이 사용자(고객), 가운데가 퍼사드 패턴을 적용한 API(은행원), 오른쪽이 각 기능별 API(입금, 출금, 잔액 교환 등)
- 사용자(고객)는 각 기능별 API(은행 업무)가 어떻게 동작하는지 몰라도 퍼사드 패턴 API(은행원)을 통해 간단하게 API를 이용할 수 있다.
- 이처럼 복잡하게 얽혀 있는 것을 정리해서 `사용하기 편한 인터페이스를 고객에게 제공하는게 목적`
- 그래서 고객은 복잡한 시스템을 알 필요없이 시스템의 외부에 대해서 단순한 인터페이스를 이용하기만 하면 된다. 퍼사드를 이용하면 자칫 동작의 목적과 같은 중요한 사항을 놓치는 실수를 줄일 수 있다. (왜? 사용하는 사람은 몰라도 되니깐!)

### 3. 구조

![2](https://github.com/hig0ni/CS-study/assets/111436454/12282ddd-0b38-4333-a097-062b48bdf43d)

![3](https://github.com/hig0ni/CS-study/assets/111436454/4625c917-49f3-40b1-92ad-cf0ec94f961b)


```
Facade : 서브시스템 기능을 편리하게 사용할 수 있도록 하기 위해 여러 시스템과 상호 작용하는 복잡한 로직을 재정리해서 높은 레벨의 인터페이스를 구성한다. Facade 역할은 서브 시스템의 많은 역할에 대해 ‘단순한 창구’가 된다. 클라이언트와 서브시스템이 서로 긴밀하게 연결되지 않도록 한다.

Additional Facade : 퍼사드 클래스는 반드시 한개만 존재해야 한다는 규칙같은 건 없다. 연관 되지 않은 기능이 있다면 얼마든지 퍼사드 2세로 분리한다. 이 퍼사드 2세는 다른 퍼사드에서 사용할 수도 있고 클라이언트에서 직접 접근할 수도 있다.

SubSystem(하위 시스템) : 수십 가지 라이브러리 혹은 클래스들

Client : 서브 시스템에 직접 접근하는 대신 Facade를 사용한다
```

- 퍼사드 패턴은 다른 디자인 패턴과는 다르게 `클래스 구조가 정형화 되지 않은 패턴`이다. 반드시 클래스 위치는 어떻고 어떤 형식으로 위임을 해야되고 이런것이 없다. 그냥 퍼사드 클래스를 만들어 적절히 기능 집약화만 해주면 그게 디자인 패턴이 되는 것. (패턴이라기 보단 논리에 가까움)
- 즉, 퍼사드는 복잡한 것을 단순하게 보여주는 것에 초점을 둔다. 클라이언트로 하여금 복잡한 것을 의식하지 않도록 해주는 것.
- 재귀적인 Facade 패턴의 적용
    
   ![6](https://github.com/hig0ni/CS-study/assets/111436454/727d0867-1ed6-419c-9a9e-317a42854358)

    
    **재귀적 Facade**란 위에서 언급한 **Additional Facade** 를 말하는 것이다.
    
    예를들어 다수의 클래스, 다수의 패키지를 포함하고 있는 큰 시스템에 요소 요소 마다 Facade 패턴을 여기 저기 적용하고 다시 그 Facade를 합친 Facade를 만드는 식으로, 퍼사드를 재귀적으로 구성하면 시스템은 보다 편리하게 된다. 이처럼 퍼사드는 한개만 있으라는 법은 없으며 필요에 의하면 얼마든지 늘려 의존할 수 있다.
    
    쉽게 말해서 큰 시스템을 구역 별로 나눠서 Facade 패턴을 적용한다는 것(그럼 여러개의 Facade 패턴이 적용한 API가 생기겠죠?) ⇒ 업무별로 은행원 여러명 만듬(입출금, 보험 등)
    

### 4. Java 로 패턴 이해하기

데이터베이스로부터 어떤 데이터를 조회해서 출력해주는 JDBC 비슷한 자바 패키지가 있다고 하자. 우리는 이 라이브러리를 이용하여 데이터베이스로부터 값을 얻어오고 화면에 데이터를 파싱해서 예쁘게 출력하려는 프로그램을 만들려고 한다.

그런데 이 라이브러리를 이용하는데 있어 데이터베이스를 조회해서 데이터를 가공하기 까지 다음과 같은 규칙이 존재한다고 한다.

1. dbms를 바로 조회하기전에
2. 과거에 조회된 데이터인지를 캐시에서 먼저 조사를 하고
3. 캐시에 데이터가 있다면 이 캐시에 데이터를 가공하고 출력
4. 캐시에 데이터가 없다면 DBMS를 통해서 조회를 하고
5. 조회된 데이터를 가공하고 출력함과 동시에 캐시에 저장한다.

개발자는 위의 사항을 제대로 숙지한 상태에서 프로그래밍을 하여야 한다.

### 퍼사드 패턴을 적용하지 않은 경우

- API

```java
// DBMS에 저장된 데이터를 나타내는 클래스
class Row  {
    private String name;
    private String birthday;
    private String email;

    public Row(String name, String birthday, String email) {
        this.name = name;
        this.birthday = birthday;
        this.email = email;
    }

    public String getName() {
        return name;
    }

    public String getBirthday() {
        return birthday;
    }

    public String getEmail() {
        return email;
    }
}

// 데이터베이스 역할을 하는 클래스
class DBMS {
    private HashMap<String, Row> db = new HashMap<>();

    public void put(String name, Row row) {
        db.put(name, row);
    }

    // 데이터베이스에 쿼리를 날려 결과를 받아오는 메소드
    public Row query(String name) {
        try {
            Thread.sleep(500); // DB 조회 시간을 비유하여 0.5초대기로 구현
        } catch(InterruptedException e) {}

        return db.get(name.toLowerCase());
    }
}

// DBMS에서 조회된 데이터를 임시로 담아두는 클래스 (속도 향상)
class Cache {
    private HashMap<String, Row> cache = new HashMap<>();

    public void put(Row row) {
        cache.put(row.getName(), row);
    }

    public Row get(String name) {
        return cache.get(name);
    }
}

// Row 클래스를 보기좋게 출력하는 클래스
class Message {
    private Row row;

    public Message(Row row) {
        this.row = row;
    }

    public String makeName() {
        return "Name : \"" + row.getName() + "\"";
    }

    public String makeBirthday() {
        return "Birthday : " + row.getBirthday();
    }

    public String makeEmail() {
        return "Email : " + row.getEmail();
    }
}
```

- Client

```java
class Client {
    public static void main(String[] args) {
        // 1. 데이터베이스 생성 & 등록
        DBMS dbms = new DBMS();
        dbms.put("홍길동", new Row("홍길동", "1890-02-14", "honggildong@naver.com"));
        dbms.put("임꺽정", new Row("임꺽정", "1820-11-02", "imgguckjong@naver.com"));
        dbms.put("주몽", new Row("주몽", "710-08-27", "jumong@naver.com"));

        // 2. 캐시 생성
        Cache cache = new Cache();

        // 3. 트랜잭션에 앞서 먼저 캐시에 데이터가 있는지 조회
        String name = "홍길동";
        Row row = cache.get(name);

        // 4. 만약 캐시에 없다면
        if (row == null){
            row = dbms.query(name); // DB에 해당 데이터를 조회해서 row에 저장하고
            if(row != null) {
                cache.put(row); // 캐시에 저장
            }
        }

        // 5. dbms.query(name)에서 조회된 값이 있으면
        if(row != null) {
            Message message = new Message(row);

            System.out.println(message.makeName());
            System.out.println(message.makeBirthday());
            System.out.println(message.makeEmail());
        }
        // 6. 조회된 값이 없으면
        else {
            System.out.println(name + " 가 데이터베이스에 존재하지 않습니다.");
        }
    }
}
```

데이터를 조회하고 출력되기 까지 여러개의 객체가 사용되고 있다. 

이 코드를 처음 보는 사용자는 이해하기 어려울 수 있다.

위의 코드를 활용하려면 API에 있는 각 메서드들의 기능들을 모두 이해해야한다.

당장은 문제가 없을지는 모르겠지만, 나중에 수정과 확장함에 있어 개발자가 위의 수칙들을 까먹고 실수를 하여 서비스에 버그가 생길수 있다.

- 퍼사드 패턴 구조

![4](https://github.com/hig0ni/CS-study/assets/111436454/33ad1ae1-8cf5-477b-9ac9-b7c45f5e2f7b)

### 퍼사드 패턴을 적용한 경우

- API
    
    ```java
    // DBMS에 저장된 데이터를 나타내는 클래스
    class Row  {
        private String name;
        private String birthday;
        private String email;
    
        public Row(String name, String birthday, String email) {
            this.name = name;
            this.birthday = birthday;
            this.email = email;
        }
    
        public String getName() {
            return name;
        }
    
        public String getBirthday() {
            return birthday;
        }
    
        public String getEmail() {
            return email;
        }
    }
    
    // 데이터베이스 역할을 하는 클래스
    class DBMS {
        private HashMap<String, Row> db = new HashMap<>();
    
        public void put(String name, Row row) {
            db.put(name, row);
        }
    
        // 데이터베이스에 쿼리를 날려 결과를 받아오는 메소드
        public Row query(String name) {
            try {
                Thread.sleep(500); // DB 조회 시간을 비유하여 0.5초대기로 구현
            } catch(InterruptedException e) {}
    
            return db.get(name.toLowerCase());
        }
    }
    
    // DBMS에서 조회된 데이터를 임시로 담아두는 클래스 (속도 향상)
    class Cache {
        private HashMap<String, Row> cache = new HashMap<>();
    
        public void put(Row row) {
            cache.put(row.getName(), row);
        }
    
        public Row get(String name) {
            return cache.get(name);
        }
    }
    
    // Row 클래스를 보기좋게 출력하는 클래스
    class Message {
        private Row row;
    
        public Message(Row row) {
            this.row = row;
        }
    
        public String makeName() {
            return "Name : \"" + row.getName() + "\"";
        }
    
        public String makeBirthday() {
            return "Birthday : " + row.getBirthday();
        }
    
        public String makeEmail() {
            return "Email : " + row.getEmail();
        }
    }
    ```
    

- Facade
    
    ```java
    class Facade {
        private DBMS dbms = new DBMS();
        private Cache cache = new Cache();
    
        public void insert() {
            dbms.put("홍길동", new Row("홍길동", "1890-02-14", "honggildong@naver.com"));
            dbms.put("임꺽정", new Row("임꺽정", "1820-11-02", "imgguckjong@naver.com"));
            dbms.put("주몽", new Row("주몽", "710-08-27", "jumong@naver.com"));
        }
    
        public void run(String name) {
            Row row = cache.get(name);
    
            // 1. 만약 캐시에 없다면
            if (row == null){
                row = dbms.query(name); // DB에 해당 데이터를 조회해서 row에 저장하고
                if(row != null) {
                    cache.put(row); // 캐시에 저장
                }
            }
    
            // 2. dbms.query(name)에서 조회된 값이 있으면
            if(row != null) {
                Message message = new Message(row);
    
                System.out.println(message.makeName());
                System.out.println(message.makeBirthday());
                System.out.println(message.makeEmail());
            }
            // 3. 조회된 값이 없으면
            else {
                System.out.println(name + " 가 데이터베이스에 존재하지 않습니다.");
            }
        }
    }
    ```
    
- Client
    
    ```java
    class Client {
        public static void main(String[] args) {
    
            // 1. 퍼사드 객체 생성
            Facade facade = new Facade();
    
            // 2. db 값 insert
            facade.insert();
    
            // 3. 퍼사드로 데이터베이스 & 캐싱 & 메세징 로직을 한번에 조회
            String name = "홍길동";
            facade.run(name);
        }
    }
    ```
    

### 결과

![5](https://github.com/hig0ni/CS-study/assets/111436454/b6e633db-4c7f-48f9-ac06-16ca8bfb967e)


사용자는 내부의 핵심 로직을 몰라도 데이터베이스에 캐싱하고 조회할 수 있게 됨.

### 5. 장단점

장점

- 하위 시스템의 복잡성에서 코드를 분리하여, 외부에서 시스템을 사용하기 쉬움
- 복잡한 코드를 감춤으로써, 클라이언트가 시스템의 코드를 모르더라도 Facade 클래스만 이해하고 사용 가능
- 하위 시스템 간의 의존 관계가 많을 경우 이를 감소시키고, 의존성을 한 곳으로 모을 수 있음

```java
중간에 매개체 역할을 해주는 퍼사드 객체가 있기 때문에 
실제 내부 로직이 어떻게 변경이 되더라도 상관이 없어지므로 의존성이 감소된다.
```

단점

- 퍼사드가 앱의 모든 클래스에 결합된 God Object가 될 수 있음 ⇒ SOLID 원칙중 `단일 책임 원칙`에 위배

```java
여러 개의 책임을 자신의 속성과 행동으로 직접 수행하는 객체를 GOD Object 라고 한다.

GOD Object 는 우리말로 신 객체라는 뜻이다.

하나의 클래스가 많은 책임을 직접 수행하는 모습을 비유적으로 신 같다고 표현한 것이다.
```

- 퍼사드 클래스 자체가 서브시스템에 대한 의존성을 가지게 되어 의존성을 완전히는 피할 수는 없음

대부분의 경우 퍼사드 객체는 하나만 있어도 충분하므로, 퍼사드 객체를 종종 싱글톤으로 구성한다.

퍼사드 패턴은 복잡한 객체를 대신할 대리인을 만든다는 점에서 비슷하지만 사용자가 서브 시스템 내부의 클래스를 직접 사용하는 것을 제한할 수는 없다는 점에서 차이가 있는 것 같다.

프로젝트내에서도 각자 기능별 API를 담당하게 되는데, 솔직히 각자 API 이해하기 힘듬. 이걸 퍼사드 패턴으로 구현한다면 어느 부분을 수정해달라고 요청하기도 편하고 각 기능을 이해하는데 도움이 될 것 같음
