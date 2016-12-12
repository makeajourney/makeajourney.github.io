---
title : "MySQL user 관리"
layout : post
---


### user 보기  

```sql
use mysql;
select host, user, password from user;
```

```sql
USE mysql;
select * from user;
```


<br/>

### user 추가  

```sql
create user '<user_id>';
create user '<user_id>'@'localhost' identified by '<password>';
create user '<user_id>'@'%' identified by '<password>';
```

```sql
insert into user(host, user, password) values ('localhost', '<user_id>', password('<password>'));
insert into user(host, user, password) values ('%', '<user_id>', password('<password>'));
flush privileges;
```


<br/>

### user 삭제  

```sql
drop user '<user_id>@localhost;'
```

```sql
delete from user where user='<user_id>';
```


<br/>

### user에게 db 사용 권한 부여  

```sql
grant all privileges on <DB명>.<테이블> to '<user_id>'@'host'[ identified by '<password>'];
grant all privileges on <DB명>.* to '<user_id>'@'localhost';
grant all privileges on <DB명>.* to '<user_id>'@'%';
grant select, insert, update on <DB명>.* to '<user_id>'@'localhost';

grant all privileges on *.* to '<user_id>'@'localhost';
```  


- 변경 내용 반영  

```sql
flush privileges;
```


<br/>

### 부여된 권한 확인

```sql
show grants for '<user_id>'@'<host>';
```


<br/>

### 사용 권한 제거

```sql
revoke all on <DB명>.<테이블> from '<user_id>'@'<host>';
```
