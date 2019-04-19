---
title: "PostgreSQL을 사용하는 어느 백엔드 개발자의 반성?"
layout: post
---

# PostgreSQL

PGadmin4를 사용하게 되면서, 툴에 익숙해지기 위해 기본적인 명령어들을 실행해보는 것 부터 시작하기로 했다.  
별 생각 없이 `SHOW TABLES;`를 실행했는데 에러만 자꾸 출현하고 테이블 리스트가 나오지 않는다.  
뭔가 이상했다. 분명히 `user@db/dbname`으로 SQL editor가 실행된 것도 확인했는데 어째서...!  
그저 내가 PGAdmin에 익숙하지 않아 그런것이라 생각해서 동료 개발자에게 PGAdmin이 이상하다고 물었더니,  

> postgresql하고 mysql하고 좀 많이 달라요  
> table 리스트 얻어오는 쿼리는 보통  
> ```SELECT * FROM pg_catalog.pg_tables;```
> 요렇게 합니다

라는 답변이 돌아왔다. 나의 무지가 이렇게 드러나다니..!  
이전 직장에서도 PostgreSQL(이하 PG)을 사용했는데, 난 왜 이렇게 PostgreSQL을 잘 모르고 있나 하는 생각이 들었다.  


1. 너무 편리한 IDE에 익숙했다.

IntelliJ로 대표되는 요즘 잘 나가는 IDE들은 Database의 연결을 확인하고 데이터를 조회할 수 있는 기능을 가지고 있다.  별도로 MySQLWorkbench 같은 걸 쓸 필요가 없다.  
연결만 잘 시켜둔다면, DB의 상태도 확인 가능하고 쿼리도 IDE를 벗어나지 않고 날릴 수 있다.  
나는 RubyMine을 사용하는 동안 USE DATABASE 혹은 SHOW TABLES와 같은 쿼리를 날려본 일이 없다.  


2. ORM을 사용한다.

PG나 MySQL이나 기본적인 DDL, DML은 비슷하다. ANSI Standard SQL이 있고, 세부적으로 자세한 내용들이야 다르지만  기본적으로는 대부분의 Open Source Relational Database들이 이를 따르고 있다.  
근데 사실 DDL은 확신하지 못하겠다. 생각해보니 나는 처음 PG를 쓸 때 부터 DDL은 ORM level에서 다 해결했기 때문에, Raw SQL을 날려본 일이 없다. 보통 Raw Query를 작성하게 될 때는, 내가 작성한 쿼리가 어느정도의 성능을 가질것인가를 평가할 때, 의도대로 동작하는지 확인할 때 정도를 대표적으로 얘기할 수 있을 것 같다.  

ORM의 장점이야 너무 많다. 훌륭한 의약품에는 부작용도 존재할 수 있는 것 처럼, 훌륭한 ORM을 사용해서 빠른 개발 혹은  상태 관리의 편리성 등 여러가지 장점을 이용하고 있지만, 정작 기본적으로 내가 사용하고 있는 DB의 특징이나 사용법을 간과하게 되는 경우가 종종 생긴다.  

예전 직장에서 DBA 경력이 있으신 데이터분석팀원과 얘기하면서, MySQL과 PG의 CRUD 연산시 레코드를 처리하는 방식의 차이점을 들은 적이 있다.  
나의 경우에는 데이터베이스로서 MySQL을 처음 접했고, 학부에서도 이를 기준으로 배웠다보니(사실은 MySQL이 아니라 ISAM) 기본적으로 다 이렇게 동작할거라고 생각했었던 것 같다. 사실은 MySQL 조차 다 같은 엔진을 사용하는 것이 아님에도 말이다. 


사실은 얼마나 깊이 없는 개발을 해왔던가 반성하면서, PG와 좀 더 친해져보기로....  



## PostgreSQL features highlights

PostgreSQL은 다른 enterprise DBMS가 제공하는 것 처럼, 아래와 같은 많은 훌륭한 기능을 제공합니다.  

- 사용자 정의 타입
- Table 상속
- 세련된 locking mechanism
- 외래키 참조 무결성
- Views, rules, subquery
- 트랜잭션 중첩 (savepoints)
- 다버전 동시성 제어 (MVCC)
- 비동기 복제

최신 버전의 PostgreSQL은 아래와 같은 기능들을 지원합니다. (8.0 기준)  

- Native Microsoft Windows Server version
- Tablespaces
- 특정시점 복구

<http://www.postgresqltutorial.com/what-is-postgresql/>  
