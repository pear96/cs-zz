# Template Method Pattern

## 정의

### GoF의 정의

> 알고리즘의 구조를 메소드에 정의하고, 하위 클래스에서 알고리즘 구조의 변경없이 알고리즘을 재정의 하는 패턴이다. 알고리즘이 단계별로 나누어 지거나, 같은 역할을 하는 메소드이지만 여러곳에서 다른형태로 사용이 필요한 경우 유용한 패턴이다.

- 어떤 작업을 처리하는 일부분을 서브 클래스로 캡슐화해 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내용을 바꾸는 디자인 패턴
  - 전체적인 구조는 유지하면서 부분적으로 다른 구문으로 구성된 메서드의 코드 중복을 최소화 할 때 유용함
  - 동일한 기능을 상위 클래스에 정의해두고 확장 혹은 변화가 필요한 부분만 서브 클래스에서 구현
  - **행위(Behavioural) 패턴**
    - 객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴
    - 한 객체가 혼자 수행할 수 없는 작업을 여러 개의 객체로 어떻게 분배하는지, 또 그렇게 하면서도 객체 사이의 결합도를 최소화하는 것에 중점을 두는 패턴
<img src="03_Template Method Pattern.assets/template-method-pattern.png">

- AbstractClass
  - 템플릿 메서드를 정의
  - 하위 클래스에서 공통 알고리즘을 정의하고 하위 클래스에서 구현할 기능을 정의
- ConcreteClass
  - 상속 받은 primitive 메서드 또는 hook 메서드를 구현
  - 상위 클래스에 구현된 템플릿 메서드의 일반적인 알고리즘에서 하위 클래스에 적합하게 오버라이딩

## 템플릿 메소드 패턴 장단점

### 장점

**1.** 중복코드를 줄일 수 있다.

**2.** 자식 클래스의 역할을 줄여 핵심 로직의 관리가 용이하다.

**3.** 좀더 코드를 객체지향적으로 구성할 수 있다.

### 단점

**1.** 추상 메소드가 많아지면서 클래스 관리가 복잡해진다.

**2.** 클래스간의 관계와 코드가 꼬여버릴 염려가 있다.

## 예시

<img src="03_Template Method Pattern.assets/template-method-example.png">

- 엘리베이터 제어 시스템의 모터 구동시키는 기능의 예시
  - 현대 모터를 이용하는 제어 시스템은 HyundaiMotor 클래스의 move 메서드를 정의할 수 있다.
  - HyundaiMotor 클래스와 Door 클래스의 연관 관계
    - move 메서드를 실행할 때 Door가 닫혀 있는지 확인하기 위해 연관 관계가 성립함

```java
public class Door {
  private DoorStatus doorStatus;

  public Door() { doorStatus = DoorStatus.CLOSED; }
  public DoorStatus getDoorStatus() { return doorStatus; }
  public void close() { doorStatus = DoorStatus.CLOSED; }
  public void open() { doorStatus = DoorStatus.OPENED; }
}
```

```java
public class HyundaiMotor {
  private Door door;
  private MotorStatus motorStatus;

  public HyundaiMotor() {
    this.door = door;
    motorStatus = MotorStatus.STOPPED; // 초기: 멈춘 상태
  }
  private void moveHyundaiMotor(Direction direction) {
    // Hyundai Motor를 구동시킴
  }
  public MotorStatus getMotorStatus() { return motorStatus; }
  private void setMotorStatus() { this.motorStatus = motorStatus; }

  /* 엘리베이터 제어 */
  public void move(Direction direction) {
    MotorStatus motorStatus = getMotorStatus();
    // 이미 이동 중이면 아무 작업을 하지 않음
    if (motorStatus == MotorStatus.MOVING) return;

    DoorStatus doorStatus = door.getDoorStatus();
    // 만약 문이 열려 있으면 우선 문을 닫음
    if (doorStatus == DoorStatus.OPENED) door.close();

    // Hyundai 모터를 주어진 방향으로 이동시킴
    moveHyundaiMotor(direction);
    // 모터 상태를 이동 중으로 변경함
    setMotorStatus(MotorStatus.MOVING);
  }
}
```

```java
public class Client {
  public static void main(String[] args) {
    Door door = new Door();
    HyundaiMotor hyundaiMotor = new HyundaiMotor(door);
    hyundaiMotor.move(Direction.UP); // 위로 올라가도록 엘리베이터 제어
  }
}
```

- HyundaiMotor 클래스의 move 메서드는 우선 getMotorStatus 메서드를 호출해 모터의 상태를 조회한다.
  - 모터가 이미 동작 중이면 move 메서드의 실행을 종료한다.
- Door 클래스의 getDoorStatus 메서드를 호출해 문의 상태를 조회한다.
  1. 문이 열려 있는 상태면 Door 클래스의 close 메서드를 호출해 문을 닫는다.
  2. 그리고 moveHyundaiMotor 메서드를 호출해 모터를 구동시킨다.
  3. setMotorStatus를 호출해 모터의 상태를 MOVING으로 기록한다.

### 문제점

- 다른 클래스의 모터를 제어해야 하는 경우
  - 현대 모터 이외에 LG 모터까지 관리해야할 필요가 생긴다면?

