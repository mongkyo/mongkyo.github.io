---
title: "SQL-Tutorial"
categories: 
  - Dev
tags:
    - mac
    - SQL
author_profile: true

toc: true
toc_label: "My Talbe of Contents"
toc_icon: "cog"
toc_sticky: true

last_modified_at:
comments: true
---


# SQL Tutorial
## [w3schools](https://www.w3schools.com/sql/) 

- 데이터를 가져올 때 

```
$ SELECT * FROM <가져올 데이터 테이블> 
```

- 관계형 데이터베이스(RDBMS) 

관계형 데이터베이스에 저장된 데이터베이스 객체들은 테이블이라고 불린다 이 테이블은 열과 줄로 구성된다 


Some of The Most Important SQL Commands

Commands | Means
---------|--------
SELECT | 데이터베이스에서 데이터를 추출한다
UPDATE | 데이터베이스안의 데이터를 업데이트 한다
DELETE | 데이터베이스에서 데이터를 삭제한다
INSERT INTO | 데이터베이스에 새로운 데이터를 삽입한다
CREATE DATABASE | 새로운 데이터베이스를 생성한다
ALTER DATABASE | 데이터베이스를 수정한다
CREATE TABLE | 새로운 테이블을 생성한다
ALTER TABLE | 테이블을 수정한다 
DROP TABLE | 테이블을 삭제한다 
CREATE INDEX | 인덱스(검색 키)를 생성한다 
DROP INDEX | 인덱스를 삭제한다

### SELECT문 

SELECT문은 데이터베이스에서 데이터를 선택하는 데 사용된다. 리턴 된 데이터는 결과 세트라고하는 결과 테이블에 저장된다

SELECT문에서 유일한 값을 가지고 싶을 때는 `SELECTA DISTINCT`를 사용한다 

`SELECT COUNT`는 뽑은 데이터의 수를 보여준다 

- SQL WHERE Clause

WHERE 절에서는 지정된 조건을 충족하는 레코드 만 추출한다

WHERE절에서 사용하는 Operators

Operator | Description
---------|------------
= | Equal
<> | Not Equal, 일부 SQL문에서는 `!=`로 쓰인다
> | 크다
< | 작다
>= | 크거나 같다
<= | 작거나 같다
BETWEEN | 특정범위 사이
LIKE | Search for a pattern
IN | 열에 특정한 여러 값들을 가지고 있나 

WHERE구문에서 AND연산자와 OR연산자

AND연산자는 주어진 조건들이 모두 TURE 경우에만 record를 보여준다
OR연산자는 주어진 조건들 중 하나만 TRUE 여도 record를 보여준다

NOT연산자는 조건이 TRUE가 아닐 경우 record를 보여준다 

이 연산자들은 () 기호를 사용해서 적절하게 우선순위를 설정할 수 있다

### ORDER BY 

ORDER BY Keyword는 record들을 오름차순과 내림차순으로 보여준다
기본적으로 ORDER BY는 오름차순으로 보여주기 때문에 내림차순으로 보고싶은 경우에는 DESC keyword를 같이 입력한다 

### SQL INSERT

두 가지 방법으로 넣을 수 있다 

1.

```
INSERT INTO table_name(column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```
> 입력하지 않은 테이블의 값으로는 NULL값이 들어간다 

2.

```
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

NULL 값은 비교 연산자를 사용하여 테스트를 할 수 없다. IS NULL이나 IS NOT NULL 연산자를 사용해야한다

### UPDATE, DELETE

UPDATE에서는 WHERE절이 업데이트 할 데이터의 조건을 말한다. 이 WHERE절을 넣지 않으면 모든 데이터가 수정된다
업데이트할 내용은 SET 구문을 활용한다 

> Note: Be careful when updating records in a table! Notice the WHERE clause in the UPDATE statement. The WHERE clause specifies which record(s) that should be updated. If you omit the WHERE clause, all records in the table will be updated!

```
UPDATE <TABLE NAME>
SET <변경 내용>
WHERE <condition>
```


DELETE도 UPDATE와 비슷하게 삭제할 조건을 WHERE절에 적어준다. 마찬가지로 이 WHERE절을 적지 않으면 모든 데이터가 삭제된다.
> Note: Be careful when deleting records in a table! Notice the WHERE clause in the DELETE statement. The WHERE clause specifies which record(s) should be deleted. If you omit the WHERE clause, all records in the table will be deleted!

```
DELETE FROM <table_name>
WHERE <condition>;
```

모든 데이터를 지우고 싶을 때는 두가지 방법을 사용한다 

```
1.
DELETE FROM table_name;

