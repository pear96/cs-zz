# DB #5

### SQL Injection

> 이용자의 입력값이 SQL 구문의 일부로 사용될 경우
>
> 조작된 SQL 구문이 DB에 그대로 전달되어 **<u>비정상적인 명령</u>**을 실행시키는 공격 기법

<br>

### 공격 목적

> SQL Injection의 공격 목적

- **인증 우회**
  - SQL Injection 공격의 대표적인 경우
  - 보통 Login Form에서 공격 수행
  - 아이디, 비밀번호 입력할 때 **다른 쿼리문을 함께 입력**
  - 정상 계정 정보가 아니더라도 인증 획득 가능
- **DB 데이터 조작 및 유출**
  - **에러 메시지**를 이용하여 공격하는 방법
  - 악의적인 구문을 삽입하여 발생한 에러 메시지를 통해 DB 구조 유추
  - 조작된 Query가 실행되어 DB 데이터에 접근 / 조작 가능

- **시스템 명령어 실행**
  - 일부 DB의 경우 확장 프로시저를 호출하여 **시스템 명령어** 실행

<br>

### 공격 기법

> SQL Injection은 최소한 다음의 조건을 충족할 경우 발생 가능
>
> 1. 애플리케이션이 DB와 연동
> 2. 외부 입력값이 쿼리문으로 사용

- **SQL Injection**

  > 대상 웹페이지가 결과 리스트를 제공하거나, 오류 메시지를 출력할 경우 사용

  - WHERE 구문 우회

    > WHERE 조건이 무조건 참이 되도록 쿼리를 조작

    ```mysql
    "SELECT * FROM Users WHERE UserID='"+ userId +"' AND Password='" + password + "'"
    
    userId: admin' #
    password: ssafy
    
    SELECT * FROM Users WHERE UserID='admin' #' AND Password='ssafy'
    ```

    `#` 이후로 주석 처리가 되어 `admin` 계정의 비밀번호를 몰라도 인증을 받을 수 있게 된다.
    <br>
    
    ```mysql
    userId: admin
    password: ssafy' OR '1' = '1
    
    SELECT * FROM Users WHERE UserID='admin' AND Password='ssafy' OR '1'='1'
    ```
    
    WHERE 문을 항상 참이 되도록 하는 SQL 구문을 입력하여 다른 비밀번호를 입력하여도 인증을 받을 수 있게 할 수도 있다.
    
    <br>
    
    ```mysql
    userId: admin'; DELETE FROM Users #
    password: ssafy
    
    SELECT * FROM Users WHERE UserID='admin'; DELETE FROM Users #' AND Password='ssafy'
    ```
    
    `;` 을 사용하면 한 줄에 여러 명령어를 사용할 수 있는 점을 이용하여 위와 같이 테이블 전체를 날려버릴 수 도 있다.
    
    <br>
  
    - 고의적 에러 유발 후 정보 획득
  
      > 고의적으로 SQL 구문 에러를 발생시켜 에러 메시지를 기반으로 정보를 알아냄
  
      ```mysql
      userId: admin' UNION SELECT 1 #
      password: ssafy
      
      SELECT * FROM Users WHERE UserID='admin' UNION SELECT 1 #' AND Password='ssafy'
      ```
  
      UNION을 하기 위해선 두 테이블의 컬럼의 수가 같아야 한다.
  
      1의 갯수를 점차 늘려가며 테이블의 컬럼의 수를 유추할 수 있다.
  
      MySQL의 경우 `information_schema`의 `TABLES`를 통해 원하는 테이블과 UNION하여 데이터를 획득할 수 있다.
  
      <br>

- **Blind SQL Injection**

  > 해당 페이지가 오류, 결과 리스트도 출력해주지 않을 경우 시도하는 공격 기법
  >
  > 쿼리 결과의 **참 / 거짓**으로부터 DB값 유출
  >
  > 아이디찾기, 게시판 검색과 같은 기능에서 자주 사용

  ```mysql
  # 아이디찾기
  userId: admin' AND '1'='1' #
  ```

  이런식의 쿼리문을 이용하여 해당 아이디가 존재하는지, 비밀 번호가 무엇인지 확인 가능

​		<br>

### 방어 기법

- 웹 방화벽 도입

  > Rule에 따라 제어. Rule 설정을 통해 SQL Injection 대비 가능

- 유효성 검사

  > **Never Trust User Input**

  - 블랙리스트
    - 특수문자, 예약어, 함수 등의 키워드를 **블랙리스트**로 등록하여 사용하지 못하게 함.
    - 금지된 문자 이외는 허용
  - 화이트리스트
    - 블랙리스트보다 강화된 기법
    - 허용된 문자 이외 불가능
    - 정규식을 이용하여 패턴화 시키는 것이 유지보수에 유리

- 오류 메시지 감추기
  - 오류 메시지를 통해 DB 구조 파악이 가능할 수 있음
  - 너무 자세한 안내 메시지 피하기
    - 로그인 실패 시 ID가 틀렸는지, PW가 틀렸는지까지 구체적으로 알려줄 필요 x

- 지속적으로 취약점 점검
  - 정기적으로 SQL Injection에 취약한지 점검
  - 모의 해킹 등을 활용하여 검사하는 것을 권장
- 로깅
  - SQL Injection의 특성상 많은 오류를 발생시키거나 동일한 페이지 / 기능을 반복적으로 호출
  - 500 에러 등이 많이 발생하거나, 동일 IP에서 동일한 페이지 / 기능을 반복 호출할 경우 주의깊게 살펴볼 필요 O



### 여기 한 번 읽어보세요

https://www.invicti.com/blog/web-security/sql-injection-cheat-sheet/