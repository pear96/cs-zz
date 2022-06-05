# DB #6

### SQL vs NoSQL

> SQL : 관계형 데이터베이스 전용 프로그래밍 언어
>
> NoSQL : 관계형 데이터베이스를 제외한 나머지 모든 유형에서 사용하는 언어

<br>

### SQL

> 관계형 데이터베이스 : 고정된 열과 행을 가지고 있는 테이블
>
> 관계형 데이터베이스와 상호작용할 때 사용되는 언어

####  특징

- **스키마**
  - 테이블에 어떤 데이터가 들어갈지 **구조** 정의
  - 필드의 이름, 데이터 유형 등 정의
  - **<u>스키마를 준수하지 않는 데이터 추가 불가</u>**
- **관계**
  - **중복을 피해** 여러 개의 테이블에 나누어 데이터 저장
  - 각각의 테이블에서 부정확한 데이터를 다룰 위험 X
  - **<u>테이블간의 관계를 직관적으로 파악 가능</u>**

#### 종류

- Oracle
- MySQL
- SQLite
- MariaDB
- PostgreSQL

<br>

### NoSQL

> Non SQL / Not Only SQL
>
> 관계형 데이터베이스가 아닌 모든 데이터베이스

#### 특징

- **스키마, 관계 없음**
  - 스키마가 없다 : `Not Exist(X) Flexible(O)`
  - 데이터를 저장할 때 정해진 방식이 없다는 것은 아님.
  - 데이터를 읽어올 때에만 스키마 사용.


#### 종류

- **Key - Value**
  - Key - Value의 쌍으로 구성된 데이터의 배열
  - Redis, Dynamo, Cassandra
- **Document**
  - Key - Value와 유사
  - Json 형태로 **문서화**하여 저장
  - MongoDB
- **Graph**
  - 자료구조의 그래프와 비슷한 형식
  - 노드(Entity)를 저장하고, 관계(Edge)로 연결
  - SNS 같은 것을 만들 때 사용하면 좋다.
  - Neo4J

<br>

### 확장성

- 수직적 확장

  > 데이터베이스 물리적 성능 향상 (ex. 메모리, CPU 업그레이드)

  - SQL 데이터베이스에서 사용

- 수평적 확장

  > 서버 증설, 클라우드 서비스 이용을 통한 확장

  - NoSQL 데이터베이스에서 사용

<br>

### SQL vs NoSQL?

> 모든 것을 완벽하게 해결하는 솔루션 존재 X
>
> 각각의 장, 단점 / 목적 분석을 통해 필요에 맞는 것 선택

- SQL

  - 장점
    - 스키마의 정의로 인한 데이터 무결성 보장
    - 중복 없이 저장된 데이터
  - 단점
    - 유연성 부족
    - 복잡한 JOIN
    - 수직적 확장만 가능
  - 언제 사용?
    - 데이터의 수정이 많은 경우
    - 일관된 데이터를 사용하는 경우
    - 우리 수준에선 어지간한건 여기서 다 커버 가능.
- NoSQL

  - 장점
    - 애플리케이션이 원하는 형태로 데이터 저장
    - 읽기 속도가 빠름
    - 수평적 확장이 가능
  - 단점
    - 데이터 구조 결정이 어려움
    - 데이터의 중복이 가능하기에 수정이 오래 걸림
  - 언제 사용?
    - 데이터의 구조가 변경이 잦은 경우
    - 데이터의 수정이 많지 않은 경우
    - 엄청난 양의 데이터를 다뤄 데이터베이스를 수평으로 확장해야 하는 경우