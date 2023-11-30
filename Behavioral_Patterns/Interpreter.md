# 🧐 인터프리터 패턴 (Interpreter Pattern)

### 1. 개념
- CS에서 인터프리터란, 소스코드를 기계언어로 변환하는 프로그램을 의미
- 간단한 언어의 문법을 정의하고 해석하는 패턴 → ex) 정규표현식
- 언어가 주어지면 해당 표현을 사용하여 언어로 문장을 해석하는 인터프리터를 사용하여 문법 표현을 정의하는 방법

### 2. 구조
![image](https://github.com/jooh9992/CodingTest/assets/54580802/fa250f6f-e77f-46f5-8cab-0a41521af69c)
- Context : Expression 에서 사용하는 모든 데이터들이 저장되어있는 공간
- Expression : 일련의 규칙을 계산하여 결과 값을 반환
- TerminalExpression : 종료가 가능한 `Expression`, 계산된 결과를 반환 (종료를 포함 - 더이상 다른 문자로 치환될 수 없는 종점 문자임을 의미)
- NonTerminalExpression : 다른 `Expression` 을 참조하는 `Expression`

### 3. Java로 패턴 이해하기

```java
public interface Expression {
    boolean interprete(String context);
}

public class MinusExpression implements Expression {
    String data = "-";

    @Override
    public boolean interprete(String context) {
        if(context.contains(data)){
            return true;
        }
        return false;
    }
}

public class PlusExpression implements Expression {
    String plus = "+";

    @Override
    public boolean interprete(String context) {
        if(context.contains(plus)){
            return true;
        }
        return false;
    }
}
``` 

```java
public class Main {
    public static void main(String[] args) {
        Expression plus = new PlusExpression();
        Expression minus = new MinusExpression();

        System.out.println("plus.interprete(\"1+3\") = " + plus.interprete("1+3"));
        System.out.println("minus.interprete(\"1+3\") = " + minus.interprete("1+3"));
    }
}
``` 
![image](https://github.com/jooh9992/CodingTest/assets/54580802/6171b814-b6e5-42be-8eb9-489516aeb818)

### 4. Java와 Spring에서의 인터프리터
- 자바의 정규표현식
```java
public static void main(String[] args) {
        System.out.println(Pattern.matches(".pr...", "spring"));
        System.out.println(Pattern.matches("[a-z]{6}", "spring"));
        System.out.println(Pattern.matches("seo", "jakeseo"));
        System.out.println(Pattern.matches("\\d", "1")); // one digit
        System.out.println(Pattern.matches("\\D", "a")); // one non-digit
    }
``` 
- 스프링의 expression Language (SpEL)
```java
@Service
public class MyService implements ApplicationRunner {
    @Value("#{2 + 5}") // SpEL
    private String value;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println(value);
    }
}
``` 

### 5. 장단점
- **장점**
    - 각 문법 규칙을 클래스로 표현하기 때문에 언어를 쉽게 구현할 수 있음
    - 문법이 클래스에 의해 표현되기 때문에 언어를 쉽게 변경하거나 확장할 수 있음
    - 클래스 구조에 메소드만 추가하면 프로그램을 해석하는 기본 기능 외에 보기 쉽게 출력하는 기능이나 더 나은 프로그램 확인 기능 같은 새로운 기능을 추가할 수 있음
- **단점**
    - 문법 규칙의 개수가 많아지면 복잡하거나 성능 저하가 발생
