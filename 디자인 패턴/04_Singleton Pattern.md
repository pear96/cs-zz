# Singleton Pattern

## 정의

객체의 인스턴스가 오직 1개만 생성되는 패턴

> 생성된 객체를 어디에서든지 참조할 수 있다. - 생성(Creational) 패턴
>
> - 생성 패턴
>   - 객체 생성에 관련된 패턴
>   - 객체의 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않도록 유연성을 제공

## 장단점

### 장점

1. 고정된 메모리 영역을 얻으면서 단 한번만 new로 생성한 후 같은 인스턴스를 사용하여 메모리 낭비를 방지할 수 있다.
2. 전역 인스턴스이므로 다른 클래스에도 데이터를 공유하기 쉽다.
3. **절대적으로 단 한개의 인스턴스만 유지하고 싶은 경우** 필요한 패턴이다.
4. 두 번째 이용시부터 객체 로딩 시간이 줄어 성능이 향상된다.

### 단점

1. 싱글턴 인스턴스가 너무 많은 일을 하거나 공유하는 데이터가 많아질 수록 다른 클래스들과의 결합도가 높아져 "개방-폐쇄 원칙"을 위배하게 된다.

   이는 OOP의 원칙에 어긋나므로 수정이 어려워지고 유지보수의 비용이 높아질 수 있다.

2. 멀티쓰레드 환경에서는 동기화 처리가 안될 경우 인스턴스가 2개가 될 수도 있다.
   - 경합 조건(Race Condition) 을 발생시키는 경우
     - 싱글턴 인스턴스가 아직 생성되지 않았을 때 `스레드 1`이 `getInstance` 메서드의 if문을 실행해 이미 인스턴스가 생성되었는지 확인한다. 현재 싱글턴 변수는 null인 상태다.
     - 만약 `스레드 1`이 생성자를 호출해 인스턴스를 만들기 전 `스레드 2`가 if문을 실행해 싱글턴 변수가 null인지 확인한다. 현재 싱글턴 변수는 null이므로 인스턴스를 생성하는 생성자를 호출하는 코드를 실행하게 된다.
     - `스레드 1`도 `스레드 2`와 마찬가지로 인스턴스를 생성하는 코드를 실행하게 되면 결과적으로 싱글턴 클래스의 인스턴스가 2개 생성된다.
3. 테스트가 어렵다.
   - 싱글턴 인스턴스는 자원을 공유하기 때문에 테스트가 결정적으로 격리된 환경에서 수행되도록 하려면 인스턴스의 상태가 매번 초기화 되어야 한다.

꼭 필요한 경우가 아니면 사용하지 말 것.

## 예시

```java
public class Singleton {

    private static Singleton instance = new Singleton();
    
    private Singleton() {
        // 생성자는 외부에서 호출못하게 private 으로 지정해야 한다.
    }

    public static Singleton getInstance() {
        return instance;
    }

    public void say() {
        System.out.println("hi, there");
    }
}
```

### 프린터의 예시

```java
public class Printer {
  // 외부에 제공할 자기 자신의 인스턴스
  private static Printer printer = null;
  private Printer() { }
  // 자기 자신의 인스턴스를 외부에 제공
  public static Printer getPrinter(){
    if (printer == null) {
      // Printer 인스턴스 생성
      printer = new Printer();
    }
    return printer;
  }
  public void print(String str) {
    System.out.println(str);
  }
}
```

```java
public class User {
  private String name;
  public User(String name) { this.name = name; }
  public void print() {
    Printer printer = printer.getPrinter();
    printer.print(this.name + " print using " + printer.toString());
  }
}
public class Client {
  private static final int USER_NUM = 5;
  public static void main(String[] args) {
    User[] user = new User[USER_NUM];
    for (int i = 0; i < USER_NUM; i++) {
      // User 인스턴스 생성
      user[i] = new User((i+1))
      user[i].print();
    }
  }
}
```

위의 코드는 경합 조건을 발생할 경우 인스턴스가 두 개 생성될 수 있음.

### 해결

1. 정적 변수에 인스턴스를 만들어 바로 초기화

