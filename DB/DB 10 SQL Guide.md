# DB #10 SQL Guide

### SQL Guide

> 기본적으로 사용되는 SQL 및 성능을 고려한 SQL 구문 작성 방법 제시
>
> 응용프로그램 소스 관리의 용이성 제공
>
> 응용프로그램 및 데이터베이스의 성능 면에서도 중요

<br>

### SQL 구문 이해

> 자주 사용하는 DML 기준으로 작성

#### 1. 기본 SQL 구문

1. **INSERT**

   > 데이터를 입력할 때 사용

   - Single Row 입력

   ```sql
   INSERT
     INTO TABLE_NAME
          (
           COLUMN_LIST,
           ...
          )
   VALUES (
           HOST_VARIABLE OR CONSTANT
          );
   ```
   
   - Multiple Row 입력
   
   ```sql
   INSERT
     INTO TABLE_NAME
          (
           COLUMN_LIST,
           ...
          )
   SELECT 구문;
   ```

<br>

2. **UPDATE**

   > 데이터를 수정할 때 사용

   ```sql
   UPDATE TABLE_NAME
      SET (COLUMN_LIST, ...) = (HOST_VARIABLE OR CONSTANT OR SELECT 구문)
    WHERE CONDITION;
   ```


<br>

3. **DELETE**

   > 데이터를 삭제할 때 사용

   ```sql
   DELETE
     FROM TABLE_NAME
    WHERE CONDITION;
   ```

<br>

4. **SELECT**

   > 데이터를 검색할 때 사용

   ```sql
   SELECT COLUMN_LIST,
   	   ...
     FROM TABLE_NAME ALIAS_NAME
    WHERE CONDITION
    GROUP BY COLUMN_LIST, ...
   HAVING CONDITION
    ORDER BY COLUMN_LIST, ...;
   ```

<br>

#### 2. Outer Join

> 어떤 집합을 기준으로 해서 조인되는 다른 집합과의 연결에 실패했더라도 그 결과를 추출하는 조인

```sql
SELECT A.NO, B.NO, B.CON
  FROM TABLE1 A, TABLE2 B
 WHERE A.NO = B.NO(+)
   AND B.CON(+) = '10';
```

- 조인 순서가 미리 정해지므로 **조인 순서**를 이용한 튜닝 **불가**
  - Outer Join은 가능한 <u>피하는 것 추천</u>
- Outer Join을 담당하는 테이블에 대한 모든 조건에 `(+)`기호가 붙어야 원하는 결과를 얻을 수 있다.
- `(+)`기호를 이용하여 **IN, OR**의 연산자를 이용하여 비교 **불가**
- `(+)`기호를 이용하여 **서브 쿼리**와 비교할 수 없다.

<br>

#### 3. IN / EXISTS

> 데이터의 존재 여부를 판단하기 위해 사용
>
> 같은 결과를 보여주나 내부 로직의 차이 존재
>
> 데이터 집합의 처리 범위가 **넓고**, **부분 범위 처리**가 필요한 경우 : `EXISTS`
>
> 데이터 집합의 처리 범위가 **좁고**, 결과 데이터 **집합이 작은 경우** : `IN`

- TAB1

| COL1 | YYMM |
| ---- | ---- |
| 1111 | 9801 |
| 2222 | 9801 |
| 3333 | 9802 |
| 4444 | 9804 |
| 5555 |      |
| 6666 | 9807 |
|      | 9809 |

- TAB2

| COL2 | YYMM |
| ---- | ---- |
| 1111 | 9801 |
| 2222 | 9801 |
| 3333 |      |
| 4444 | 9801 |
|      | 9809 |

