# 프록시 패턴

# Proxy Pattern 이란?

- Proxy Pattern 은 대상 `원본 객체를 대리하여 대신 처리하게 함으로써 로직의 흐름을 제어`하는 행동 패턴이다.
- Proxy의 사전적인 의미는 '대리인'이라는 뜻으로, 누군가에게 어떤 일을 대신 시키는 것을 의미한다.
- 이를 객체 지향 프로그래밍에 접목해보면 클라이언트가 대상 객체를 직접 쓰는게 아니라 중간에 프록시(대리인)을 거쳐서 쓰는 코드 패턴이라고 생각하면 된다.
- 따라서 대상 객체(Subject)의 메소드를 직접 실행하는 것이 아닌, 대상 객체에 접근하기 전에 프록시(Proxy) 객체의 메서드를 접근한 후 추가적인 로직을 처리한 뒤 접근하게 된다.
    
    <img width="539" alt="image" src="https://github.com/kkkwp/CS-study/assets/97209766/3708b7e6-2e61-4e27-8c18-8d8574a7d0cb">
    
# Proxy Pattern 을 사용하는 이유

- 이러한 상황에서 그냥 객체를 이용하면 된다고 생각할 수 있으나 이런 대리 객체를 통한 방식을 사용하는 이유는 대상 클래스가 민감한 정보를 가지고 있거나 인스턴스화 하기에 무겁거나 추가 기능을 가미하고 싶은데, `원본 객체를 수정할수 없는 상황일 때를 극복`하기 위해서 이다.

## Prxoy Pattern 의 효과

- 보안(Security)
    - 프록시는 클라이언트가 작업을 수행할 수 있는 권한이 있는지 확인하고 검사 결과가 긍정적인 경우에만 요청을 대상으로 전달한다.

- 캐싱(Caching)
    - 프록시가 내부 캐시를 유지하여 데이터가 캐시에 아직 존재하지 않는 경우에만 대상에서 작업이 실행되도록 한다.

- 데이터 유효성 검사(Data validation)
    - 프록시가 입력을 대상으로 전달하기 전에 유효성을 검사한다.

- 지연 초기화(Lazy initialization)
    - 대상의 생성 비용이 비싸다면 프록시는 그것을 필요로 할 때까지 연기할 수 있다.

- 로깅(Logging)
    - 프록시는 메소드 호출과 상대 매개 변수를 인터셉트하고 이를 기록한다.

- 원격 객체(Remote objects)
    - 프록시는 원격 위치에 있는 객체를 가져와서 로컬처럼 보이게 할 수 있다.

# Proxy Pattern 구조

- 프록시 패턴 구조도
    
    <img width="628" alt="image" src="https://github.com/kkkwp/CS-study/assets/97209766/1e840230-9cf2-4f78-9193-63f7e94feb89">
    
- `Subject`
    - Proxy와 RealSubject를 하나로 묶는 인터페이스 (다형성)
    - 대상 객체와 프록시 역할을 동일하게 하는 추상 메소드 operation() 를 정의한다.
    - 인터페이스가 있기 때문에 클라이언트는 Proxy 역할과 RealSubject 역할의 차이를 의식할 필요가 없다.

- `RealSubject`
    - 원본 대상 객체

- `Proxy`
    - 대상 객체(RealSubject)를 중계할 대리자 역할
    - 프록시는 대상 객체를 `합성`(composition)한다.
    - 프록시는 대상 객체와 같은 이름의 메서드를 호출하며, 별도의 로직을 수행 할수 있다. (인터페이스 구현 메소드)
    - 프록시는 `흐름 제어만 할 뿐 결과 값을 조작하거나 변경시키면 안 된다`.

- `Client`
    - Subject 인터페이스를 이용하여 프록시 객체를 생성해 이용
    - 클라이언트는 프록시를 중간에 두고 프록시를 통해서 RealSubject와 데이터를 주고 받는다.

# Proxy Pattern 의 종류

- Proxy Pattern 은 단순하며 자주 사용되는 패턴으로 활용 방식이 다양하다, 대표적으로 아래와 같은 프록시 패턴들이 존재한다.

- Subject 역할의 공통된 ISubject 가 존재한다고 가정하고 코드 예시를 살펴보면 된다.
    
    ```java
    interface ISubject {
        void action();
    }
    ```
    
## 1. 기본형 프록시 패턴

