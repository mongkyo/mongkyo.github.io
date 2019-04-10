---
title: "2018.09.30 - SQL-Database"
categories: 
  - Dev
tags:
    - mac
    - SQL
    - Database
author_profile: true

toc: true
toc_label: "My Talbe of Contents"
toc_icon: "cog"
toc_sticky: true

last_modified_at:
comments: true
---

# SQL-Database

### SQL CREATE DATABASE 

CREATE DATABASE 구문은 새로운 SQL 데이터베이스를 만들 때 사용한다.

__CREATE DATABASE Syntax__:

	CREATE DATABASE databasename;
	
### SQL DROP DATABASE 

DROP DATABASE 구문은 기존에 있던 데이터베이스를 삭제하는 데 사용한다.

__DROP DATABASE Syntax__:

	DROP DATABASE databasename;
	
### SQL CREATE TABLE

CREEATE TABLE 구문은 새로운 테이블을 생성할 때 사용한다.

__CREEATE TABLE Syntax__:

	CREATE TABLE table_name (
	    column1 datatype(character),
	    column2 datatype,
	    column3 datatype,
	    ...
	);
	
> column 매개 변수는 테이블의 열 이름을 지정한다.
> datatype 매개 변수는 열에서 보유 할 수 있는 데이터 유형(예 : varchar, 정수, 날짜 등)을 지정한다.
> column1의 괄호 안에 character는 들어갈 수 있는 최대 문자열을 나타낸다

### SQL DROP TABLE

DROP TABLE은 데이터베이스 내에 존재하는 테이블을 삭제한다.

__DROP TABLE Syntax__:

	DROP TABLE table_name;
	
### SQL ALTER TABLE

- ALTER TABLE은 기존 테이블의 열을 추가, 삭제 또는 수정하는 데 사용한다. 
- ALTER TABLE은 또한 기존 테이블에 존재하는 다양한 제약들을 추가하거나 제거하는 데 사용한다.

__ALTER TABLE Syntax__:

```
# ALTER TABLE - ADD Column

ALTER TABLE table_name
ADD column_name datatype;


# ALTER TABLE - DROP Column

ALTER TABLE table_name
DROP COLUMN column_name;


# ALTER TABLE - ALTER/MODIFY Column

ALTER TABLE table_name
ALTER COLUMN column_name datatype;
```

### SQL Constraints

SQL Constraints는 테이블의 데이터에 대한 규칙을 지정하는 데 사용된다. 이 제약 조건은 테이블에 들어갈 수 있는 데이터 유형을 제한할 수 있다. 이렇게 하면 표의 데이터 정확성과 신뢰성이 보장된다.

- SQL Create Constraints

CREATE TABLE 으로 테이블이 작성이 되거나 ALTER TABLE 으로 테이블이 작성 된 후에 제한 조건을 넣을 수 있다. 

SQL에는 일반적으로 아래의 표와 같은 제약조건들이 있다.

sort | content
----|----
NOT NULL | column이 NULL값을 가질 수 없음을 보장
UNIQUE | column안의 모든 값들이 다르다는 것을 보장
PRIMARY KEY | NOT NULL과 UNIQUE의 조합, 테이블의 각 행을 고유하게 식별
FOREIGN KEY | 다른 테이블의 행과 레코드를 고유하게 식별
CHECK | column안에 모든 값들이 특정한 조건을 만족
DEFAULT | 값이 지정되지 않은 경우 column의 기본값을 설정
INDEX | 데이터베이스 내에서 데이터를 빠르게 생성하고 검색하는 데 사용

__Constraints Syntax__:

```
CREATE TABLE table_name (
    column1 datatype constraint
    column2 datatype constraint
    column3 datatype constraint
    ...
)
```

### SQL NOT NULL Constraint

기본적으로 열(column)은 NULL값을 포함 할 수 있다. NOT NULL 제약 조건은 열이 NULL값을 허용하지 않도록 강제한다. 이렇게 되면 필드에 항상 값이 포함 될 수 있다. 즉 새 레코드를 삽입하거나 필드에 값을 추가하지 않고 레코드를 업데이트 할 수 있다.

