---
title : "mysql의 default now()를 mariadb에 적용하려면?"
layout : post
---

#### mysql에서 현재 날짜와 시각 입력  
mysql에서 날짜와 시각을 입력 할 때  
```
CREATE TABLE '테이블명' (
  '필드명' DATETIME NOT NULL DEFAULT NOW()
  );
```  
를 많이 사용한다.  
이렇게 DB TABLE SCHEMA를 생성하면 INSERT가 있을 때 마다 해당 필드에 값을 쓰지 않아도 저절로 INSERT할 당시의 시각을 써주는 장점이 있다.  
Mariadb를 쓰면서 이와 똑같이 사용하려고 했더니, 테이블 생성이 불가했다. 알고보니 도메인이나 사용하는 DEFAULT의 값이 조금 달랐다.  


#### mariadb에서 현재 날짜와 시각 입력  
```
CREATE TABLE '테이블명' (
  '필드명' TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
  );
```  
를 사용하면 mariadb에서도 INSERT시에 자동으로 현재 날짜와 시각을 해당 필드에 삽입해준다.  
mysql과 mariadb의 문법이 기본적으로 비슷하다보니, mariadb를 쓰면서도 mysql을 쓰고 있는 느낌이 들어서 이런 문제에 부딪칠 때 자꾸 mysql로 검색하고 있다.....   
