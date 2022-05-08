# 4. Stack & Queue

## Queue

![Queue](https://blog.kakaocdn.net/dn/bhvAPe/btqHlVqf0RY/Y4oCoA4wUkEpvIkU80i43K/img.png)

- 줄을 서는 행위
- 가장 먼저 넣은 데이터를 가장 먼저 꺼내는 구조
- **FIFO**(First-In First-Out) 혹은 LILO(Last-In Last-Out)
- 한 쪽 끝은 front - 삭제 연산만 수행
- 한 쪽 끝은 rear - 삽입 연산만 수행

**용어**

- Enqueue: 큐에 데이터를 넣는 기능
  - `append`(python), `push`(javascript), `add`(java)
  - 시간복잡도: O(1)
- Dequeue: 큐에서 데이터를 꺼내는 기능
  - `pop`(python), `shift`(javascript), `remove`(java)
  - 시간복잡도: O(1)

**용도**

- 멀티 태스킹을 위한 프로세스 스케쥴링 방식 구현
- 넓이 우선 선택(BFS)
- 컴뷰터 버퍼
- JavaScript의 Callback Queue

**구현**

- 삽입 및 삭제가 용이한 LinkedList로 구현
  - Java에서 Queue는 인터페이스이므로 객체 생성이 불가능하므로 LinkedList를 형번환하여 조작

```java
import java.util.LinkedList;
import java.util.Queue;

Queue<Integer> queue = new LinkedList<>(); // 선언
queue.add(1);     // queue에 값 1 추가, 꽉 차있다면 Overflow 에러
queue.add(2);     // queue에 값 2 추가
queue.offer(3);   // queue에 값 3 추가

queue.peek();     // queue의 첫번째 값 참조

queue.poll();     // queue에 첫번째 값을 반환하고 제거 비어있다면 null
queue.remove();   // queue에 첫번째 값 제거
queue.clear();    // queue 초기화
```

<br>

## Stack

![Stack](https://blog.kakaocdn.net/dn/YhtxB/btqHsbZTFED/DhCPI65pmzfsqETjti138k/img.jpg)

- 데이터를 제한적으로 접근할 수 있는 구조
- 한 쪽 끝에서만 자료를 넣거나 뺄 수 있는 구조
- 가장 나중에 쌓은 데이터를 가장 먼저 빼낼 수 있는 데이터 구조
- **LIFO**(Last-In First-Out) 혹은 FILO(First-In Last-Out)

**용어**

- Push: 스택에 데이터를 넣는 기능
- Pop: 스택에서 데이터를 꺼내는 기능

**용도**

- 단순하고 빠른 성능을 위해 사용
- 배열 구조를 활용
- JavaScript의 Call Stack
- 인터럽트 처리, 수식 계산, 서브루틴의 복귀 번지 저장
- 깊이 우선 탐색(DFS), 재귀적 호출
- 계산기, 뒤로 가기
- 후위표기법
- 문자열 반전

**구현**

- 삭제 시, index를 줄이고 초기화만 하면 되는 Array로 구현

```java
import java.util.Stack; //import

Stack<Integer> stack = new Stack<>(); // int형 스택 선언
stack.push(1);     // stack에 값 1 추가, 꽉 차있다면 Overflow 에러
stack.push(2);     // stack에 값 2 추가
stack.push(3);     // stack에 값 3 추가

stack.peek();     // stack의 가장 상단의 값 출력

stack.pop();       // stack에 값 제거, 비어있다면 Underflow 에러, 시간복잡도: O(1)
stack.clear();     // stack의 전체 값 제거 (초기화)

stack.size();      // stack의 크기 출력 : 2

stack.empty();     // stack이 비어있는지 check (비어있다면 true)

stack.contains(1) // stack에 1이 있는지 check (있다면 true)
```

<br>

## 추가

- Stack으로 Queue를 구현하기
- Circular Queue, Priority Queue



> 출처
>
> [[Java\] 자바 Queue 클래스 사용법 & 예제 총정리 - 코딩팩토리](https://coding-factory.tistory.com/602)
>
> [[Java\] 자바 Stack 클래스 사용법 & 예제 총정리 - 코딩팩토리](https://coding-factory.tistory.com/602)