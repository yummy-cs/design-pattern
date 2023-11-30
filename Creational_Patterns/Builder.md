# Builder

## 개념
- `복잡한 객체들을 단계별로 생성할 수 있도록 생성 디자인 패턴`
- 생성자에 들어갈 매개 변수를 메서드로 하나하나 받아들이고 마지막에 통합 빌드해서 객체를 생성하는 방식
- 클래스의 선택적 매개변수가 많은 상황에서 유용하게 사용

## 예시
- 집을 만들 때 벽, 바닥, 문, 창문, 지붕등을 이용해 간단한 집을 만들었는데 마당, 난방등 더 추가하고 싶을 때
- 수제 햄버거 주문할 때 빵이나 패티 등 속재료들을 선택하고 싶을 때

## 구조
![image](https://github.com/jooh9992/CodingTest/assets/54580802/d31c8a0e-5cfd-4b39-a755-ca364e8f424a)

    - Builder : 모든 유형의 빌더들에 공통적인 제품 생성 단계를 선언
    - ConcreteBuilder : 생성 단계들의 다양한 구현 제공
    - Product : 결과로 나온 객체
    - Director : Builder를 이용해서 Product를 만드는 클래스

## 구현
- 빌더 클래스 구현하기
```java
class StudentBuilder {
    private int id;
    private String name;
    private String grade;
    private String phoneNumber;

    public StudentBuilder id(int id) {
        this.id = id;
        return this;
    }

    public StudentBuilder name(String name) {
        this.name = name;
        return this;
    }

    public StudentBuilder grade(String grade) {
        this.grade = grade;
        return this;
    }

    public StudentBuilder phoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
        return this;
    }

		public Student build() {
        return new Student(id, name, grade, phoneNumber); // Student 생성자 호출
    }
}
```

- 빌더 클래스 실행하기
```java
public static void main(String[] args) {

    Student student = new StudentBuilder()
                .id(1801585)
                .name("lee")
                .grade("Senior")
                .phoneNumber("010-5555-5555")
                .build();

    System.out.println(student);
}

---------------
결과
Studaen {id='1801585', name=lee, grade=Senior}
```

## 장단점
1. 장점
- 필수 멤버와 선택적 멤버 분리 가능
- 객체를 생성하는 코드를 간결하고 직관적으로 만들 수 있어 객체 생성 코드가 복잡해지는 것을 방지

2. 단점
- 코드 복잡성 증가 - 빌더 패턴을 적용하기 위해 N개의 클래스에 대응하는 N개의 새로운 빌더 클래스를 생성해야 함
- 필드 추가 시 수정 필요 빌더 패턴을 사용하는 경우, 객체에 필드를 추가하거나 삭제할 때 빌더 클래스도 수정해야 함, 이는 빌더 클래스와 객체 클래스 간의 의존성을 높이는 것으로, 유지보수에 어려움을 초래할 수 있음

## Builder 패턴 예시들
- java.lang.StringBuilder의 append()
- java.lang.StreamBuilder의 append()
- java.util.stream.Stream.Builder
- java.nio.ByteBuffer의 put() - CharBuffer, ShortBuffer, IntBuffer, LongBuffer, FloatBuffer, DoubleBuffer
- javax.swing.GroupLayout.Group의 addComponent()
- java.lang.Appendable의 모든 구현
- Spring 에서는 UriComponents 인스턴스를 Uricomponentsbuilder를 통해서 만들 수 있음
- Lombok의 @Builder : 어노테이션만 붙여주면 클래스를 컴파일할 때 자동으로 클래스 내부에 빌더 API 생성
