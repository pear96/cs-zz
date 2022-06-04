# Facade Pattern

## Facade Pattern이란

> Facade :
>
> 1. 길거리에서 보이는 건물의 정면
> 2. **속의 내용물을 감추기 위해 유지하는 외관**

파사드의 위의 정의와 같이 소프트웨어가 복잡하게 얽혀있는 경우, 복잡한 소프트웨어의 코드가 라이브러리 안쪽에 의존하는 일을 감소시켜 주고 간단하게 사용할 수 있는 인터페이스를 제공하는 디자인 패턴

GoF 서적에 따르면 "하위 시스템을 보다 쉽게 사용할 수 있게 해주는 고급 인터페이스를 정의한다."고 되어 있음.

Adapter 패턴과 거의 같은 방식으로 작동하지만 목적이 서로 다름

- Adapter 패턴은 원래 코드를 다른 코드와 작동할 수 있는 래퍼를 제공
- Facade 패턴은 원래 코드를 더 쉽게 처리할 수 있는 래퍼를 제공

## 사용 예

- 캡슐화되지 않은 코드를 처리할 때 Facade 패턴을 사용함
- 원하는 코드를 다시 작성 할 수 없을 때 일반적으로 Facade 패턴을 사용함
  하지만 기본 코드가 변경되면 Facade 패턴도 변경되어야 한다.

## 구조

<img src="02_Facade Pattern.assets/scode=mtistory2&fname=http%3A%2F%2Fcfile4.uf.tistory.png">

## 코드 예시

### 스마트홈의 예시

```java
public class Computer {
    private boolean turnedOn = false;

    public void turnOn() {
        turnedOn = true;
        System.out.println("컴퓨터를 킨다.");
    }
    public void turnOff() {
        turnedOn = false;
        System.out.println("컴퓨터를 끈다.");
    }
    public boolean isTurnedOn() {
        return turnedOn;
    }
}
```

```java
public class Light {
    private boolean turnedOn = false;

    public void turnOn() {
        turnedOn = true;
        System.out.println("불을 킨다.");
    }
    public void turnOff() {
        turnedOn = false;
        System.out.println("불을 끈다.");
    }
    public boolean isTurnedOn() {
        return turnedOn;
    }
}
```

```java
public class Radio {
    private boolean turnedOn = false;

    public void turnOn() {
        turnedOn = true;
        System.out.println("라디오를 킨다.");
    }
    public void turnOff() {
        turnedOn = false;
        System.out.println("라디오를 끈다.");
    }
    public boolean isTurnedOn() {
        return turnedOn;
    }
}
```

이제 위 세개의 클래스를 하나의 메서드가 다 처리하도록 통합하는 과정.

```java
public class HomeFacade {
    private Computer computer;
    private Light light;
    private Radio radio;

    public HomeFacade(Computer computer, Light light, Radio radio) {
        this.computer = computer;
        this.light = light;
        this.radio = radio;
    }
    public void homeIn() {
        System.out.println("집에 오면 컴퓨터 켜고, 라디오 켜고, 불 켜기.");
        if (!computer.isTurnedOn()) {
            computer.turnOn();
        }
        if (!light.isTurnedOn()) {
            light.turnOn();
        }
        if (!radio.isTurnedOn()) {
            radio.turnOn();
        }
    }
    public void homeOut() {
        System.out.println("컴퓨터 끄고, 라디오 끄고, 불 끄고 외출하기.");
        if (computer.isTurnedOn()) {
            computer.turnOff();
        }
        if (light.isTurnedOn()) {
            light.turnOff();
        }
        if (radio.isTurnedOn()) {
            radio.turnOff();
        }
    }
}
```

```java
public class TestPattern {

    public static void main(String[] args) {

        Computer computer = new Computer();
        Light light = new Light();
        Radio radio = new Radio();

        // 이전 사용 방식
        System.out.println("========집에 나갈때 파사드 적용 전==========");
        computer.turnOff();
        light.turnOff();
        radio.turnOff();

        System.out.println("========집에 들어올때 파사드 적용 후==========");
        // 파사드 패턴 적용 후 사용 방식
        HomeFacade home = new HomeFacade(computer, light, radio);
        home.homeIn();
    }
}
```

### 결과

<img src="02_Facade Pattern.assets/img-16538264865143.png">

### 클래스 다이어그램

<img src="02_Facade Pattern.assets/img.png">