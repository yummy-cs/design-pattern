# Visitor Pattern 이란?

- Visitor 라는 영어 뜻은 말 그대로 방문자라는 의미를 가지고 있으며, 이름에 걸맞게 `방문자와 방문 공간을 분리`하고 `방문 공간에서 방문자를 맞이할 때, 이후의 행동을 방문자에게 위임`한다고 생각하면 된다.
- 코드에 역할에 직접적으로 말하면 `데이터의 구조와 그 구조 마다의 처리를 분리하는 것이라고 생각하면 된다.`
    - 하나의 구조에 여러명의 방문자(처리)를 분리하는 것이다.
- 해당 패턴을 적용하면 `새로운 처리를 추가하고자 할 때, 새로운 방문자를 만들고 데이터 구조는 해당 방문자를 받아들이도록 하면 된다.`
- 이렇게 된다면, Cliet Code 에서는 방문자의 입장이 아니라, 방문자의 입장에서 먼저 생각하게 되어 `처리에 대한 로직을 위해서 구조를 변경하지 않고 새로운 처리를 추가 할 수 있다.`

# Visitor Pattern의 기대효과

- 기능의 확장 즉, 방문자 역할의 Code와 방문지 Code를 추가할 때 `기존 Code를 수정할 필요 없이` `방문자, 방문지 역할의 Code를 추가`하면 되기 때문에 `OCP(개방 폐쇄 원칙)을 준수`할 수 있다.
- 또한 `방문자의 입장`에서는 `하나의 처리에 대한 책임`을 하기 때문에 `SRP(단일 책임 원칙)을 준수`할 수 있다.
- 마찬가지로 `방문지의 입장`에서도 자신의 `방문 방식` 과 `방문 승인 여부` 만 책임지기 때문에 SRP를 준수한다.

# Visitor Patter 구조

- 구조도
    
    <img width="547" alt="image" src="https://github.com/kkkwp/CS-study/assets/97209766/a714ceb4-6d3a-4d22-b182-1aef56b2cc93">
    
    - Visitor
        - 방문자 클래스의 인터페이스.
        - `visit(Element)` 을 공용 인터페이스로 쓴다.
        - `Element` 는 방문 공간이다.
    
    - Element
        - 방문 공간 클래스의 인터페이스
        - `accept(Vistor)` 를 공용 인터페이스로 부터 받아 구현한다.
        - `Visitor` 는 방문자다.
            - 내부적으로 `accept(Visitor visitor)` 로써 `Vistor.visit(this)` 를 호출하는 것으로 처리에 대한 책임을 위임한다.
        
    - ConcreteVisitor
        - **처리에 대한 담당을 하는 Visitor의 구현체로 SRP를 지키도록 하는 요소이다.
    
    - ConcreteElement(ElementA, ElementB, … )
        - Element 를 구체적으로 구현한 클래스로 데이터 구조에 해당되며, 각 ConcreteVisitor에게 책임을 위임한다.

# 코드 예제

- Vistior Pattern의 좋은 예시로, 현재 디렉토리 하위의 디렉토리 혹은 파일의 경로를 출력하는 프로그램의 예시가 있다.
- 이 상황은 Element(방문지, 데이터 구조) 의 ConcreteElement(File, Directory) 에 해당되는 방문지에 대해서 현재는 Visitor 의 ConcreteVisitor인 ListVisitor가 파일과 디렉토리를 방문하면 `현재 위치의 경로를 출력하는 처리` 만 구현된 상태이다.
- 만약 ListVisitor 는 현재 위치의 경로만 반환하지만, 그 외에도 방문한 방문지에서 삭제하는 역할 을 하는 ConcreteVisitor를 만들 수 있는 상태인 것이다.
- 즉, `방문지 -> Directory, File 의 데이터 구조마다 Visitor의 구현체의 방문을 수락할 수 있는 상태이며, ConcreteVisitor는 각 방문지에서 방문할 때, 무슨 로직을 수행할지 결정하고 있는 상태이다.`

- 예제 코드 다이어그램
    
    <img width="561" alt="image" src="https://github.com/kkkwp/CS-study/assets/97209766/1a4899c1-6280-40e3-a0a0-aaa1756defb2">
    
## Element

- 방문지, 데이터 구조의 추상체 역할을 하는 `Element` 인터페이스로, 방문자(Visitor)에 대한 방문요청에 대한 응답을 가지고 있다.
    
    ```java
    @Component
    public interface Element {
        void accept(Visitor visitor);
    }
    ```
    
## ConcreteElement_1

- 방문지, 데이터 구조 인터페이스의 구현체 역할을 하는 `Directory` 클래스로, 방문 요청(`accept(Visitor visitor)`) 를 구현하고 Visitor에 대한 방문 방식( `add(element)` , `iterator()` )을 구현하고 있다.
    
    ```java
    @Getter
    @ToString
    @Builder
    @AllArgsConstructor
    public class Directory implements Element{
        private final String name;
        private final ArrayList<Element> dir = new ArrayList<>();
    
        public Element add(Element element){
            dir.add(element);
            return this;
        }
    
        public Iterator<Element> iterator(){
            return dir.iterator();
        }
    
        @Override
        public void accept(Visitor visitor) {
            visitor.visit(this);
        }
    }
    ```
    