```java
public class Printer {
   // static 변수에 외부에 제공할 자기 자신의 인스턴스를 만들어 초기화
   private static Printer printer = new Printer();
   private Printer() { }
   // 자기 자신의 인스턴스를 외부에 제공
   public static Printer getPrinter(){
     return printer;
   }
   public void print(String str) {
     System.out.println(str);
   }
}
```

2. 인스턴스를 만드는 메서드에 동기화

```java
public class Printer {
   // 외부에 제공할 자기 자신의 인스턴스
   private static Printer printer = null;
   private int counter = 0;
   private Printer() { }
   // 인스턴스를 만드는 메서드 동기화 (임계 구역)
   public synchronized static Printer getPrinter(){
     if (printer == null) {
       printer = new Printer(); // Printer 인스턴스 생성
     }
     return printer;
   }
   public void print(String str) {
     // 오직 하나의 스레드만 접근을 허용함 (임계 구역)
     // 성능을 위해 필요한 부분만을 임계 구역으로 설정한다.
     synchronized(this) {
       counter++;
       System.out.println(str + counter);
     }
   }
}
```

### 또 다른 예시

```c++
class Buffer {
    private:
        Buffer() { cursize = 0; };
        static bool instanceFlag;
        static Buffer* instance;
        int cursize;
    public:
        static Buffer* getInstance();
        virtual ~Buffer() {
            instanceFlag = false;
        }
        bool IsFull();
        bool IsEmpty();
        Order buffer[max];
        int add(Order NewOrder);
        int retrieve(Order& dummy);
};
```

```c++
bool Buffer::instanceFlag = false;
Buffer* Buffer::instance = NULL;

Buffer* Buffer::getInstance() {
	if (!instance) {
		instance = new Buffer();
		instanceFlag = true;
	}
	return instance;
}

bool Buffer::IsFull() {
	if (cursize == max)
		return 1;
	else
		return 0;
}

bool Buffer::IsEmpty() {
	if (cursize == 0)
		return 1;
	else
		return 0;
}

int Buffer::add(Order NewOrder) {
	if (IsFull()) {
		cout << "Buffer is already full." << endl;
		return -1;
	}
	buffer[cursize] = NewOrder;
	cursize++;
	return 1;
}

int Buffer::retrieve(Order& dummy) {
	if (IsEmpty()) {
		cout << "There is nothing to retrieve." << endl;
		return 0;
	}
	int ind = 0;
	bool found = false;
	while (ind < cursize && !found) {//linear search
		if (dummy.GetShopId() == buffer[ind].GetShopId()) { // if hit
			cout << "Retrieving an order." << endl;
			dummy = buffer[ind]; // copy
			int dex = ind; // delete and tighten up
			while (dex < cursize) {
				buffer[dex] = buffer[dex + 1];
				dex++;
			}
			found = true;
			cursize--;
		}
		ind++;
	}
	return 1;
}
```

```c++
class Client {
    private:
        CircularStackType* RecentOrder;
        string cId;
        Buffer* bufferptr;
        Tree* treeptr;
        int m_nCurCommand;
    public:
        /**
        *	default constructor
        */
        Client(string id = "");

        /**
        *	destructor
        */
        ~Client();

        void Run();

        int GetCommand();

        string GetId();

        /**
        *	@brief	Makes new order and push into the RecentOrder stack.
        *	@pre	None
        *	@post	New order objective is pushed into RecentOrder stack.
        */
        void OrderNew();

        /**
        *	@brief	Shows first item of RecentOrder stack.
        *	@pre	The stack should be initialised.
        *	@post	Information of most recent order is shown.
        */
        void ShowAllRecent();

        /**
        *	@brief	Prints receipt of the last order.
        *	@pre	The stack should be initialised and not empty.
        *	@post	The receipt is printed as file output.
        */
        void PrintBill();
};
```

```c++
Client::Client(string id) {
	RecentOrder = new CircularStackType;
	cId = id;
	bufferptr = Buffer::getInstance();
	treeptr = Tree::getInstance();
	m_nCurCommand = 0;
	//look for linked list by id to find existing recent list
}
```