> 테이블이 이미 생성된 경우 ALTER TABLE으로 NOT NULL 제약 조건을 추가할 수 있다

### SQL UNIQUE Constraint

UNIQUE 제약 조건은 각 열(column)의 모든 값들이 서로 다른지 확인한다. UNIQUE 및 PRIMARY KEY 제약 조건은 열 또는 열 집합의 고유성을 보장한다. PRIMARY KEY 제약조건은 자동적으로 UNIQUE 제약 조건을 갖는다. UNIQUE 제약조건은 한 테이블안에서도 많이 가질 수 있지만 PRIMARY KEY 제약 조건은 테이블 당 한개만 가질 수 있다. 

### SQL PRIMARY KEY Constraint

PRIMARY KEY 제약 조건은 데이터베이스 테이블의 각 레코드를 유니크하게 구분한다. 기본 키는 UNIQUE 값을 포함해야하고 NULL값을 포함 할 수 없다. 테이블은 한개의 기본키만을 가져야 한다. 이 기본키는 하나 또는 여러 필드로 구성 될 수 있다.

### SQL FOREIGN KEY Constraint

FOREIGN KEY는 두 테이블을 서로 연결하기 위해 사용된다. 이 FOREIGN KEY는 다른 테이블의 PRIMARY KEY를 참조하는 테이블의 필드(또는 필드의 모음)이다. 이 FOREIGN KEY가 들어있는 테이블을 하위 테이블이라 하고 후보 키가 들어있는 테이블을 참조된 테이블 또는 상위 테이블이라고 한다. 

### SQL CHECK Constraint

CHECK 제약 조건은 열(column)에 배치 할 수 있는 값 범위를 제한하기 위해 사용된다. 한개의 열에 CHECK 제약 조건을 정의하면 이 열에 대해 특정 값만 허용된다. 만약 한 테이블에 CHECK 제약 조건을 정의 한다면 그 행에 있는 다른 열의 값들에 기초해서 어떤 열에 있는 값들을 제한 할 수 있다. 

### SQL DEFAULT Constraint

DEFAULT 제약 조건은 열의 기본값을 제공하기 위해 사용된다. 다른 값을 지정하지 않으면 default값이 모든 새 레코드에 추가된다.

### SQL CREATE INDEX

CREATE INDEX 꾸문은 테이블에 인덱스들을 생성하기 위해 사용한다. 인덱스는 데이터베이스 안에서 데이터를 빠르게 검색하기 위해 사용한다. 사용자가 인덱스들을 볼수는 없지만 쿼리 속도를 높이기 위해 사용한다. 

> 인덱스를 사용하여 테이블을 업데이트 하는 것은 인덱스를 사용하지 않고 테이블을 업데이트하는 것 보다 시간이 더 많이 소요된다.(왜냐면 인덱스들 또한 업데이트를 해야하기 때문) 따라서 자주 검색할 열에 대해서만 인덱스를 생성해야한다.

__CREATE INDEX Syntax__:

```
CREATE INDEX index_name
ON table_name (column1, column2, ...);
```

__CREATE UNIQUE INDEX Syntax__:

```
CREATE UNIQUE INDEX index_name
ON table_name (column1, column2, ...);
```

### SQL AUTO INCREMENT Field

Auto-Increment는 새 레코드가 테이블에 삽입 될 때 고유 번호가 자동으로 생성된다. 때때로 이것은 새로운 레코드가 삽입될 때마다 자동으로 생성되는 PRIMARY KEY 필드가 된다.

### SQL Working With Dates

- __SQL Dates__

날짜 작업을 할 때 가장 힘든 부분은 데이터에 삽입하려는 날짜 형식이 데이터베이스의 날짜 열 형식과 일치하는지 확인하는 것이다. 데이터에 날짜 부분만 포함되어 있으면 쿼리가 정상적으로 작동한다. 하지만 시간이 포함될 경우에는 좀 더 복잡해진다. 따라서 검색어를 간단하고 쉽게 유지하기 위해서는 날짜에 시간 구성 요소를 넣으면 안된다. 