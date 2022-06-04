# DB #8 JDBC & Connection Pool

### DB Connection

> DB와 어플리케이션 간 통신을 할 수 있는 수단

웹 아키텍처

- 2 Tier
  - Client가 직접 DB에 접근하는 구조
- 3 Tier
  - Client - Middle Ware - DB
  - Middle Ware가 비즈니스 로직 구현, 트랜잭션 처리 등을 수행
  - Spring Project는 여기!

<br>

### JDBC

> Java DataBase Connectivity

- Java를 활용하여 다양한 관계형 DB 작업을 수행할 때 사용되는 API

<img src="DB 08.assets/147056453-fdfedc31-ee31-420b-82cc-20fc3e5a454d.jpg">

<img src="DB 08.assets/image-20220529010121240.png">

<img src="DB 08.assets/image-20220529011835108.png">

`application.properties`, `application.yml` 등의 파일을 보면 위와 같이 적혀있는 것이 JDBC 설정을 한 것.

<br>

#### JDBC 동작 과정

- JDBC Driver Load
- DB 연결
- Statement 생성
- SQL문 전송
- 결과 받기
- 연결 해제

<img src="DB 08.assets/image-20220529005939196.png">

DB 연결을 할 때 위의 코드를 통해 작업을 진행해야 한다.

하지만 매 작업마다 짧지 않은 코드가 반복된다. 

~~Spring을 사용하고 있다면 위의 과정을 Spring이 대신 해주니 고마워합시다.~~

또한 사용자가 요청을 할 때마다 Driver Load -> Connection 객체 생성 -> DB 연결 -> 종료 및 객체 제거의 과정이 반복 되기 때문에 매우 비효율적.

이런 문제를 해결하기 위해 등장한 것이 **Connection Pool**

<br>

### Connection Pool

> 일정량의 Connection 객체를 미리 만들어 Pool에 저장
>
> 객체를 빌려주고 다시 반납받는 프로그래밍 기법

#### 특징

- Pool에 미리 Connection이 있기에 **<u>불필요한 작업이 생략</u>**되므로 빠른 DB 접속 가능.
- Connection 수를 제한하여 과도한 접속으로 인한 **<u>서버 자원 고갈 방지</u>** 가능
- DB 모듈을 공통화 함으로 인해 **<u>유지보수 유리</u>**.
- 만들어둔 Connection을 **<u>재사용</u>**하는 것이기에 **<u>적절한 Pool의 크기</u>**를 정하는 것이 중요.

<br>

#### 유의사항

> 동시 접속자 수는 상대적인 개념이므로, 자신의 서버 상태에 맞게 판단

- 동시 접속자가 적을 경우?
  - DB가 부하를 견딜 수 있으므로 신경쓰지 않아도 됨.
- 동시 접속자가 많을 경우?
  - Connection의 수는 정해져 있기에 너무 많은 DB 접근이 발생하면 Connection이 반납될 때까지 기다려야 함.
    - 사용자는 1~2초 이내에 답이 오지 않으면 느리다고 판단.
  - Connection Pool의 크기를 너무 키우면 메모리를 많이 차지.
    - 성능 저하 우려.
  - 적절한 Connection Pool Size
    - connections = ((core_count * 2) + effective_spindle_count)
      - core_count : 현재 사용하는 서버 환경에서의 CPU 수
      - effective_spindle_count : 기본적으로 DB 서버가 관리할 수 있는 동시 I/O 요청 수