```java
public class LGMotor {
  private Door door;
  private MotorStatus motorStatus;

  public LGMotor() {
    this.door = door;
    motorStatus = MotorStatus.STOPPED; // 초기: 멈춘 상태
  }
  private void moveLGMotor(Direction direction) {
    // LG Motor를 구동시킴
  }
  public MotorStatus getMotorStatus() { return motorStatus; }
  private void setMotorStatus() { this.motorStatus = motorStatus; }

  /* 엘리베이터 제어 */
  public void move(Direction direction) {
    MotorStatus motorStatus = getMotorStatus();
    // 이미 이동 중이면 아무 작업을 하지 않음
    if (motorStatus == MotorStatus.MOVING) return;

    DoorStatus doorStatus = door.getDoorStatus();
    // 만약 문이 열려 있으면 우선 문을 닫음
    if (doorStatus == DoorStatus.OPENED) door.close();

    // LG 모터를 주어진 방향으로 이동시킴
    moveLGMotor(direction); // (이 부분을 제외하면 HyundaiMotor의 move 메서드와 동일)
    // 모터 상태를 이동 중으로 변경함
    setMotorStatus(MotorStatus.MOVING);
  }
}
```

- HyundaiMotor 클래스와 LGMotor 클래스에는 getMotorStatus, setMotorStatus 등 중복 코드가 많아짐

### 해결책

#### 방법 1 - 상속

<img src="03_Template Method Pattern.assets/template-method-solution1.png">

```java
/* HyundaiMotor와 LGMotor의 공통적인 기능을 구현하는 클래스 */
public abstract class Motor {
  protected Door door;
  private MotorStatus motorStatus; // 공통 2. motorStatus 필드

  public Motor(Door door) { // 공통 1. Door 클래스와의 연관관계
    this.door = door;
    motorStatus = MotorStatus.STOPPED;
  }
  // 공통 3. etMotorStatus, setMotorStatus 메서드
  public MotorStatus getMotorStatus() { return MotorStatus; }
  protected void setMotorStatus(MotorStatus motorStatus) { this.motorStatus = motorStatus; }
}
```

```java
/* Motor를 상속받아 HyundaiMotor 클래스를 구현 */
public class HyundaiMotor extends Motor{
  public HyundaiMotor(Door door) { super(door); }
  private void moveHyundaiMotor(Direction direction) {
    // Hyundai Motor를 구동시킴
  }
  public void move(Direction direction) {
    위와 동일
  }
}
```

```java
/* Motor를 상속받아 LGMotor 클래스를 구현 */
public class LGMotor extends Motor{
  public LGMotor(Door door) { super(door); }
  private void moveLGMotor(Direction direction) {
    // LG Motor를 구동시킴
  }
  public void move(Direction direction) {
    위와 동일
  }
}
```

- 대부분의 코드 중복은 면했지만, move 메서드는 여전히 유사한 코드로 작성되어 완벽한 해결이 아님

#### 방법 2 - 부분적으로 중복되는 메서드 역시 상속을 활용해서 중복을 줄일 수 있다.

<img src="03_Template Method Pattern.assets/template-method-solution2.png">

1. move 메서드를 상위 클래스로 이동시키고
2. 각 하위 클래스에서 오버라이딩하여 정의한다.

```java
/* HyundaiMotor와 LGMotor의 공통적인 기능을 구현하는 클래스 */
public abstract class Motor {
  ...
  위와 동일

  // HyundaiMotor와 LGMotor의 move 메서드에서 공통되는 부분만을 가짐
  public void move(Direction direction) {
    MotorStatus motorStatus = getMotorStatus();
    // 이미 이동 중이면 아무 작업을 하지 않음
    if (motorStatus == MotorStatus.MOVING) return;

    DoorStatus doorStatus = door.getDoorStatus();
    // 만약 문이 열려 있으면 우선 문을 닫음
    if (doorStatus == DoorStatus.OPENED) door.close();

    // 모터를 주어진 방향으로 이동시킴
    moveMotor(direction); // (HyundaiMotor와 LGMotor에서 오버라이드 됨)
    // 모터 상태를 이동 중으로 변경함
    setMotorStatus(MotorStatus.MOVING);
  }
}
```

```java
/* Motor를 상속받아 HyundaiMotor 클래스를 구현 */
public class HyundaiMotor extends Motor{
  public HyundaiMotor(Door door) { super(door); }
    
  @Override
  protected void moveMotor(Direction direction) {
    // Hyundai Motor를 구동시킴
  }
}
```

```java
/* Motor를 상속받아 LGMotor 클래스를 구현 */
public class LGMotor extends Motor{
  public LGMotor(Door door) { super(door); }
    
  @Override
  protected void moveMotor(Direction direction) {
    // LG Motor를 구동시킴
  }
}
```

### 메서드 오버라이딩의 또 다른 예시

```java
//추상 클래스 선생님
abstract class Teacher{
	
    public void start_class() {
        inside();
        attendance();
        teach();
        outside();
    }
	
    // 공통 메서드
    public void inside() {
        System.out.println("선생님이 강의실로 들어옵니다.");
    }
    
    public void attendance() {
        System.out.println("선생님이 출석을 부릅니다.");
    }
    
    public void outside() {
        System.out.println("선생님이 강의실을 나갑니다.");
    }
    
    // 추상 메서드
    abstract void teach();
}
 
// 국어 선생님
class Korean_Teacher extends Teacher{
    
    @Override
    public void teach() {
        System.out.println("선생님이 국어를 수업합니다.");
    }
}
 
//수학 선생님
class Math_Teacher extends Teacher{

    @Override
    public void teach() {
        System.out.println("선생님이 수학을 수업합니다.");
    }
}

//영어 선생님
class English_Teacher extends Teacher{

    @Override
    public void teach() {
        System.out.println("선생님이 영어를 수업합니다.");
    }
}

public class Main {
    public static void main(String[] args) {
        Korean_Teacher kr = new Korean_Teacher(); //국어
        Math_Teacher mt = new Math_Teacher(); //수학
        English_Teacher en = new English_Teacher(); //영어
        
        kr.start_class();
        System.out.println("----------------------------");
        mt.start_class();
        System.out.println("----------------------------");
        en.start_class();
    }
}
```