- IN vs EXISTS

  ```sql
  /*
  	IN은 전체 범위 처리를 통하여 서브 쿼리의 조건을 만족하는
  	모든 데이터 집합을 구한 다음 메인 쿼리의 조건 처리
  */
  
  SELECT *
    FROM TAB1
   WHERE YYMM IN (SELECT YYMM
                 	  FROM TAB2);
                 	  
  /*  result
  	COL1	YYMM
  	1111	9801
  	2222	9801
  			9809
  */
  ```

  - IN절의 서브 쿼리 결과 -> 9801, NULL, 9809이지만
    NULL은 비교할 수 없는 값이기에 YYMM이 NULL인 데이터는 나오지 않는다.

  <br>

  ```sql
  /*
  	EXISTS의 서브 쿼리는 메인 쿼리가 찾은 데이터에 대해 하나씩 존재 여부를 판단
  	즉, 부분 범위 처리 가능
  	메인 쿼리가 먼저 수행되고 서브 쿼리는 존재 여부만 판단하고 다시 메인 쿼리로 돌아감
  */
  
  SELECT *
    FROM TAB1 A
   WHERE EXISTS (SELECT '1'
                   FROM TAB2 B
                  WHERE B.YYMM = A.YYMM);
                  
  /* result
  	COL1	YYMM
  	1111	9801
  	2222	9801
  			9809
  */
  ```

  - TAB1에서 추출된 YYMM 값에 대하여 TAB2의 YYMM값과 같은지 확인
  - NULL일 때는 서로 비교할 수 없으므로 YYMM이 NULL인 데이터가 비교대상에서 제외

<br>

#### 4. NOT IN / NOT EXISTS

> NOT IN : 전체 범위 처리
>
> NOT EXISTS : 부분 범위 처리
>
> 거의 대부분 같은 결과가 나오지만 **다른 결과**가 나올 수 있음
>
> - NOT IN 서브 쿼리 결과에 NULL이 있는 경우
> - NOT EXISTS 메인 쿼리의 결과가 NULL인 경우

- NOT IN / NOT EXISTS
  ```sql
  SELECT *
    FROM TAB1
   WHERE YYMM NOT IN (SELECT YYMM
                     	  FROM TAB2);
                     	  
  /* result
  	만족하는 결과 없음
  */
  ```

  - NOT IN의 서브 쿼리 결과 : 9801, 9809, NULL
  - TAB1.YYMM <> '9801' AND **TAB1.YYMM <> NULL** AND TAB1.YYMM <> '9809'
    가운데 조건이 항상 FALSE이므로 결과 출력 X

  ```sql
  SELECT *
    FROM TAB1 A
   WHERE NOT EXISTS (SELECT '1'
                       FROM TAB2 B
                      WHERE B.YYMM = A.YYMM);
                      
  /* result
  	COL1	YYMM
  	3333	9802
  	4444	9804
  	5555
  	6666	9807
  */
  ```

  - NOT EXISTS는 메인 쿼리에서 나온 YYMM 값으로 서브 쿼리에 존재 여부를 판단하여
    존재하지 않는 데이터를 가져 옴
  - TAB1의 YYMM의 값이 9801인 행이 TAB2에 존재하므로 리턴 X
  - TAB1의 YYMM의 값이 NULL인 경우, TAB2의 컬럼과 비교.
    NULL은 비교 대상이 아니므로 서브 쿼리의 결과가 FALSE
    -> 5555 리턴 O

<br>

### 작성 가이드

- 한 서비스 당 가능한 한 SQL 구문으로 작성
  - 여러 로직으로 처리하는 것보다 한 SQL 구문이 이해하기 쉬움
  - 유지보수 유리
  - Optimizer가 한 SQL 구문 단위로 최적화하기 때문에 성능 향상
    - **Optimizer**
    - 가장 효율적인 방법으로 SQL을 수행할 최적의 쿼리 경로를 생성해주는 DBMS의 핵심 엔진
    - **규칙 기반**, **비용 기반**이 존재

- Keyword는 정확히 알고 **목적**에 따라 적용

  - Outer Join

    > Outer Join에 대해 정확히 알고 작성.

  - NOT IN vs NOT EXISTS, IN vs EXISTS

    > 둘 사이의 정확한 차이점을 알고 작성

  - DISTINCT

    > 항상 Sort 연산을 수행하므로, 처리값 결과가 항상 Unique한 경우 사용 지양

  - OR

    > OR 사용 가급적 제한 추천

    - 단순한 OR의 경우 괜찮지만, 복잡한 OR의 경우 거의 Full Table Scan을 진행하므로 성능 저하
    - IN으로 사용할 수 있다면 IN으로 바꿔서 사용 추천
    - Full Table Scan이 되는 경우 UNION ALL 사용

  - UNION ALL

    > UNION보다 UNION ALL을 사용하여 Sort 연산의 오버헤드를 없앰.

  - IN

    > - 동적 IN을 처리하는 경우 (1, 5, 10, ...) 단위의 고정 SQL로 처리
    >   캐시를 활용하여 성능을 높일 수 있다.
    > - 인덱스를 사용하기는 하나 OR로 연결되는 구문이므로 list에 개수가 많으면 속도 저하 우려

