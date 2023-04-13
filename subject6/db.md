# Database, DBMS(Database Management System), RDBMS(Relational Database Management System)

데이터베이스란 여러 사람이 함께 데이터를 공유하며 사용할 목적으로 데이터를 체계적으로 구조화한 데이토 모음을 의미한다.
DBMS란 데이터베이스를 'CRUD'하고 접근 권한과 보안 기능까지 제어해주는 소프트웨어이다.
RDBMS는 행과 열로 이루어진 관계(Relation)로 데이터를 저장하여 데이터베이스를 관리하는 시스템, 소프트웨어를 의미한다.
MySQL, PostgreSQL 등이 있다. 
* 여기서 관계는 Relationship이 아님에 주의.

# 데이터베이스 언어

RDBMS는 데이터베이스를 정의하고, 수정하며, 조작하기 위한 수단으로서 데이터베이스 언어를 제공한다.
이는 SQL언어로써 DDL, DML, DCL로 각각 일컬어지며 각각의 주요 기능은 다음과 같다.

## DDL, DML, DCL

DDL -> 데이터를 정의하고, 수정하거나 삭제하는 역할을 함.
DML -> 데이터를 수정하거나 삭제, 검색하는 역할을 함.
DCL -> 데이터 접근 권한을 부여하거나, 되돌리거나, 수정하는 역할을 함.

# 관계형 모델

## Attribute(속성)

이름과 타입으로 이루어지며 관계의 컬럼(열)에 위치한다. (Name/Varchar, age/int, married/true)

## Tuple(튜플)

속성과 값을 의미하며 관계의 row(행)에 위치한다. {(Name/Varchar, jack), (age/int, 20), (married/true, true)}
값은 속성이 제시한 타입을 충족해야한다.

## 관계(Relation)

튜플의 집합을 의미한다. 즉, 속성과 값을 가지고 있는 Tuple이 모여 하나의 테이블, 관계를 구성하는 것이다.
관계를 CRUD, merge 등 여러 연산을 하고 싶을 때 사용하는 것이 관계대수이다.

## 관계대수

### Projection

관계에서 특정 속성만을 뽑아내는 것이다.

```

// people 관계에서 나이와 키의 속성만을 추출

select name, height
from people;

```

### Selection

관계에서 조건을 만족하는 튜플만을 추출하는 것이다.

```

// 키 180 넘는 사람의 이름과 키를 추출
select name, height
from people
where height>180;

```

### Cartesian Product

두 관계를 합쳐서 제 3의 관계를 생성해내는 연산이다. ,를 통해 카티션 곱이 가능하다.

```

// 결혼한 남자들의 이름과 키를 추출

select man.name, man.height
from man, married
where man.name = married.name and married.name = 'true';

```

# ER Model, ERD

ER Model은 Entity와 Realtionship으로 데이터베이스를 구축해나가는 것을 의미한다. 여기서 Entity란 고유하게 식별할 수 있는 사물을 의미한다. 에컨대 account, order, product 등 해당 데이터를 구분하는 기준을 Entity로서 명세한다.
Realtionship은 Entity 간의 관계를 의미한다. One to One, One to Many, Many to Many 등의 관계가 있어 데이터베이스 설계 시 명확히 고려해야 한다.

ERD는 ER모델을 다이어그램으로 도식화한 것을 의미한다. 설계 단계에서 데이터베이스의 관계를 명시하는데 도구가 되곤 한다.
