# 🖐반복자 패턴(Iterator Pattern)

출처: [💠 반복자(Iterator) 패턴 - 완벽 마스터하기 (tistory.com)](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EB%B0%98%EB%B3%B5%EC%9E%90Iterator-%ED%8C%A8%ED%84%B4-%EC%99%84%EB%B2%BD-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0?category=967431)
### 1. 개념

![1](https://github.com/hig0ni/CS-study/assets/111436454/69888e59-528a-49bd-b3e6-513d6c8707cd)

- 반복자(Iterator) 패턴은 일련의 데이터 집합에 대하여 `하나씩 순회`할 수 있도록 하는 패턴
- 데이터 집합이란 객체들을 그룹으로 묶어 자료의 구조를 취하는 컬렉션을 말한다. 대표적인 컬렉션으로 리스트, 스택, 트리, 그래프, 테이블 등이 있다.
- 반복자 패턴은 컬렉션 구현 방법을 노출시키지 않으면서도 그 집합체 안에 들어있는 모든 항목에 접근할 수 있게 해주는 방법을 제공

### 2. 예시

```java
for(int i=0;i<arr.length;i++)
{
		System.out.println(arr[i]);
}
```

다음과 같이 배열 요소에 접근하기 위한 반복문이 있다. 여기서 arr의 타입이 배열이 아닌 다른 자료 구조로 바뀐다면 이 반복문은 수정이 되어야 한다. Iterator 패턴은 이러한 자료구조에 상관없이 객체 접근 방식을 통일시키고자 할 때 사용된다.

### 3. 구조

![2](https://github.com/hig0ni/CS-study/assets/111436454/efb8f453-437c-43f1-b3f8-0cbb855614cf)

- **Aggregate (인터페이스)** : ConcreateIterator 객체를 반환하는 인터페이스
    - iterator() : ConcreateIterator 객체를 만드는 팩토리 메서드
- **Iterator (인터페이스)** : 집합체 내의 요소들을 순서대로 검색하기 위한 인터페이스
    - hasNext() : 순회할 다음 요소가 있는지 확인 (true / false)
    - next() : 요소를 반환하고 다음 요소를 반환할 준비를 하기 위해 커서를 이동시킴
- **ConcreateAggregate (클래스)** : 여러 요소들이 이루어져 있는 데이터 집합체
- **ConcreateIterator (클래스)** : 반복자 객체
    - ConcreateAggregate가 구현한 메서드로부터 생성되며, ConcreateAggregate 의 컬렉션을 참조하여 순회한다.
    - 어떤 전략으로 순회할지에 대한 로직을 구체화 한다.

### 4. Java 코드로 패턴 이해하기

- 집합체 관련 클래스

```java
import java.util.Arrays;

// 집합체 객체 (컬렉션)
interface Aggregate {
    Iterator iterator();
}

class ConcreteAggregate implements Aggregate {
    Object[] arr; // 데이터 집합 (컬렉션)
    int index = 0;

    public ConcreteAggregate(int size) {
        this.arr = new Object[size];
    }

    public void add(Object o) {
        if(index < arr.length) {
            arr[index] = o;
            index++;
        }
    }

    // 내부 컬렉션을 인자로 넣어 이터레이터 구현체를 클라이언트에 반환
    @Override
    public Iterator iterator() {
        return new ConcreteIterator(arr);
    }

		@Override
		    public String toString() {
		        return Arrays.toString(arr);
		}
}
```

- 반복자 관련 클래스

```java
// 반복체 객체
interface Iterator {
    boolean hasNext();
    Object next();
}

class ConcreteIterator implements Iterator {
    Object[] arr;
    private int nextIndex = 0; // 커서 (for문의 i 변수 역할)

    // 생성자로 순회할 컬렉션을 받아 필드에 참조 시킴
    public ConcreteIterator(Object[] arr) {
        this.arr = arr;
    }

    // 순회할 다음 요소가 있는지 true / false
    @Override
    public boolean hasNext() {
        return nextIndex < arr.length;
    }

    // 다음 요소를 반환하고 커서를 증가시켜 다음 요소를 바라보도록 한다.
    @Override
    public Object next() {
        return arr[nextIndex++];
    }
}
```

- 클래스 흐름

```java
public static void main(String[] args) {
    // 1. 집합체 생성
    ConcreteAggregate aggregate = new ConcreteAggregate(4); // 크기가 4인 객체 집합 생성
    aggregate.add(1); 
    aggregate.add("안녕하세요"); 
    aggregate.add(3.14); 
    aggregate.add(true);

		System.out.println(aggregate); // [1, 안녕하세요, 3.14, true]

    // 2. 집합체에서 이터레이터 객체 반환
    Iterator iter = aggregate.iterator();

    // 3. 이터레이터 내부 커서를 통해 순회
    while(iter.hasNext()) {
        System.out.printf("%s → ", iter.next());
    }

```

![3](https://github.com/hig0ni/CS-study/assets/111436454/ea3180e6-8b61-4da6-8e20-e8b618592e01)

### 5. 장단점과 사용 시기

사용시기

- 컬렉션에 상관없이 객체 접근 순회 방식을 통일하고자 할 때
- 컬렉션의 복잡한 내부 구조를 클라이언트로 부터 숨기고 싶은 경우 (편의 + 보안)
- 데이터 저장 컬렉션 종류가 변경 가능성이 있을 때

장점

- 일관된 이터레이터 인터페이스를 사용해 여러 형태의 `컬렉션에 대해 동일한 순회 방법 제공`
- `컬렉션의 내부 구조 및 순회 방식을 몰라도 된다`. (원래는 각 컬렉션 별로 구조, 순회 방식이 다름)
- 집합체의 구현과 접근하는 처리 부분을 반복자 객체로 분리해 `결합도를 줄일 수 있다.`
- 순회 알고리즘을 별도의 반복자 객체에 추출하여 각 클래스의 책임을 분리하여 `단일 책임 원칙을 준수`한다.
- 데이터 저장 컬렉션 종류가 변경되어도 클라이언트 구현 코드는 손상되지 않아 수정에는 닫혀 있어 `개방-폐쇄 원칙을 준수`한다.

```java
Client에서 iterator로 접근하기 때문에 ConcreteAggregate 내에 수정 사항이 생겨도 
iterator에 문제가 없다면 문제가 발생하지 않는다.
```

![4](https://github.com/hig0ni/CS-study/assets/111436454/0e356baa-8e4c-4014-829f-1ad786f9a142)

단점

- 클래스가 늘어나고 복잡도가 증가한다.

```java
만일 앱이 간단한 컬렉션에서만 작동하는 경우 패턴을 적용하는 것은 복잡도만 증가할 수 있다.
ex) 리스트 면? => 그냥 반복문 사용하는게 더 간단
```

- 구현 방법에 따라 캡슐화를 위배할 수 있다.

### 6. 구체적인  Java 코드 예제

게시판에 글을 올릴 때 **1. 게시글을 최근글**, **2. 작성순**으로 정렬해서 나열 할 수 있게 구현하려고 한다. 즉, 두가지 정렬 전략을 구현해야 되는 것이다.

```java
발표 때 두개가 똑같지 않나하는 의문이 있었는데,
날짜를 Date()를 쓴게 아니라 직접 입력해서
등록한 순서로 조회하는거랑, 게시글 날짜별로 조회하는거랑 차이가 있었음
```

- **iterator 패턴 적용 X**

```java
// 게시글
class Post {
    String title; // 게시글 제목
    LocalDate date; // 게시글 발행일

    public Post(String title, LocalDate date) {
        this.title = title;
        this.date = date;
    }
}

// 게시판
class Board {
    // 게시글을 리스트 집합 객체로 저장 관리
    List<Post> posts = new ArrayList<>();

    public void addPost(String title, LocalDate date) {
        this.posts.add(new Post(title, date));
    }

    public List<Post> getPosts() {
        return posts;
    }
}
```

```java
public static void main(String[] args) {
    // 1. 게시판 생성
    Board board = new Board();

    // 2. 게시판에 게시글을 포스팅
    board.addPost("디자인 패턴 강의 리뷰", LocalDate.of(2020, 8, 30));
    board.addPost("게임 하실분", LocalDate.of(2020, 2, 6));
    board.addPost("이거 어떻게 하나요?", LocalDate.of(2020, 6, 1));
    board.addPost("이거 어떻게 하나요?", LocalDate.of(2021, 12, 22));

    List<Post> posts = board.getPosts();
    
    // 3. 게시글 발행 순서대로 조회하기
    for (int i = 0; i < posts.size(); i++) {
        Post post = posts.get(i);
        System.out.println(post.title + " / " + post.date);
    }

    // 4. 게시글 날짜별로 조회하기
    Collections.sort(posts, (p1, p2) -> p1.date.compareTo(p2.date)); // 집합체를 날짜별로 정렬
    for (int i = 0; i < posts.size(); i++) {
        Post post = posts.get(i);
        System.out.println(post.title + " / " + post.date);
    }
}
```

![5](https://github.com/hig0ni/CS-study/assets/111436454/6441e2e5-dd50-4a81-b15b-1436d1f43d20)

for문을 사용해서 집합체의 요소들을 순회하였다. 그러나 이러한 구성 방식은 Board에 들어간 게시글을 순회할 때, Board가 어떠한 구조로 이루어져 있는지를 클라이언트에 노출된다.

```java
어떻게 노출되는가에 대한 의문?
Board 클래스의 getPosts() 메소드는 List<Post> 객체를 직접 반환하므로 
클라이언트가 게시물 목록에 직접 액세스하고 조작할 수 있다. 
이는 클라이언트가 Board 클래스의 내부 구조에 대한 지식을 갖고 있어
캡슐화를 위반하고 구현 세부 사항을 노출한다는 의미.

List<Post>를 직접 노출함으로써 클라이언트가 예상치 못한 방식으로 목록을 수정할 수 있고
이는 의도하지 않은 게시물 삭제 또는 수정과 같은 잠재적인 문제로 이어질 수 있다. 
```

- **iterator 패턴 적용 O**

```java
// 저장 순서 이터레이터
class ListPostIterator implements Iterator<Post> {
    private Iterator<Post> itr;

    public ListPostIterator(List<Post> posts) {
        this.itr = posts.iterator();
    }

    @Override
    public boolean hasNext() {
        return this.itr.hasNext(); // 자바 내부 이터레이터에 위임해 버림
    }

    @Override
    public Post next() {
        return this.itr.next(); // 자바 내부 이터레이터에 위임해 버림
    }
}

// 날짜 순서 이터레이터
class DatePostIterator implements Iterator<Post> {
    private Iterator<Post> itr;

    public DatePostIterator(List<Post> posts) {
        // 최신 글 목록이 먼저 오도록 정렬
        Collections.sort(posts, (p1, p2) -> p1.date.compareTo(p2.date));
        this.itr = posts.iterator();
    }

    @Override
    public boolean hasNext() {
        return this.itr.hasNext(); // 자바 내부 이터레이터에 위임해 버림
    }

    @Override
    public Post next() {
        return this.itr.next(); // 자바 내부 이터레이터에 위임해 버림
    }
}
```

```java
// 게시글
class Post {
    String title; // 게시글 제목
    LocalDate date; // 게시글 발행일

    public Post(String title, LocalDate date) {
        this.title = title;
        this.date = date;
    }
}

// 게시판
class Board {
    List<Post> posts = new ArrayList<>();

    public void addPost(String title, LocalDate date) {
        this.posts.add(new Post(title, date));
    }

    public List<Post> getPosts() {
        return posts;
    }
		// ListPostIterator 이터레이터 객체 반환
    public Iterator<Post> getListPostIterator() {
        return new ListPostIterator(posts);
    }
		// DatePostIterator 이터레이터 객체 반환
    public Iterator<Post> getDatePostIterator() {
        return new DatePostIterator(posts);
    }
}
```

```java
public static void main(String[] args) {
    // 1. 게시판 생성
    Board board = new Board();

    // 2. 게시판에 게시글을 포스팅
    board.addPost("디자인 패턴 강의 리뷰", LocalDate.of(2020, 8, 30));
    board.addPost("게임 하실분", LocalDate.of(2020, 2, 6));
    board.addPost("이거 어떻게 하나요?", LocalDate.of(2020, 6, 1));
    board.addPost("이거 어떻게 하나요?", LocalDate.of(2021, 12, 22));

    // 게시글 저장 순서대로 조회하기
    print(board.getListPostIterator());

    // 게시글 날짜별로 조회하기
    print(board.getDatePostIterator());
}

public static void print(Iterator<Post> iterator) {
    Iterator<Post> itr = iterator;
    while(itr.hasNext()) {
        Post post = itr.next();
        System.out.println(post.title + " / " + post.date);
    }
}
```

![6](https://github.com/hig0ni/CS-study/assets/111436454/695eb40a-8a71-4539-a70f-34e581f6663a)

이제 클라이언트는 게시글을 순회할 때 Board 내부가 어떤 집합체로 구현(Array, List, Tree, Queue ..등) 되어 있는지 알 수 없게 감추고 전혀 신경 쓸 필요가 없게 되었다. 그리고 순회 전략을 각 객체로 나눔으로써 때에 따라 적절한 이터레이터 객체만 받으면 똑같은 이터레이터 순회 코드로 다양한 순회 전략을 구사할 수 있게 되었다.

- ****집합체 구현에 상관 없이 순회를 표현****

Iterator를 사용하는 또다른 이유는 이터레이터를 사용함으로써 집합체 구현과 분리할 수 있기 때문

```java
Iterator<Post> itr = iterator;
while(itr.hasNext()) {
    Post post = itr.next();
    System.out.println(post.title + " / " + post.date);
}
```

이터레이터 객체를 반환하면 컬렉션을 순회할때 `hasNext()`와 `next()`라는 Iterator의 메소드만을 이용하기 때문에, 집합체인 Board의 내부 구성을 감출수 있게 된다. 즉, 위의 while문은 Board의 구현에 의존하지 않는 것이다.

이 말은 만일 추후에 Board의 집합체를 수정하더라도 Board 클래스가 올바른 Iterator만을 반환해 준다면 클라이언트의 코드(위의 while 루프)는 변경하지 않아도 되게 된다.

### 7. Java에서의 Iterator 패턴

**java.util.Enumeration**: Iterator가 만들어지기 전 사용하던 API로 현재는 사용 안함

****java.util.Iterator****

- hasNext() : 순회할 요소가 있는지 확인
- next() : 현재 커서의 요소를 출력하고 다음으로 커서를 이동
- remove() : Iterator에서 next()로 받았던 해당 엘리먼트를 삭제.
    - 모든 이터레이터에서 다 지원하는 것은 아니다. UnsupportedOperationException()을 던지는 경우가 많다. ⇒ remove()를 쓸 수 없도록 만듬
    - 보통 이 기능은 동시에 다발적으로 같은 명령을 수행해도 안전한 컬렉션에서 제공한다.
- forEachRemaining() : 함수형 인터페이스를 통해 순회 코드를 심플화 해준다

```java
public static void main(String[] args) {
    Set<Integer> aggregate = new TreeSet<>(List.of(1,2,3,4,5));

    // 이터레이터의 while 문 코드를 for문으로 축약할 수 도 있다.
    for (Iterator<Integer> i = aggregate.iterator(); i.hasNext();) {
        System.out.println(i.next());
    }

    // forEachRemaining()
    aggregate.iterator().forEachRemaining(System.out::println);
}
```