## ConcreteElement_2

- 방문지, 데이터 구조 인터페이스의 구현체 역할을 하는 File클래스로, 방문 요청(`accept(Visitor visitor)`) 를 구현하고 있으며 별다른 방문 방식은 제안하지 않는다.
    
    ```java
    @Getter
    @ToString
    @Builder
    @AllArgsConstructor
    public class File implements Element {
        private final String name;
    
        @Override
        public void accept(Visitor visitor) {
            visitor.visit(this);
        }
    }
    ```

## Visitor

- 방문자 역할, 데이터에 대한 처리에 대한 추상체 역할을 하는 `Visitor` 인터페이스로, 각 방문지(`데이터 구조`)에 대한 방문 방식(`데이터에 대한 처리`)를 가지고 있다.
    
    ```java
    @Component
    public interface Visitor {
        public void visit(File file);
        public  void visit(Directory directory);
    }
    ``` 

## ConcreteVisitor

- 방문자 역할, 데이터에 대한 처리에 대한 구현체인 `ListVisitor` 클래스로, 각 방문지(`데이터 구조`)에 대한 방문의 처리 로직( `visit(File file)`, `visit(Directory directory)` )을 구현하고 있다.
    
    ```java
    @Builder
    @AllArgsConstructor
    @RequiredArgsConstructor
    public class ListVisitor implements Visitor{
        
        // 현재 경로를 저장
        private String currentDirName;
    
        // 파일 방문했을 때 처리 로직
        @Override
        public void visit(File file) {
            System.out.println(currentDirName + "/" + file.getName());
        }
    
        // 디렉토리 방문했을 때 처리 로직
        // 디렉토리로 갈 때는 지금까지 거쳐온 경로를 쌓기 위해 아래와 같이
        // Directory의 ArrayList<Element> 와 Iterator를 이용해 경로를 저장한다.  
        @Override
        public void visit(Directory directory) {
    
            System.out.println(currentDirName + "/" + directory.getName());
    
            String saveDirName = currentDirName;
    
            currentDirName = currentDirName + "/" + directory.getName();
    
            Iterator<Element> it = directory.iterator();
    
            while(it.hasNext()){
                Element element = it.next();
                element.accept(this);
            }
            currentDirName = saveDirName;
        }
    }
    ```

## Client Code

- 현재 디렉토리 /home 의 하위의 디렉토리에 대해서 방문할 방문지에 해당하는 각 파일과 디렉토리를 생성하고, 생성한 방문지 중 디렉토리라면 하위로 더 접근할 수 있기 때문에 생성된 디렉토리에 `Directory.add(Elment)` 방문했음을 기록한다.
- 또한 현재 디렉토리 /home 하위의 경로에 대해 방문을 승인하여 결과를 확인한다.
    
    ```java
    @Service
    @RequiredArgsConstructor
    public class ClientCode {
        public static void main(String[] args) {
            // 현재 디렉토리 => home 의 하위 디렉토리 workspace 경로 찾기
            Directory workSpaceDir = Directory.builder().name("workspace").build();
    
            // /home/workspace 하위의 디렉토리 경로 찾기
            Directory compositeDir = Directory.builder().name("Visitor").build();
            Directory testDir1 = Directory.builder().name("testUser1").build();
            Directory testDir2 = Directory.builder().name("testUser2").build();
    
            // /home/workspace 하위의 디렉토리 경로 방문
            workSpaceDir.add(compositeDir);
            workSpaceDir.add(testDir1);
            workSpaceDir.add(testDir2);
    
            // /home/workspace/Visitor 하위에 존재하는 파일 찾기
            File element = new File("Element.java");
            File entity = new File("Entity.java");
            File file = new File("File.java");
            File directory = new File("Directory.java");
            File visitor = new File("Visitor.java");
            File listVisitor = new File("ListVisitor.java");
            File main = new File("Main.java");
    
            //home/workspace/Visitor 하위에 존재하는 파일 방문
            compositeDir.add(element);
            compositeDir.add(entity);
            compositeDir.add(file);
            compositeDir.add(directory);
            compositeDir.add(visitor);
            compositeDir.add(listVisitor);
            compositeDir.add(main);
    
            // 현재 /home 디렉토리에서 하위의
            // /workspace 아래의 경로 중 방문한 디렉토리와 파일에 대한 열람 기록 처리를 승인
            workSpaceDir.accept(new ListVisitor("/home"));
        }
    }
    ```
    
## 결과

