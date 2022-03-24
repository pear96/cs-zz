# 2. Linked List

## Linked List란

![LinkedList](https://blog.kakaocdn.net/dn/bqsySc/btqEk1stewE/tnty2kV69c7l45eyUO3Jh0/img.png)

- 양방향 연결 리스트
- 각 노드가 데이터가 노드로 구성되어 연결되어 있는 방식
- 노드의 포인터가 이전 노드와 다음 노드와의 연결을 담당
- 데이터의 추가/삭제가 많은 경우 사용 (탐색 또는 정렬을 많이 하는 경우 배열 사용)
- 장점
  - 객체 추가, 삭제 시 앞뒤 링크만 변경되고 나머지 링크는 변경되지 않음
  - 전체의 인덱스가 한 칸씩 뒤로 밀리거나 당겨지는 일이 없기 때문에 데이터의 추가와 삭제가 용이
- 단점
  - 인덱스가 없기 때문에 특정 요소에 접근 속도가 느림

<br>

### 선언

- 초기의 크기를 미리 설정 x
- Casting: 내부에 임의의 값을 넣어 사용, 잘못된 타입으로 캐스팅한 경우 에러 발생
  - `Arrays.asList`: 일반 배열을 `ArrayList`로 반환

- Generics: 자료형의 안전성을 위해 객체 타입 선언
  - `int`는 기본자료형이므로 불가능, `int`를 객체화시킨 wrapper 클래스인 `Integer` 사용


```java
import java.util.LinkedList;
import java.util.Arrays;

LinkedList list = new LinkedList(); //타입 미설정 Object로 선언된다.

LinkedList<Student> members = new LinkedList<Student>(); //타입설정 Student객체만 사용가능
LinkedList<Integer> num = new LinkedList<Integer>(); //타입설정 int타입만 사용가능
LinkedList<Integer> num2 = new LinkedList<>(); //new에서 타입 파라미터 생략가능
LinkedList<Integer> list2 = new LinkedList<Integer>(Arrays.asList(1,2)); // 생성시 값추가(Casting)
```

<br>

### 추가

![LinkedList 값 추가](https://blog.kakaocdn.net/dn/bMDHqL/btqEjCtIRbt/NMvN6OfM0Vr5HxiqUMDoF0/img.png)

- `.add(index, value)`: `index`에 `value` 추가
- `.addFirst(value)`: 맨 앞에 `value` 추가
  - head pointer를 통해 바로 가능, O(1)

- `addLast(value)`: 맨 뒤에 `value` 추가
  - head에서부터 마지막까지 탐색하여야 해서 O(n)


```java
import java.util.LinkedList;
LinkedList<Integer> list = new LinkedList<Integer>();

list.addFirst(1); // 가장 앞에 데이터 추가
list.addLast(2); // 가장 뒤에 데이터 추가
list.add(3); // 가장 뒤에 데이터 추가
list.add(1, 10); // index 1에 데이터 10 추가
```

<br>

### 삭제

![LinkedList 값 삭제](https://blog.kakaocdn.net/dn/3KkwI/btqEh72AAaE/wXVXkFgW2ARH77snWIHdL0/img.png)

- `.remove(index)`: `index`의 value 삭제
- `.removeFirst()`: 맨 앞의 value 삭제
- `.removeLast()`: 맨 뒤의 value 삭제
- `.clear()`: 모든 value 삭제

```java
import java.util.LinkedList;
LinkedList<Integer> list = new LinkedList<Integer>(Arrays.asList(1,2,3,4,5));

list.removeFirst(); //가장 앞의 데이터 제거
list.removeLast(); //가장 뒤의 데이터 제거
list.remove(); //생략시 0번째 index제거
list.remove(1); //index 1 제거
list.clear(); //모든 값 제거
```

### 크기

- `.size()`: LinkedList의 크기 반환

```java
import java.util.LinkedList;
LinkedList<Integer> list = new LinkedList<Integer>(Arrays.asList(1,2,3));

System.out.println(list.size()); // 3
```

<br>

### 검색

- `.contains(value)`: `value`가 있다면 `true`를 반환
- `indexOf(value)`: `value`의 `index` 반환, 없다면 `-1` 반환
- 탐색 시간 복잡도는 선형 자료구조이므로 O(n)

<br>

### 시간 복잡도

- 같은 길이를 가지는 두 Linked List를 연결
  - 한 Linked List의 끝에 다른 Linked List를 연결시켜야 함, 끝을 알기 위해 O(n)

<br>

### 이중 연결 리스트

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/Doubly_linked_list.png/400px-Doubly_linked_list.png)

- 각각의 포인터가 앞의 노드와 뒤의 노드를 가리킴
- 단일 연결리스트보다 구현이 어려움
  - 단일 연결 리스트보다 삽입과 삭제 시에 재연결해줘야 하는 포인터의 수가 많아 구현이 더 어렵고 시간이 더 걸림

### 원형 연결 리스트

- 일반적인 연결 리스트에 마지막 노드와 처음 노드를 연결시켜 원형으로 만든 구조
- 다음 노드를 가리키는 포인터가 null을 가리키는 경우가 없음
- CPU 스케줄링 중 모든 프로세스에 균등하고 자원을 반복해서 분배하는 round-robin에 적용될 수 있음
- head에 노드를 추가할 경우 마지막 노드를 찾아야하므로 시간 복잡도는 O(n)

<br>

## 출처

- https://coding-factory.tistory.com/552
- https://psychoria.tistory.com/767
- https://ko.wikipedia.org/wiki/%EC%97%B0%EA%B2%B0_%EB%A6%AC%EC%8A%A4%ED%8A%B8#%EC%9D%B4%EC%A4%91_%EC%97%B0%EA%B2%B0_%EB%A6%AC%EC%8A%A4%ED%8A%B8