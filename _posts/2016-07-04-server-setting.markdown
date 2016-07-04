---
title: "CentOS 7에 MariaDB 설치하기"
layout: post
---

### 설치  
`sudo yum install mariadb`  

클라이언트만 설치되고 서버는 설치되지 않을 수 있음. 그럴 경우  
`sudo yum install mariadb-server`  
명령어를 사용해 서버를 설치.
  
### 환경설정  
기본적으로 설치되는 환경에서는 한글이 제대로 나오지 않으며, 또 원격접속을 위해서도 별도의 세팅이 필요하다.  
`cp /usr/share/mysql/my-medium.cnf /etc/my.cnf`  
위 명령을 사용해 기본 세팅 복사.  
`sudo vi /etc/my.cnf`로 파일을 열어서 [mysqld]를 찾아서 아래에 환경 추가.  
```
[mysqld]
character-set-server=utf8
collation-server=utf8_general_ci
```  


### 서버 실행  
`service mariadb start`  


### root 비밀번호 변경  
`mysqladmin password`  
위 명령을 입력하면 변경할 비밀번호를 두번 입력하도록 유도된다.  


### 원격 접속을 위해 유저 추가하기  
`insert into mysql.user (host, user, password) values ('%', 'root' password('password'));`모든 아이피에서 root계정으로 접속 가능.    
`grant all privileges on *.* to 'root'@'%';` 모든 권한 추가.  
`flush privileges;` 추가된 권한 반영.  

### 원격 접속을 위해 설정 추가하기
위에서 수정했던 my.cnf파일의 [client] 부분을 찾아서 아래 내용 추가.  
```
bind-address = *
```


### 변경사항 반영 위해 디비 재시작  
`sudo systemctl restart mariadb`  

