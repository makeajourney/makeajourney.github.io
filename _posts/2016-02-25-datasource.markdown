---
title: "DataSource"
layout: post
---


### DataSource
- DriverManager와 유사한 기능 수행.  
- 웹 어플리케이션에 독립적.  
- Connection과 Statement 객체 풀링.  
- 분산 트랜잭션 수행 가능.  
- 자체적으로 커넥션 풀 기능 구현.  

<br>

### DataSource 적용

<br>

#### DataSource 구현체 준비
* <http://commons.apache.org> > Components > DBCP > Downloads > latest Release > `commons-dbcp-1.4-bin.zip`  
* <http://commons.apache.org> > Components > Pool > Downloads > `commons-pool-1.6-bin.zip`  
위 두 파일을 다운로드 받아 압축을 해제한 후, 압축이 해제된 jar 파일을 `프로젝트폴더/WebContent/WEB-INF/lib` 폴더에 복사.  

<br>

#### DataSource 사용
- BasicDataSource 인스턴스 변수 선언.
```
BasicDataSource ds;
```

- BasicDataSource 객체 생성.
```
ds = new BasicDataSource();

// sc : ServletContext
ds.setDriverClassName(sc.getInitParameter("driver"));
ds.setUrl(sc.getInitParameter("url"));
ds.setUsername(sc.getInitParameter("username"));
ds.setPassword(sc.getInitParameter("password"));
```

- DAO 객체에 DataSource 주입.
```
DAO-class-name.setDataSource(ds);
```

- Connection close
```
  try { if (ds != null) ds.close(); } catch (SQLException e) {}
```

<br>

#### DataSource 사용
- DataSource 인스턴스 선언
```
Datasource ds;
```

- DataSource Setter
```
public void setDataSource(DataSource ds) {
	this.ds = ds;
}
```

- connection 객체를 가져옴.
```
connection = ds.getConnection();
```

- connection 객체 반납.
```
try {if (connection != null) connection.close(); } catch(Exception e) {}
```
여기서 커넥션을 close()하면 실제로 커넥션을 해제하는 게 아니라, Proxy Object가 커넥션 객체를 커넥션 풀로 반납.

<br>
---


### Server에서 제공하는 DataSource 사용
- tomcat 실행환경 폴더에서 context.xml 파일 수정.  
파일 경로 : Servers > Tomcat v7.0 Server at localhost-config > context.xml  
- <Context> tag 안에 <Resource> 태그 추가.  

```
<Resource name="jdbc/db-name" auth="Container" type="javax.sql.DataSource" 
	maxActive="10" maxIdle="3" maxWait="10000" 
	username="username"
	password="password"
	driverClassName="com.mysql.jdbc.Driver" 
	url="jdbc:mysql://localhost/db-name" 
	closeMethod="close"/>
```

name : JNDI 이름  
auth : 자원 관리의 주체. Application / Container  
type : 자원의 타입 지정. fully-qualified java class name 필요.  
driverClassName : JDBC 드라이버 클래스의 이름.  
url : db connection url  
username : db user name  
password : db user password  
maxActive : DataSource로부터 꺼낼 수 있는 커넥션의 최대 개수. (Default : 8)  
maxIdle : DataSource로부터 유지할 수 있는 사용되지 않는 커넥션의 최대 개수. (Default : 8) 이를 초과한 반납되는 커넥션은 닫음.  
maxWait : 모든 커넥션이 다 사용되고 있을 때, 커넥션이 반납되어 제공하기까지 기다리는 최대 밀리초. 이 시간이 지날 때 까지 반납되는 커넥션이 없으면 예외 발생.  
closeMethod : 톰캣 서버가 종료될 때 자원을 해제하기 위해 호출하는 메소드 이름. 매개변수가 없어야 함.  

- DD file에 내용 추가.

```
<resource-ref>
  <res-ref-name>JNDI 이름</res-ref-name>
  <res-type>리턴될 자원의 클래스 이름(패키지명 포함)</res-type>
  <res-auth>자원 관리의 주체</res-auth>
</resource-ref>
```

JNDI : Java Naming and Directory Interface API. Directory Service 에 접근하는 데 필요.  

- contextInitialized() 에서 DataSource 가져오기.

```
InitialContext initialContext = new InitialContext();
DataSource ds = (DataSource) initialContext.lookup("java:comp/env/jdbc/studydb");
```

톰캣 서버에서 자원을 찾기 위해 InitialContext 객체 생성하고, lookup() 통해 JNDI 이름으로 등록되어 있는 서버 자원을 가져옴.
