# Factory Method Pattern

## Factory Method Pattern이란

- 객체 생성 처리를 서브 클래스에 분리해 처리하도록 캡슐화하는 패턴
  - 즉, 객체의 생성 코드를 별도의 클래스/메서드로 분리하여 객체 생성의 변화에 대비하는데 유용함
  - 특정 기능의 구현은 개별 클래스를 통해 제공되는 것이 바람직한 설계
    - 기능의 변경이나 상황에 따른 기능의 선택은 해당 객체를 생성하는 코드의 변경을 초래
    - 상황에 따라 적절한 객체를 생성하는 코드는 자주 중복될 수 있음
    - 객체 생성 방식의 변화는 해당되는 모든 코드 부분을 변경해야 하는 문제가 발생함
  - 스트래티지 패턴, 싱글턴 패턴, 템플릿 메서드 패턴을 사용
  - 생성 패턴의 일종
- 객체를 생성하기 위해 인터페이스를 정의하지만, 어떤 클래스의 인스턴스를 생성할지에 대한 결정은 서브클래스에서 이루어지도록 하여 **재정의 가능한 것으로 설계**하되 복잡해지지 않게 함
  - 생성할 객체 타입을 예측할 수 없을 때
  - **생성할 객체를 기술하는 책임을 서브클래스에게 정의**하고자 할 때
  - 객체 생성의 책임을 서브클래스에 위임시키고 **서브클래스에 대한 정보를 은닉**하고자 할 때

## 장단점

### 장점

- 기존 코드(인스턴스를 만드는 과정)를 수정하지 않고 새로운 인스턴스를 다른 방법으로 생성하도록 확장할 수 있다.

  - Product 와 Creator 간의 커플링(결합)이 느슨함

  - 확장에 열려있고 변경에 닫혀있는 객체지향 원칙

    을 적용했기 때문에 가능

    - 확장 : 새로운 인스턴스 추가
    - 변경 : 기존 코드를 수정

- **코드가 간결**해진다.

- 병렬적 클래스 계층도를 연결하는 역할을 담당할 수 있음

### 단점

- 클래스가 많아진다. (클래스 계층도 커질 수 있다.)
  - 제품 클래스가 바뀔 때마다 새로운 서브클래스를 생성해야 한다.
- 클라이언트가 creator 클래스를 반드시 상속해 Product를 생성해야 한다.

## 구조

<img src="05_Factory Method Pattern.assets/img.gif">

- **Product**
  - 팩토리 메서드로 생성될 **객체**의 공통 인터페이스
- **ConcreteProduct**
  - **구체적인 객체**가 생성되는 클래스
- **Creator**
  - **팩토리 메서드**를 갖는 클래스
- **ConcreteCreator**
  - 팩토리 메서드를 **구현**하는 클래스로, **ConcreteProduct** 객체를 생성

## 개념과 적용 방법

- 객체 생성을 전담하는 **별도의 Factory 클래스 이용**
  - 스트래티지 패턴과 싱글턴 패턴을 이용
- **상속**을 이용하여 하위 클래스에서 적합한 클래스의 객체를 생성
  - 스트래티지 패턴, 싱글턴 패턴과 템플릿 메서드 패턴을 이용

## 적용 예시

> **게임에서 유닛을 생성하는 코드의 예시**
>
> 전사와 마법사 두 종류의 유닛이 있으며, 해당 유닛은 레벨업을 할 때마다 체력과 마나가 각기 다른 수치만큼 증가하게 된다.

<img src="05_Factory Method Pattern.assets/img.png">

### Creator

```java
public abstract class MyCharacterCreator {

    public MyCharacter createNewCharacter() {
        final MyCharacter character = createCharacter(); <-- 이 부분에서 factory 메소드가 사용됨

        character.setHp(100);
        character.setMp(100);

        return character;
    }
    
    // Factory Method
    protected abstract MyCharacter createCharacter();
}
```

```java
public class WarriorCreator extends MyCharacterCreator {
    @Override
    protected MyCharacter createCharacter() {
        return new Warrior();
    }
}
```

```java
public class MagicianCreator extends MyCharacterCreator {
    @Override
    protected MyCharacter createCharacter() {
        return new Magician();
    }
}
```

### Product

```java
@Getter
@Setter
public abstract class MyCharacter {

    protected int hp;
    protected int mp;

    public abstract void levelUp();
}
```

```java
public class Warrior extends MyCharacter {
    @Override
    public void levelUp() {
        this.mp += 10;
        this.hp += 20;
    }
}
```

```java
public class Magician extends MyCharacter {
    @Override
    public void levelUp() {
        this.hp += 10;
        this.mp += 20;
    }
}
```

### 호출

```java
// 마법사를 생성할 때
public static void main(String[] args) {
    final MagicianCreator magicianCreator = new MagicianCreator();

    final MyCharacter character = magicianCreator.createNewCharacter();

    character.levelUp();
}

// 전사를 생성할 때
public static void main(String[] args) {
    final WarriorCreator warriorCreator = new WarriorCreator();

    final MyCharacter character = warriorCreator.createNewCharacter();

    character.levelUp();
}
```

---

### 또 다른 게임의 예시

```java
class Unit {
    Unit() {
        //생성자
    }
   //이하 유닛의 메소드들
}
```

```java
class Marine extends Unit {
    Marine() {
        //생성자
    }
    //이하 마린의 메소드들
}
```

```java
class Firebat extends Unit {
    Firebat() {
        //생성자
    }
    //이하 파이어뱃의 메소드들
}
```

```java
class Map {
    Map(File mapFile) {
        while(mapFile.hasNext() == true) {
            String[] unit = mapFile.getNext();
            if(unit[0].equals("Marine")) {
                Marine marine = new Marine(unit);
            } else if(unit[0].equals("Firebat")) {
                Firebat firebat = new Firebat(unit);
            }
           //기타 유닛의 생성자들
       }
       //유닛 초기화 이후 코드
    }
}
```

위의 클래스 구조는 작동에는 문제가 없지만 단일책임원칙에 위배됨

Map클래스는 맵에 관련된 역할만 수행해야 하는데, 맵을 불러오면서 유닛 배치를 위해 **생성되는 유닛을 분류**해야 하는 역할도 떠안고 있음

따라서 새로운 UnitCreator 클래스를 만들면,

```java
class UnitCreator {
    static Unit create(String[] data) {
        if(data[0].equals("Marine")) {
            return new Marine(data);
        } else if(data[0].equals("Firebat")) {
            return new Firebat(data);
        }
       //기타 유닛의 생성자들
    }
}
```

```java
class Map {
    Map(File mapFile) {
        while(mapFile.hasNext() == true) {
            Unit unit = UnitFactory.create(mapFile.getNext());
       }
       //유닛 초기화 이후 코드
    }
}
```

[참고 블로그](https://gmlwjd9405.github.io/2018/08/07/factory-method-pattern.html)
[전사/마법사 예시 출처](https://robin00q.tistory.com/85)
