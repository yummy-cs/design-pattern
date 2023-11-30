# 🔎 행동 패턴(Behavioral Patterns)
- 객체의 행위를 조직, 관리, 연합과 관련된 패턴
- 한 객체가 수행할 수 없는 작업을 여러 개의 객체로 어떻게 분배하며 객체 사이의 결합도 최소화에 중점을 둠

<br/>

# ⛓️ 책임연쇄 패턴 (Chain of Responsibility)
### 1. 개념
- 클라이언트의 요청에 대한 세세한 처리를 하나의 객체가 몽땅 하는 것이 아닌, 여러개의 처리 객체들로 나누고, 이들을 사슬처럼 연결해 집합 안에서 연쇄적으로 처리하는 행동 패턴
- 이러한 처리 객체들을 핸들러라고 부름, 요청을 받으면 각 핸들러는 요청을 처리할 수 있는지, 없으면 체인의 다음 핸들러로 처리에 대한 책임을 전가함

### 2. 예시
- 고객센터에 전화를 걸었을 때를 예시로 들면, 첫번째로 자동 응답기 음성 로봇이 응답하고 두번째로 상담사, 세번째로 엔지니어와 통화하고 문제가 해결되어 통화를 종료함
- 여기서 핸들러는 자동 응답기, 상담사, 엔지니어가 됨