- 코드 예시
    - Proxy 역할의 Proxy
        - RealSubject 객체를 합성하여 필드로 가지고 있다.
        
        ```java
        class Proxy implements ISubject {
            private RealSubject subject; // 대상 객체를 composition
        
            Proxy(RealSubject subject) {
                this.subject = subject;
            }
        
            public void action() {
                subject.action(); // 위임
                /* do something */
                System.out.println("프록시 객체 액션 !!");
            }
        }
        ```   
    
    - Client 역할의 Client
        
        ```java
        class Client {
            public static void main(String[] args) {
                ISubject sub = new Proxy(new RealSubject());
                sub.action();
            }
        }
        ```

## 2. 가상 프록시 패턴

- 지연 초기화를 주 목적으로 한 종류의 프록시
- 가끔 필요하지만 항상 메모리에 적재되어 있는 무거운 서비스 객체가 있는 경우에 주로 사용한다.
- 이 구현은 실제 객체의 생성에 많은 자원이 소모 되지만 사용 빈도는 낮을 때 쓰는 방식이다.
- 서비스가 시작될 때 객체를 생성하는 대신에 객체 초기화가 실제로 필요한 시점에 초기화 될 수 있도록 지연할 수 있다.

- 코드 예시
    
    ```java
    class Proxy implements ISubject {
        private RealSubject subject; // 대상 객체를 composition
    
        Proxy() {
        }
    
        public void action() {
        	// 프록시 객체는 실제 요청(action(메소드 호출)이 
    			// 들어 왔을 때 실제 객체를 생성한다.
            if(subject == null){
                subject = new RealSubject();
            }
            subject.action(); // 위임
            /* do something */
            System.out.println("프록시 객체 액션 !!");
        }
    }
    ```
    
    ```java
    class Client {
        public static void main(String[] args) {
            ISubject sub = new Proxy();
            sub.action();
        }
    }
    ```

## 3. 보호 프록시 패턴

- 프록시가 대상 객체에 대한 자원으로의 엑세스 제어(접근 권한을 다룬다)한다.
- 특정 클라이언트만 서비스 객체를 사용할 수 있도록 하는 경우에 사용한다.
- 프록시 객체를 통해 클라이언트의 자격 증명이 기준과 일치하는 경우에만 서비스 객체에 요청을 전달할 수 있게 한다.

- 코드 예시
    
    ```java
    class Proxy implements ISubject {
        private RealSubject subject; // 대상 객체를 composition
        boolean access; // 접근 권한
    
        Proxy(RealSubject subject, boolean access) {
            this.subject = subject;
            this.access = access;
        }
    
        public void action() {
            if(access) {
                subject.action(); // 위임
                /* do something */
                System.out.println("프록시 객체 액션 !!");
            }
        }
    }
    ```
    
    ```java
    class Client {
        public static void main(String[] args) {
            ISubject sub = new Proxy(new RealSubject(), false);
            sub.action();
        }
    }
    ```
    

## 4. 로깅 프록시 패턴

- 대상 객체에 대한 로깅을 추가하려는 경우에 사용한다.
- 프록시는 서비스 메서드를 실행하기 전달하기 전에 로깅을 하는 기능을 추가하여 재정의한다.

- 코드 예시
    
    ```java
    class Proxy implements ISubject {
        private RealSubject subject; // 대상 객체를 composition
    
        Proxy(RealSubject subject {
            this.subject = subject;
        }
    
        public void action() {
            System.out.println("로깅..................");
            
            subject.action(); // 위임
            /* do something */
            System.out.println("프록시 객체 액션 !!");
    
            System.out.println("로깅..................");
        }
    }
    ```
    
    ```java
    class Client {
        public static void main(String[] args) {
            ISubject sub = new Proxy(new RealSubject());
            sub.action();
        }
    }
    ```

## 5. 원격 프록시 패턴

- 프록시 클래스는 로컬에 있고, 대상 객체는 원격 서버에 존재하는 경우에 사용한다.
- 프록시 객체는 네트워크를 통해 클라이언트의 요청을 전달하여 네트워크와 관련된 불필요한 작업들을 처리하고 결과값만 반환한다.
- 클라이언트 입장에선 프록시를 통해 객체를 이용하는 것이니 원격이든 로컬이든 신경 쓸 필요가 없으며, 프록시는 진짜 객체와 통신을 대리하게 된다.

- 그림 예시
    
    <img width="662" alt="image" src="https://github.com/kkkwp/CS-study/assets/97209766/c5c2d914-8af7-4de7-a1e6-3c5c5010652e">
    

## 6. 캐싱 프록시 패턴