- 결과 출력 → 현재 디렉토리 /home 중 하위의 /workspace 디렉토리 하위의 디렉토리 혹은 파일에 접근한 기록을 출력한다.
    
    <img width="415" alt="image" src="https://github.com/kkkwp/CS-study/assets/97209766/cad360d4-d26c-4edf-9dbc-66dd441d1445">
    
## 정리

- 데이터 구조를 추가하고자 할 때는, 기존의 코드 변경 없이 데이터 구조를 정의하고 접근 방식만 정의하면 된다.
- 기존의 데이터 처리에 새로운 데이터 구조를 접근하고자 할 때는, 새로운 `visit(Element element)` 만 새롭게 추가하면 된다.
    - 하지만 대대적으로 `데이터 구조가 내부 구현 방식 자체가 변경`된다면, `모든 ConcreteVisitor 의 변경이 필요`하다!!!
- 새로운 데이터 처리 방식을 추가하고자 한다면 기존의 코드 변경 없이 새로운 ConcreteVisitor를 정의하면 된다.
- 즉, 데이터를 기준으로 `구조` 와 `처리` 에 대해서 분리하는 디자인 패턴이다.

# Visitor 패턴에서 짚고 넘어갈 만한 개념

- Visitor 인터페이스를 보면, 각 Element에 대해서 `visit(File file) , visit(Directory)` 가 있는데 이는 ConcreteVisitor 객체가 ConcreteElement 객체에 방문할 때 호출되며, 방문한 Element 유형에 맞게 visit 메서드가 호출 된다.
- 또한 Element 인터페이스에 대해서는 Visitor 에 대해 `accept(Visitor visitor)` 가 있는데, 이는 Visitor가 방문하고자 할 때, 호출 된다.
- 이 두 메서드는 Client Code에서 `ConcreteElement.accept(new ConcreteVisitor());` 로 하여 시작되는데, 이 때 Element가 Visitor의 방문을 수락하면, Visitor는 그 방문지에 맞는 처리를 자동으로 하여 아래와 같이 메서드가 호출 된다.
    - `ConcreteElement.accept(Visitor visitor)` → `ConcreteVisitor.visit(Element element)`
- OOP 에서 함수의 호출이 실제로 어떤 코드로 실행될지 결정되는 과정을 `Disptch` 라고 한다.
    - 위와 같이 여러개의 Visitor, Element 들이 동일한 이름으로 갖는 메서드를 호출할 때, 어떤 메서드로 결정되는지의 과정이 2번이기 때문에 `Double Dispatch`가 이뤄진다고 한다.

## Dispatch Of OOP

- OOP 에서 함수의 호출이 실제로 어떤 코드로 실행될지 결정되는 과정을 *`Disptch`* 라고 한다.
- Dispatch 는 아래와 같이 두 유형으로 나뉜다.
    - ***`Static Dispatch`***
        - 정적 디스패치는 컴파일 시점에 메서드 호출이 결정된다.
        - Method Overloading 과 관련되어 있어, 호출될 메서서드를 컴파일러가 선택할 때  이름과 전달되는 인수의 유형을 고려한다.
        - 정적 디스패치는 결국 어떤 메서드를 호출 할 지 컴파일러가 결정한다.
        
        ```java
        class Example {
            void printMessage(String message) {
                System.out.println("Message: " + message);
            }
        
            void printMessage(int number) {
                System.out.println("Number: " + number);
            }
        
            public static void main(String[] args) {
                Example obj = new Example();

                // 정적 디스패치, 문자열 버전의 printMessage 메소드가 호출됨
                obj.printMessage("Hello"); 

                // 정적 디스패치, 정수 버전의 printMessage 메소드가 호출됨
                obj.printMessage(42);     
            }
        }
        ```
        
    - ***`Dynamic Dispatch`***
        - 동적 디스패치는 런타임 시점에 호출될 메서드가 결정된다.
        - 상속과 다형성을 기반으로 동작하는 것이다.
        - Method Overriding 과 관련있어 실제 객체의 유형에 따라 호출될 메서드가 동적으로 선택된다.
        
        ```java
        class Animal {
            void makeSound() {
                System.out.println("Animal makes a sound");
            }
        }
        
        class Dog extends Animal {
            @Override
            void makeSound() {
                System.out.println("Dog barks");
            }
        }
        
        class Cat extends Animal {
            @Override
            void makeSound() {
                System.out.println("Cat meows");
            }
        }
        
        public class Main {
            public static void main(String[] args) {
                Animal myAnimal = new Dog()
                // 동적 디스패치, Dog 클래스의 makeSound 메소드가 호출됨
                myAnimal.makeSound(); 
        
                myAnimal = new Cat();
                // 동적 디스패치, Cat 클래스의 makeSound 메소드가 호출됨
                myAnimal.makeSound(); 
            }
        }
        
        /*
          myAnimal은 Animal 클래스 유형을 가지지만 
          실제로 실행 시점에 Dog 또는 Cat 객체로 초기화된다. 
          따라서 makeSound 메소드 호출은 객체의 유형에 따라 동적으로 변경됩니다.
        */
        ```