2.
DELETE * FROM table_name;
```


### SQL TOP, LIMIT or ROWNUM Clause
- SELECT TOP
SELECT TOP 절은 리턴 할 레코드 수를 지정하는데 사용된다. 모든 데이터베이스 시스템이 이 SELECT TOP절을 지원하는 것은 아니다. MySQL은 제한된 수의 레코드를 선택하기 위해 LIMIT 절을 지원하고 Oracle은  ROWNUM을 사용한다. 

> Note: Not all database systems support the SELECT TOP clause. MySQL supports the LIMIT clause to select a limited number of records, while Oracle uses ROWNUM.

__SQL Server__:

```
SELECT TOP number|percent column_name(s)
FROM table_name
WHERE condition;
```

__MySQL Syntax__:

```
SELECT column_name(s)
FROM table_name
WHERE condition
LIMIT number;
```

__Oracle Syntax__:

```
SELECT column_name(s)
FROM table_name
WHERE ROWNUM <= number;
```

### SQL MIN() and MAX() Functions

MIN() 함수는 선택된 column 중 가장 작은 값을 나타낸다
MAX() 함수는 선택된 column 중 가장 큰 값을 나타낸다

__MIN() Syntax__:

```
SELECT MIN(column_name) as <나타낼 값의 제목>
FROM table_name
WHERE condition;
```

__MAX() Syntax__:

```
SELECT MAX(column_name) as <나타낼 값의 제목>
FROM table_name
WHERE condition;
```

### SQL COUNT(), AVG() and SUM() Function

COUNT() 함수는 지정된 기준과 일치하는 행 수를 반환한다
AVG() 함수는 숫자열의 평균 값을 반환한다
SUM() 함수는 숫자열의 합을 반환한다

__COUNT Syntax__:

```
SELECT COUNT(column_name)
FROM table_name
WHERE condition;
```

__AVG() Syntax__:

```
SELECT AVG(column_name)
FROM table_name
WHERE condition;
```

__SUM() Syntax__:

```
SELECT SUM(column_name)
FROM table_name
WHERE condition;
```

### SQL LIKE Operator

LIKE 연산자는 WHERE절에서 열의 지정된 패턴을 검색하는 데 사용된다.
이 LIKE연산자에는 두개의 와일드 카드를 사용할 수 있다.

- % : 0, 1 또는 복수 문자를 나타낸다 (%a : a로 끝나는 문자열)
- _ : 밑 줄은 한 문자를 나타낸다

> AND와 OR 연산자를 사용하여 여러 조건을 결합 할 수 있다

### Wildcard Characters

와일드 카드 문자는 문자열의 다른 문자를 대체하는 데 사용된다. 이 와일드 카드 문자는 SQL LIKE연산자와 함께 사용된다. LIKE 연산자는 WHERE 절에서 열의 지정된 패턴을 검색하는 데 사용한다. 

SQL Serve에서는 아래의 방법을 사용 할 수 있다

- [charlist] - 일치시킬 문자와 세트의 범위를 정한다 
- [^ charlist] or [! charlist] - 일치하지 않는 문자의 집합과 범위를 정의한다

### SQL IN Operator

IN 연산자를 사용하여 WHERE 절에 여러 값을 지정 할 수 있다. 이 IN 연산자는 여러 OR 연산자를 간단하게 표현한 것과 같다

```
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1, value2, ...);

or:

SELECT column_name(s)
FROM table_name
WHERE column_name IN (SELECT STATEMENT):
```

### SQL BETWEEN Operator

BETWEEN 연산자는 주어진 범위 내의 값을 선택한다. 값은 숫자, 텍스트, 날짜 일 수 있다. BETWEEN 연산자에는 시작과 끝 값이 포함된다

```
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

### SQL Aliases

SQL Aliases 는 테이블과 열에 임시로 이름을 지정하여 사용할 수 있게 한다. 이 Aliases 기능은 query 기간 동안에만 존재한다
별칭에 공백이 있으면 " " or [ ] 를 사용한다
__Alias Column Syntax__

```
SELECT column_name AS alias_name
FROM table_name;
```

__Alias Table Syntax__

```
SELECT column_name(s)
FROM table_name AS alias_name;
```