- 데이터가 큰 경우 캐싱하여 재사용을 유도하는 곳에서 사용할 수 있도록 하는 것이다.
- 클라이언트 요청의 결과를 캐시하고 이 캐시의 수명 주기를 관리

- HTTP Proxy
    
    <img width="506" alt="image" src="https://github.com/kkkwp/CS-study/assets/97209766/c50b5a05-e487-4cc7-b27c-86085fc043d5">
    
    - HTTP Proxy는 웹서버와 브라우저 사이에서 웹 페이지의 캐싱을 실행하는 소프트웨어이다.
    - 웹 브라우저가 어떤 웹 페이지를 표시할 때 직접 웹 서버에서 그 페이지를 가져오는 것이 아니고, HTTP Proxy가 캐쉬해서 어떤 페이지를 대신해서 취득한다.
    - 만일 최신 정보가 필요하거나 페이지의 유효기간이 지났을 때 웹 서버에 웹 페이지를 가지러 간다.
    - 이를 패턴으로 따져보면, 웹 브라우저가 Client 역할, HTTP Proxy가 Proxy 역할, 그리고 웹 서버가 RealSubcjet 역할을 한다고 보면 된다.
    
# 프록시 패턴의 특징

- 접근을 제어하거가 기능을 추가하고 기존의 특정 객체를 수정할 수 없는 상황에 주로 사용한다.
    - 초기화 지연, 접근 제어, 로깅, 캐싱 등, 기존 객체 동작에 수정 없이 추가하고자 할 때 사용한다.

## 장점

- [개방 폐쇄 원칙(OCP)](https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EC%95%84%EC%A3%BC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-OCP-%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84-%EC%9B%90%EC%B9%99)
    - 기존 대상 객체의 코드를 변경하지 않고 새로운 기능을 추가할 수 있다.

