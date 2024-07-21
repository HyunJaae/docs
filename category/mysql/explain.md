---
description: MySQL EXPLAIN 명령어에 대해 알아봅시다.
---

# EXPLAIN

`EXPLAIN`문은 MySQL 이 명령문을 실행하는 방법에 대한 정보를 제공합니다. 즉, 쿼리 실행 계획에 대한 정보를 제공합니다. 아래는`integration`이라는 테이블을 조회하는 쿼리에 대한 EXPLAIN 문과 그 결과입니다.

```sql
EXPLAIN  
SELECT *  
FROM 테이블 a;
```

<figure><img src="../../.gitbook/assets/스크린샷 2024-01-29 오전 12.32.43.png" alt="EXPLAIN command result"><figcaption><p>EXPLAIN 명령어 결과</p></figcaption></figure>

각 컬럼에 대해 자세히 알아보겠습니다.

### id

`SELECT` 식별자로 쿼리 내 `SELECT` 명령어의 일련번호와 같습니다.

### select\_type

| Select\_type    | Meaning                                                                 |
| --------------- | ----------------------------------------------------------------------- |
| SIMPLE          | `UNION` 또는 서브 쿼리를 사용하지 않은 단순 `SELECT` 인 경우                              |
| PRIMARY         | 가장 상위의 `SELECT`                                                         |
| UNION           | `UNION` 구문에서 두 번째 이후로 나오는 `SELECT`                                      |
| DEPENDENT UNION | `UNION` 구문에서 두 번째 이후로 나오는 `SELECT` 이면서 외부 쿼리의 결과나 조건에 따라 영향을 받아 결정되는 경우 |
| UNION RESULT    | `UNION` 구문의 결과                                                          |

* **`DEPENDENT UNION` 예시**

```sql
SELECT column1, column2
FROM table1
UNION
-- 두 번째 SELECT 문은 첫 번째 SELECT 문의 결과에 의존합니다.
SELECT column3, column4
FROM table2
WHERE column5 = (
    SELECT column6 FROM table1 WHERE condition
    );
```



Ref : [https://dev.mysql.com/doc/refman/8.0/en/explain-output.html#explain-output-columns](https://dev.mysql.com/doc/refman/8.0/en/explain-output.html#explain-output-columns)