### SQL Join

JOIN은 두 개 이상의 테이블에 있는 행을 결합하는 데 사용된다.

- Different Types of SQL JOINs


Types       | info
------------|-----------
INNER JOIN | 두 테이블에서 값이 일치하는 레코드를 반환한다
LEFT JOIN | 왼쪽 테이블에서 모든 레코드를 반환하고 오른쪽 테이블에서 일치하는 레코드를 반환한다
RIGHT JOIN | 오른쪽 테이블에서 모든 레코드를 반환하고 오른쪽 테이블에서 일치하는 레코드를 반환한다
FULL OUTER JOIN | 왼쪽 또는 오른쪽 테이블에서 일치하는 항목이 있으면 모든 레코드를 반환한다

![JOIN_IMAGE](https://github.com/mongkyo/SQL-Homework/blob/jungmongkyo/join.png)

### SQL INNER JOIN Keyword

INNER JOIN 키워드는 두 표에서 모두 일치하는 값을 가진 레코드를 선택한다.

__INNER JOIN Syntax__:

```
SELECT column_name(s)
FROM table1
INNER JOIN table2 ON table.column_name = table2.column_name;
```

### SQL LEFT JOIN Keyword

LEFT JOIN 키워드는 왼쪽 테이블(table1)의 모든 레코드와 오른쪽 테이블(table2)의 일치 레코드를 반환합니다. 일치하는 것이 없으면 오른쪽에서 결과가 NULL입니다.

__LEFT JOIN Syntax__

```
SELECT column_name(s)
FROM table1
LEFT JOIN table2 ON table1.column_name = table2.column_name;
```

### RIGHT JOIN Keyword

RIGHT JOIN Keyword는 오른쪽 테이블(table2)의 모든 레코드와 왼쪽 테이블(table1)의 일치 레코드를 반환합니다. 일치하는 경우가 없으면 왼쪽에서 결과가 NULL입니다.

__RIGHT JOIN Syntax__:

```
SELECT column_name(s)
FROM table1
RIGHT JOIN table2 ON table1.column_name = table2.column_name;
```

### SQL FULL OUTER JOIN Keyword

FULL OUTER JOIN Keyword는 왼쪽(table1)또는 오른쪽(table2)테이블 레코드가 일치 할 때 모든 레코드를 리턴한다. 
> FULL OUTER JOIN은 잠재적으로 매우 큰 집합 결과를 리턴 할 수 있다. 


## SQL Self JOIN

Self JOIN은 원하는 데이터들이 한 테이블에 있을 때 사용한다. 즉 테이블을 자기 자신과 조인시키는 것이다. 명령어가 따로 있는 것은 아니고, OUTER JOIN 이나 INNER JOIN등을 자기 자신과 JOIN하면 Self JOIN이 된다.

__Self JOIN Syntax__:

```
SELFECT column_name(s)
FROM table1 T1, table T2
WHERE condition;
```

### SQL UNION Operator

UNION 연산자는 두 개 이상의 SELECT 문의 결과 집합을 결합하는 데 사용된다. 
> UNION 연산자는 기본적으로 고유 값만 선택한다. 만일 중복 값을 허용하려면 UNION ALL을 사용해야 한다.

- UNION내의 각각의 SELECT문은 같은 수의 columns을 가져야한다.
- columns는 유사한 데이터 형식을 가져야 한다.
- 각각의 SELECT문의 columns는 같은 순서로 있어야 한다.

__UNION Syntax__:

```
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```
__UNION ALL Syntax__:

```
SELECT column_name(s) FROM table1
UNION all 
SELECT column_name(s) FROM table2;
```

### SQL GROUP BY Statement

GROUP BY Statement는 집계 함수 (COUNT, MAX, MIN, SUM, AVG)와 함께 사용되어 결과 집합을 하나 이상의 columns로 그룹화 한다.

__GROUP BY Syntax__:

```
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);
```

### SQL HAVING Clause

WHERE 키워드를 집계 함수와 같이 사용할 수 없기 때문에 HAVING Clause가 SQL에 추가되었다.

__HAVING Syntax__:

```
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);
```

### EXISTS Operator

EXISTS 연산자는 하위 쿼리에 레코드가 있는지 테스트하는 연산자이다.

EXISTS 연산자는 하위 쿼리가 하나 이상의 레코드를 반환하면 TRUE를 반환한다.

__EXISTS Syntax__:

```
SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);
```