- 적절한 INDEX 사용

  > 아래 경우에는 인덱스가 존재해도 사용 못함
  >
  > 주의 필요

  - 인덱스 컬럼에 **외부적 변형**이 가해질 때

  - 인덱스 컬럼에 **내부적 변형**(Type Conversion)이 가해질 때

  - **부정형**으로 비교된 조건을 사용할 때 `(!=, NOT IN)`

    - 인덱스 사용을 위해 부정형 조건보다 **긍정형 조건** 사용

  - **IS (NOT) NULL**로 비교된 경우

    - 인덱스에는 NULL에 대한 정보가 없으므로 Full Table Scan 발생

  - LIKE가 **Date / Number Type**의 컬럼에 사용된 경우

    - LIKE 연산자는 Character Type이 아닌 경우 해당 컬럼을 Conversion 하므로 인덱스 사용 불가

  - LIKE **'%text%'** 또는 LIKE **'%text'**

    - LIKE는 '%'로 시작하는 비교값이 오면 인덱스 사용 불가

  - **결합 인덱스** 사용

    - A+B+C+D로 구성된 결합 인덱스에서
      ```sql
      WHERE A=:A AND B LIKE ':B%' AND C=:C AND D=:D
      WHERE A=:A AND B BETWEEN :B1 AND :B2 AND C=:C AND D=:D
      ```

      조건이 위와 같다면 **LIKE / BETWEEN**이 사용된 컬럼까지의 **인덱스(A + B)만 유효**

  - 레코드 필터링을 위해 **HAVING** 보단 **WHERE**
    - HAVING절의 컬럼에 대한 인덱스 사용 X
    - HAVING절은 GROUP BY에 의해 그룹핑된 데이터를 줄여주는 역할
    - WHERE 조건에 의해 인덱스를 사용하여 데이터를 가져온 후 그룹핑 하는 것 추천

  <br>

  - 예시

    | 인덱스 사용 X                                                | 인덱스 사용 O                                                |
    | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | SELECT DEPT, ENAME, SAL<br />  FROM EMP<br />WHERE SAL * 12 < 5000; | SELECT DEPT, ENAME, SAL<br />  FROM EMP<br />WHERE SAL < 5000 / 12; |
    | - DEPT 컬럼이 Char Type인 경우<br /><br />SELECT DEPT, ENAME, SAL<br />  FROM EMP<br />WHERE DEPT = 7788; | SELECT DEPT, ENAME, SAL<br />  FROM EMP<br />WHERE DEPT = TO_CHAR(7788); |
    | SELECT EMPNO, ENAME<br />  FROM EMP<br />WHERE EMPNO != '1234'; | SELECT EMPNO, ENAME<br />  FROM EMP<br />WHERE EMPNO > '1234'<br />       OR EMPNO < '1234'; |
    | SELECT DEPT, ENAME, SAL<br />  FROM EMP<br />WHERE EMPNO <> 123; | SELECT DEPT, ENAME, SAL<br />  FROM EMP<br />WHERE NOT EXISTS (SELECT 'x'<br />                                       FROM EMP<br />                                     WHERE EMPNO = 123); |
    | SELECT DEPT, ENAME, SAL<br />  FROM EMP<br />WHERE DATE IS NOT NULL; | SELECT DEPT, ENAME, SAL<br />  FROM EMP<br />WHERE DATE > 0; |
    | SELECT DEPT, ENAME, SAL<br />  FROM EMP<br />WHERE JOB LIKE '%ALE'; | SELECT DEPT, ENAME, SAL<br />  FROM EMP<br />WHERE JOB LIKE 'SALE%'; |
    | SELECT DEPT, SUM(SAL)<br />  FROM EMP<br />GROUP BY DEPT<br />HAVING DEPT = 100; | SELECT DEPT, SUM(SAL)<br />  FROM EMP<br />WHERE DEPT = 100<br />GROUP BY DEPT; |

    

