# 🌳복합체 패턴(Composite Pattern)

### 1. 개념
- 객체들을 트리 구조들로 구성한 후, 이러한 구조들과 개별 객체들처럼 작업할 수 있도록 하는 구조 패턴
- 복합 객체(Composite)와 단일 객체(Leaf)를 동일한 컴포넌트로 취급하여, 클라이언트에게 이 둘을 구분하지 않고 동일한 인터페이스를 사용하도록 하는 구조 패턴
- 단, `핵심 모델이 계층적 트리로 표현`될 수 있을 때 사용

![image](https://github.com/jooh9992/CodingTest/assets/54580802/a5d8c8ec-443f-4385-96e4-acf98db61c23)


### 2. 예시
- 택배 시스템에서 제품들과 상자를 생각하면 됨, 상자에는 여러개의 제품들과 제품을 담은 상자들이 포함될 수 있음
- 파일 시스템 구조에서 폴더 안에 파일이 있을 수 있고, 파일을 담은 또 다른 폴더가 있을 수 있음. 이때, 폴더는 Composite 객체라 불리고, 파일은 단일 객체이고 자식이 없어 Leaf 객체라고 불림
- <U>`복합체 패턴은 이 폴더와 파일을 동일한 타입으로 취급하여 구현을 단순화 시키는 것이 목적`</U>

### 3. 구조
![image](https://github.com/jooh9992/CodingTest/assets/54580802/7e14f534-e52b-4ae2-83e7-5f25da83b2a2)
출처: https://inpa.tistory.com/entry/GOF-💠-복합체Composite-패턴-완벽-마스터하기

    - Component : Leaf와 Composite를 묶는 공통적인 상위 인터페이스

    - Composite : 복합 객체로서, Leaf 역할이나 Composite 역할을 넣어 관리하는 역할을 함, 자녀들의 구상 클래스들을 알지 못하며, 컴포넌트 인터페이스를 통해서만 모든 하위 요소들과 함께 작동함, 요청을 전달받으면 컨테이너는 작업을 하위 요소들에 위임하고 중간 결과들을 처리한 다음 최종 결과들을 클라이언트에 반환함

    - Leaf : 단일 객체로서, 단순하게 내용물을 표시하는 역할(대부분의 실제 작업들을 수행)

    - Client : Component를 참조하여 단일 / 복합 객체를 하나의 객체로써 다룸

### 4. Java 로 패턴 이해하기
- interface Component
    
    ```java
    public interface Component {
        void operation();
    }
    ``` 

- Class Leaf
    
    ```java
    public class Leaf implements Component{

        @Override
        public void operation() {
            System.out.println(this + " 호출"); //단일 객체 호출
        }
    }
    ``` 

- Class Composite
    
    ```java
    import java.util.ArrayList;
    import java.util.List;

    public class Composite implements Component{

        // Leaf 와 Composite 객체 모두를 저장하여 관리하는 내부 리스트
        List<Component> components = new ArrayList<>();

        public void add(Component c) {
            components.add(c); // 리스트 추가
        }

        public void remove(Component c) {
            components.remove(c); // 리스트 삭제
        }

        @Override
        public void operation() {
            System.out.println(this + " 호출");

            //내부 리스트를 순회하여, 단일 Leaf이면 값을 출력하고,
            //또 다른 서브 복합 객체이면, 다시 그 내부를 순회하는 재귀 함수 동작이 됨
            for(Component component : components){
                component.operation(); //재귀
            }
        }

        public List<Component> getChild(){
            return components; //내부 리스트에 저장된 하위 객체 반환
        }
    }
    ```

- Class Client
    
    ```java
    public class Client {
        public static void main(String[] args) {
            // 1. 최상위 복합체 생성
            Composite composite1 = new Composite();

            // 2. 최상위 복합체에 저장할 Leaf와 또다른 서브 복합체 생성
            Leaf leaf1 = new Leaf();
            Composite composite2 = new Composite();

            // 3. 최상위 복합체에 개체들을 등록
            composite1.add(leaf1);
            composite1.add(composite2);

            // 4. 서브 복합체에 저장할 Leaf 생성
            Leaf leaf2 = new Leaf();
            Leaf leaf3 = new Leaf();
            Leaf leaf4 = new Leaf();

            // 5. 서브 복합체에 개체들을 등록
            composite2.add(leaf2);
            composite2.add(leaf3);
            composite2.add(leaf4);

            // 6. 최상위 복합체의 모든 자식 노드들을 출력
            composite1.operation();
        }
    }
    ```
- 결과

    ![image](https://github.com/jooh9992/CodingTest/assets/54580802/cc011325-97cd-4a4b-ae31-c25dbe14a257)


### 5. 장단점
1. 장점
- 다형성과 재귀를 사용자가 유리하게 사용해 복잡한 트리 구조를 더 편리하게 작업할 수 있음
- 개방/폐쇄 원칙. 객체 트리와 작동하는 기존 코드를 훼손하지 않고 앱에 새로운 요소 유형들을 도입할 수 있음

2. 단점
- 기능이 너무 다른 클래스들에는 공통 인터페이스를 제공하기 어려울 수 있으며, 어떤 경우에는 컴포넌트 인터페이스를 과도하게 일반화해 이해하기 어렵게 만들 수 있음
