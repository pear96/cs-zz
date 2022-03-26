# 3. ArrayList

- List Interface를 상속받은 여러 클래스 중 하나
- 일반 배열과 동일하게 연속된 메모리 공간을 사용
- 인덱스는 0부터 시작

- Array와 다르게 배열의 크기가 가변적
  - Capacity: 내부적으로 저장이 가능한 메모리 용량 존재
  - Size: 현재 사용중인 공간의 크기
  - Capacity 이상의 size를 사용하려고 하면 더 큰 공간의 메모리 할당

<br>

### 생성

- `Arrays.asList()`: 일반 Array를 ArrayList로 변환


```java
import java.util.ArrayList;

ArrayList<Integer> integers1 = new ArrayList<Integer>(); // 타입 지정
ArrayList<Integer> integers2 = new ArrayList<>(); // 타입 생략 가능
ArrayList<Integer> integers3 = new ArrayList<>(10); // 초기 용량(Capacity) 설정
ArrayList<Integer> integers4 = new ArrayList<>(integers1); // 다른 Collection값으로 초기화

import java.util.Arrays;

ArrayList<Integer> integers5 = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5)); // Arrays.asList()
```

<br>

### 추가/변경

- `.add(index, value)`: `index`에 `value` 추가, `index`가 생략된 경우 ArrayList의 가장 끝에 추가
- `.set(index, value):` `index`의 값을 `value`로 변경, `index`가 생략된 경우, 가장 앞의 값 변경

```java
import java.util.ArrayList;

ArrayList<String> colors = new ArrayList<>();

colors.add("Black");
colors.add(0, "Green");

colors.set(0, "Blue");
```

<br>

### 삭제

- `.remove(int index)`: parameter가 `int`일 경우 `index`의 `value` 삭제, 삭제된 `value` 반환
- `.remove(Object value)`: parameter가 `Object`일 경우 첫번째로 나오는 `value` 값 삭제, `Boolean` 반환
- `.clear()`: 모든 value 삭제

```java
import java.util.ArrayList;

ArrayList<String> colors = new ArrayList<>(Arrays.asList("Black", "White", "Green", "Red"));

colors.remove(0); //index 0 제거
colors.remove(int 0); //index 0 제거

colors.remove("White"); // 'White' 값 제거
colors.remove(Object "White"); // 'White' 값 제거

list.clear(); //모든 값 제거
```

### 크기

- `.size()`: ArrayList의 크기 반환

```java
import java.util.ArrayList;

ArrayList<String> colors = new ArrayList<>(Arrays.asList("Black", "White", "Green", "Red"));

System.out.println(colors.size()); // 3 출력
```

<br>

### 검색

- `.contains(value)`: `value`가 있다면 `true`를 반환
- `indexOf(value)`: `value`의 `index` 반환, 없다면 `-1` 반환

<br>

## 출처

- https://psychoria.tistory.com/765
- https://coding-factory.tistory.com/551