- [단일 책임 원칙(SRP)](https://inpa.tistory.com/entry/OOP-%F0%9F%92%A0-%EC%95%84%EC%A3%BC-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-SRP-%EB%8B%A8%EC%9D%BC-%EC%B1%85%EC%9E%84-%EC%9B%90%EC%B9%99)
    - 대상 객체는 자신의 기능에만 집중 하고, 그 이외 부가 기능을 제공하는 역할을 프록시 객체에 위임하여 다중 책임을 회피 할 수 있다.

- 원래 하려던 기능을 수행하며 그외의 로깅, 인증, 네트워크 통신 등을 수행하는데 유용하다.
- 클라이언트는 객체를 신경쓰지 않고, 서비스 객체를 제어하거나 생명 주기를 관리할 수 있다.
- 사용자 입장에서는 프록시 객체나 실제 객체나 사용법은 유사하므로 사용성에 문제 되지 않는다.

## 단점

- 많은 프록시 클래스를 도입해야 하므로 코드의 복잡도가 증가한다.
    - 예를 들어 여러 클래스에 로깅, 인증, 네트워크 통신 기능 등을 가미 시키고 싶다면, 동일한 코드를 적용함에도 각각의 클래스에 해당되는 프록시 클래스를 만들어서 적용해야 되기 때문에 코드량이 많아지고 중복이 발생 된다.
    - 자바에서는 리플렉션에서 제공하는 동적 프록시(Dynamic Proxy) 기법을 이용해서 해결할 수 있다.

- 프록시 클래스 자체에 들어가는 자원이 많다면 서비스로부터의 응답이 늦어질 수 있다.

# 실무에서 Proxy Pattern

## Dynamic Proxy 를 이용한 Spring Framework 라이브러리

- 프록시 디자인 패턴은 대상 원본 클래스 수만큼 일일히 프록시 클래스를 하나하나 만들어 줘야하는 치명적인 단점이 존재한다, 즉 객체가 100개면 프록시 객체도 100개 만들어줘야 한다는 뜻이다.
- 위의 단점을 보완하기 위해 `컴파일 시점이 아닌 런타임 시점에 프록시 클래스를 만들어 주는 방식으로 JVM에서 지원하는 이 방식`을 [동적 프록시(Dynamic Proxy)](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EB%88%84%EA%B5%AC%EB%82%98-%EC%89%BD%EA%B2%8C-%EB%B0%B0%EC%9A%B0%EB%8A%94-Dynamic-Proxy-%EB%8B%A4%EB%A3%A8%EA%B8%B0)` 기법이라고 불리운다.
- 동적 프록시는 개발자가 직접 일일히 프록시 객체를 생성하는 것이 아닌, 애플리케이션 실행 도중 java.lang.reflect.Proxy 패키지에서 제공해주는 API를 이용하여 동적으로 프록시 인스턴스를 만들어 등록하는 방법으로서, 자바의 [Reflection API](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EB%88%84%EA%B5%AC%EB%82%98-%EC%89%BD%EA%B2%8C-%EB%B0%B0%EC%9A%B0%EB%8A%94-Reflection-API-%EC%82%AC%EC%9A%A9%EB%B2%95)기법을 응용한 연장선의 개념이다.

## Spring AOP

- Aspect 클래스인 MethodExecutionTimeAspect
    
    ```java
    import org.aspectj.lang.JoinPoint;
    import org.aspectj.lang.annotation.After;
    import org.aspectj.lang.annotation.Aspect;
    import org.aspectj.lang.annotation.Before;
    import org.springframework.stereotype.Component;
    
    @Aspect
    @Component
    public class MethodExecutionTimeAspect {
    
        @Before("execution(* com.example.myapp.service.*.*(..))")
        public void beforeMethodExecution(JoinPoint joinPoint) {
            String methodName = joinPoint.getSignature().getName();
            System.out.println("Method " + methodName + " started.");
        }
    
        @After("execution(* com.example.myapp.service.*.*(..))")
        public void afterMethodExecution(JoinPoint joinPoint) {
            String methodName = joinPoint.getSignature().getName();
            System.out.println("Method " + methodName + " finished.");
        }
    }
    ```
    

- 메소드 실행 시간을 로깅할 서비스 클래스 MyService
    
    ```java
    import org.springframework.stereotype.Service;
    
    @Service
    public class MyService {
    
        public void performTask1() {
            // 비즈니스 로직
        }
    
        public void performTask2() {
            // 비즈니스 로직
        }
    }
    ```
    

- Spring 설정
    - Spring 설정 파일에서 Aspect를 등록한다.
    
    ```java
    <!-- XML 설정 파일에서 Aspect를 스캔하도록 지정 -->
    <context:component-scan base-package="com.example.myapp.aspect" />
    
    <!-- AspectJ 지원을 활성화 -->
    <aop:aspectj-autoproxy />
    ```
    
    - `MyService` 클래스의 메소드가 호출되면 Spring AOP를 통해 `MethodExecutionTimeAspect`가 실행됩니다.
    - `실행되는 순간` Bean 으로 등록된 Aspect 클래스 내부에서 Reflection API를 사용하여 메소드 이름을 가져와 로깅하게 되며 이는 Refelction API 에 적용된 Proxy Pattern 의 개념인 Dynamic Proxy 때문에 가능한 것이다.

- Main 에서 실행
    
    ```java
    import org.springframework.context.annotation.AnnotationConfigApplicationContext;
    
    public class MyApp {
    
        public static void main(String[] args) {
            AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
            MyService service = context.getBean(MyService.class);
    
            service.performTask1();
            service.performTask2();
    
            context.close();
        }
    }
    
    ------< 결과 > ----------------
    Method performTask1 started.
    Method performTask1 finished.
    Method performTask2 started.
    Method performTask2 finished.
    ```
    

## Spring JPA

### 지연 로딩

```java
@Entity
public class Order {
    @ManyToOne(fetch = FetchType.LAZY)
    private Customer customer;
    // ...
}
```

- 위의 코드에서 `FetchType.LAZY`로 설정된 경우, `Order` 엔티티를 조회할 때 `Customer` 엔티티는 실제 데이터베이스에서 로드되지 않고 프록시로 대체된다.
- 실제로 `Customer` 데이터가 필요한 시점에서 로딩된다.

### 트랜잭션 관리

```java
@Service
@Transactional
public class ProductService {
    @Autowired
    private ProductRepository productRepository;

    public void updateProductPrice(Long productId, double newPrice) {
        Product product = productRepository.findById(productId).orElse(null);
        if (product != null) {
            product.setPrice(newPrice);
            // 프록시를 통해 트랜잭션 관리
        }
    }
}
```

- Spring Data JPA는 트랜잭션 관리 또한 프록시를 사용한다.
- 예를 들어, 엔티티를 수정하고 트랜잭션을 커밋할 때, 프록시는 엔티티의 변경 사항을 추적하고 해당 변경 사항을 데이터베이스에 반영한다.

### 출처

- [인파님 블로그 - Proxy Pattern](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%ED%94%84%EB%A1%9D%EC%8B%9CProxy-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90)
