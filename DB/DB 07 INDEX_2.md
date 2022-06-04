# DB #7 INDEX_2

### INDEX 자료구조

> Hash Table, B+Tree 등 존재
>
> 가장 보편적으로 사용되는 B-Tree

<br>

### B-Tree

> Balanced Tree

#### 특징

- 자식 노드의 수가 2개 이상
- 한 노드 내의 key가 1개 이상
- 항상 정렬된 상태 유지
- 이름에서 알 수 있듯 **O(logN)** 보장

<br>

<img src="DB 07 INDEX_2.assets/image-20220522173635481.png">

<br>

### INDEX 생성 및 조회

```sql
# MySQL 기준
# 인덱스 생성
CREATE INDEX [인덱스명] ON [테이블명](컬럼1, 컬럼2, ...);

# 인덱스 조회
SHOW INDEX FROM [테이블명];

# 인덱스 삭제
ALTER TABLE [테이블명] DROP INDEX [인덱스명];
```