### 3. 구조
![image](https://github.com/jooh9992/CodingTest/assets/54580802/2dc417c6-977d-436f-b14b-3e9ca6e5bf6b)

[출처: Chain Of Responsibility 패턴 - 완벽 마스터하기 ](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-Chain-Of-Responsibility-%ED%8C%A8%ED%84%B4-%EC%99%84%EB%B2%BD-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0)
- Handler : 요청을 수신하고 처리 객체들의 집합을 정의하는 인터페이스
- ConcreteHandler: 요청을 처리하는 실제 처리 객체
    - 자신이 처리할 수 없는 요구가 나오면 바라보고 있는 다음 체인의 핸들러에게 요청을 떠넘긴다.
- Client: 요청을 Handler 전달함

### 4. Java 로 패턴 이해하기
출처: https://www.youtube.com/watch?v=FAHEWQD6EVE
- **책임 연쇄 패턴 적용 전**
    
    ```java
    class UrlParser {
        public static void run(String url) {
            // protocol 파싱
            int index = url.indexOf("://");
            if (index != -1) {
                System.out.println("PROTOCOL : " + url.substring(0, index));
            } else {
                System.out.println("NO PROTOCOL");
            }

            // domain 파싱
            int startIndex = url.indexOf("://");
            int lastIndex = url.lastIndexOf(":");

            System.out.print("DOMAIN : ");
            if (startIndex == -1) {
                if (lastIndex == -1) {
                    System.out.println(url);
                } else {
                    System.out.println(url.substring(0, lastIndex));
                }
            } else if (startIndex != lastIndex) {
                System.out.println(url.substring(startIndex + 3, lastIndex));
            } else {
                System.out.println(url.substring(startIndex + 3));
            }

            // port 파싱
            int index2 = url.lastIndexOf(":");
            if (index2 != -1) {
                String strPort = url.substring(index2 + 1);
                try {
                    int port = Integer.parseInt((strPort));
                    System.out.println("PORT : " + port);
                } catch (NumberFormatException e) {
                    e.printStackTrace();
                }
            }
        }
    }
    ``` 
        
    ```java
    class Client {
        public static void main(String[] args) {
            String url1 = "http://www.youtube.com:80";
            System.out.println("INPUT: " + url1);
            UrlParser.run(url1);

            String url2 = "http://localhost:8080";
            System.out.println("INPUT: " + url2);
            UrlParser.run(url2);
        }
    }
    ``` 

    - 결과

    ![image](https://github.com/jooh9992/CodingTest/assets/54580802/072825f7-1583-44d2-b7df-a17643a520a4)

- **책임 연쇄 패턴 적용 후**
![image](https://github.com/jooh9992/CodingTest/assets/54580802/d142bea3-48e9-42f0-a913-cd1535509fe7)

    
    ```java
    // 구체적인 핸들러를 묶는 인터페이스 (추상 클래스)
    abstract class Handler {
        // 다음 체인으로 연결될 핸들러
        protected Handler nextHandler = null;

        // 생성자를 통해 연결시킬 핸들러를 등록
        public Handler setNext(Handler handler) {
            this.nextHandler = handler;
            return handler; // 메서드 체이닝 구성을 위해 인자를 그대로 반환함
        }

        // 자식 핸들러에서 구체화 하는 추상 메서드
        protected abstract void process(String url);

        // 핸들러가 요청에 대해 처리하는 메서드 
        public void run(String url) {
            process(url);

            // 만일 핸들러가 연결된게 있다면 다음 핸들러로 책임을 떠넘긴다
            if (nextHandler != null)
                nextHandler.run(url);
        }
    }
    ``` 
        
    ```java
    class ProtocolHandler extends Handler {
        @Override
        protected void process(String url) {
            int index = url.indexOf("://");
            if (index != -1) {
                System.out.println("PROTOCOL : " + url.substring(0, index));
            } else {
                System.out.println("NO PROTOCOL");
            }
        }
    }

    class DomianHandler extends Handler {
        @Override
        protected void process(String url) {
            int startIndex = url.indexOf("://");
            int lastIndex = url.lastIndexOf(":");

            System.out.print("DOMAIN : ");
            if (startIndex == -1) {
                if (lastIndex == -1) {
                    System.out.println(url);
                } else {
                    System.out.println(url.substring(0, lastIndex));
                }
            } else if (startIndex != lastIndex) {
                System.out.println(url.substring(startIndex + 3, lastIndex));
            } else {
                System.out.println(url.substring(startIndex + 3));
            }
        }
    }

    class PortHandler extends Handler {
        @Override
        protected void process(String url) {
            int index = url.lastIndexOf(":");
            if (index != -1) {
                String strPort = url.substring(index + 1);
                try {
                    int port = Integer.parseInt((strPort));
                    System.out.println("PORT : " + port);
                } catch (NumberFormatException e) {
                    e.printStackTrace();
                }
            }
        }
    }
    ``` 

    ```java
    class Client {
        public static void main(String[] args) {
            // 1. 핸들러 생성
            Handler handler1 = new ProtocolHandler();
            Handler handler2 = new DomianHandler();
            Handler handler3 = new PortHandler();

            // 2. 핸들러 연결 설정 (handler1 → handler2 → handler3)
            handler1.setNext(handler2).setNext(handler3);

            // 3. 요청에 대한 처리 연쇄 실행
            String url1 = "http://www.youtube.com:80";
            System.out.println("INPUT: " + url1);
            handler1.run(url1);

            System.out.println();

            String url2 = "http://localhost:8080";
            System.out.println("INPUT: " + url2);
            handler1.run(url2);
        }
    }
    ``` 

    - 결과

    ![image](https://github.com/jooh9992/CodingTest/assets/54580802/7e2d2349-6381-4f2a-a262-f9e3bc283750)


### 5. 사용 시기
- 특정 요청을 2개 이상의 여러 객체에서 판별하고 처리해야 할 때
- 특정 순서로 여러 핸들러를 실행해야 하는 경우
- 프로그램이 다양한 방식과 종류의 요청을 처리할 것으로 예상되지만 정확한 요청 유형과 순서를 미리 알 수 없는 경우

### 6. 장단점
- 장점: 클라이언트는 처리 객체의 체인 집합 내부의 구조를 알 필요가 없음, 요청의 호출자와 수신자를 분리시킬 수 있음, 클라이언트 코드를 변경하지 않고 핸들러를 체인에 동적으로 추가하거나 처리 순서를 변경하거나 삭제할 수 있어 유연함
- 단점: 충분한 디버깅을 거치지 않았을 경우 집합 내부에서 무한 사이클이 발생할 수 있음
