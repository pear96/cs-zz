# CS #1 Refactoring

> 컴퓨터가 인식 가능한 코드는 바보라도 작성할 수 있지만,
>
> 인간이 이해할 수 있는 코드는 실력있는 프로그래머만 작성할 수 있다.
>
> -**마틴 파울러**-

<br>

### 정의

> 겉으로 드러나는 기능은 그대로 유지
>
> **BUT** 알아보기 쉽고 수정이 용이하도록 리팩토링 기법을 적용하여 소프트웨어 내부 수정

- **두 개의 모자**
  - 두 가지 구별된 작업(**기능 추가 / 리팩토링**)을 위해 시간을 나누어야 한다.
  - 기능 추가 -> 기존 코드 수정 X
  - 리팩토링 -> 기능 추가 X
  - 기능 추가를 위해 개발 시작 -> 코드 구조를 바꾸면 개발이 쉬워짐을 파악 -> 리팩토링 진행 -> 다시 기능 추가 -> 기능 동작 확인 -> 새로 만든 기능에 대한 코드의 구조 수정 -> 리팩토링 진행

<br>

### 이유

- 더 나은 설계
  - 지속적인 리팩토링이 이루어지지 않으면 설계 파악이 어려워짐
  - <u>이해해야할 코드량 증가</u>
- 더 쉬운 **코드 이해**
- **버그** 발견 용이
  - 코드가 하는 일을 깊게 파악
- **생산성** 증가
  - 프로그래밍 속도 상승
  - 품질 향상

<br>

### 언제 시행?

- 기능 추가를 쉽게 하기 위해
  - 중복 코드가 많이 보이면 **매개변수화**하여 중복을 피함
  - 오류를 발생시키는 코드가 퍼져있다면 **한 곳에 합쳐** 작업
- 코드 이해를 쉽게 하기 위해
  - 의도가 더 명확하게 드러나도록
- **비효율**적인 코드를 발견했을 때
  - 간단하게 수정할 수 있다면 바로 수정
  - 오래 걸리는 것으로 판단되면 천천히 작업
- ***<u>같은 작업을 3번째 반복하게 되었을 때</u>***

<br>

### Bad Smell (코드의 구린내)

> 설계의 문제 or 심각한 문제점의 조짐

1. **Bloaters : gargantuan code (거대한 코드)**

   > 너무 커지거나 길어진 상태

   - Long Method
     - Method가 너무 길어진 형태.
   - Large Class
     - Class가 너무 큼
     - 너무 많은 **fields / methods/ lines**을 가진 경우
   - Primitive Obsession
     - **Simple Task**를 위해 **Small Object**를 사용하는 것이 아닌 **Primitives**를 사용하는 경우
   - Long Parameter List
     - **3 ~ 4**개 이상의 매개변수를 가지는 Method
     - 매개변수가 간결해야 코드 이해 용이
     - <u>단, Parameter 간의 관계가 적다면 해당 문제를 가지지 않는다고 볼 수 있음</u>
   - Data Clumps
     - 3 ~ 4개 이상의 데이터 뭉치
     - 이름, 나이, 성별 등을 각각의 변수로 가지고 있는 경우 -> 사람 Class로 대체 가능

   <br>

2. **Object-Oriented Abusers**

   > Object-Oriented 개념이 잘 적용되지 못한 경우

   - Switch Statements
     - 복잡한 **Switch**문이나 **If**문이 있는 경우
     - 다형성을 활용하여 해결 / Method 추출 / 서브클래스
   - Temporary Field
     - **임시 변수**를 활용하여 다른 동작이나 판단을 하는 코드
     - Method 생성을 통해 해결
   - Refused Bequest (거부된 유산?)
     - 부모 클래스를 상속했지만 **일부 Methods / Properties**만 사용하는 경우
     - 부모 클래스를 수정
   - Alternative Class With Different Interface
     - **서로 다른 클래스**가 **같은 기능**을 수행하지만 다른 **Method 이름**을 가진 경우

   <br>

3. **Change Preventers**

   > 하나의 결과가 변경되는 것이 다른 여러 곳의 변경을 필요로 하는 경우

   - Divergent Change
     - **하나의 클래스**가 다양한 원인으로 인해 **수정**되는 경우
     - 단일 책임 원칙을 위배했을 가능성 높음
   - Shotgun Surgery
     - **하나의 변경**이 **산발적인 수정**을 유발하는 경우
   - Parallel Inheritance Hierarchies
     - 한 클래스의 <u>서브 클래스</u>를 **만들 때마다** 다른 클래스의 서브 클래스를 만들어야하는 경우

   <br>

4. **Dispensables**

   > 불필요한 것들

   - Comments
     - **너무 많은** 주석
   - Duplicate Code
     - **중복**된 코드
   - Lazy Class
     - 하나의 클래스를 작성할 때마다 **유지 관리와 이해 비용**이 추가되는 경우
   - Data Class
     - **Getter / Setter**만 존재하는 클래스
   - Dead Code
     - 더 이상 사용되지 않는 코드
   - Speculative Generality (추측성 일반화)
     - 아무것도 하지 않는데 **추측성으로 생긴 코드**
     - **YAGNI**
       - **Y**ou **A**ren't **G**onna **N**eed **I**t
       - 실제로 필요할 때 구현하되 필요할 것이라고 예상할 때에는 구현 X

   <br>

5. **Couplers**

   > 클래스간의 잘못된 Dependency

   - Feature Envy
     - Method가 자기 자신보다 **다른 Object의 Data**를 더 많이 사용하는 경우
   - Inappropriate Intimacy
     - 클래스끼리 **지나치게 밀접**하여 private field, method 까지 알아내려 하는 경우
     - 캡슐화, 정보 은닉 위반
   - Message Chain
     - `A` -> `B()` -> `C()` -> `D()`와 같이 **연속하여 Call**하는 경우
   - Middle Man
     - 어떤 클래스가 다른 클래스의 **Delegation의 역할**만 하는 경우
   - Incomplete Library Class
     - **Library**에서 제공하는 methods들이 **완전하지 않은** 경우

<br>

### 작은 습관

> 리팩토링은 본래 작업이 끝난 후 진행
>
> 작업하면서도 도입할 수 있으면서도 얼마나 고심하여 코드를 작성했는지 보여줄 수 있는
>
> 작은 습관

```java
// 1. 비교 [고정값] [비교 연산자] [변동값]
int a = 10;
/*
    if (a > 10) {
        // code
    }
*/
if (10 < a) {
    // recommend!!
}


// 2. 반복문 크기 설정
List<Integer> tmpList;
/*
    for (int i = 0; i < tmpList.size(); i++) {
        // code
    }
*/
for (int i = 0, size = tmpList.size(); i < size; i++) {
    // recommend!!
}


// 3. 반복문 대입
List<String> strList;
/*
    for (int i = 0, size = strList.size(); i < size; i++) {
        String tmpStr = strList.get(i);
        // code
    }
*/
String tmpStr;
for (int i = 0, size = strList.size(); i < size; i++) {
    tmpStr = strList.get(i);
    // recommend!!
}
```

