---
title : "wildfly로 전자정부프레임워크 애플리케이션 배포시 문제 해결 방법."
layout : post
---


## wildfly로 전자정부프레임워크 애플리케이션 배포시 문제 해결 방법.  

정상적으로 build 이루어지나, wildfly가 war file을 풀면서 에러 발생.  

- error log  
```
[0m[31m00:21:43,301 ERROR [org.jboss.msc.service.fail] (MSC service thread 1-4) MSC000001: Failed to start service jboss.deployment.unit."ROOT.war".PARSE: org.jboss.msc.service.StartException in service jboss.deployment.unit."ROOT.war".PARSE: WFLYSRV0153: Failed to process phase PARSE of deployment "ROOT.war"
...
Caused by: javax.xml.stream.XMLStreamException: ParseError at [row,col]:[13,23]
Message: Unexpected value 'body-content' encountered
...
[0m[31m00:21:43,320 ERROR [org.jboss.as.controller.management-operation] (Controller Boot Thread) WFLYCTL0013: Operation ("deploy") failed - address: ([("deployment" => "ROOT.war")]) - failure description: {"WFLYCTL0080: Failed services" => {"jboss.deployment.unit.\"ROOT.war\".PARSE" => "org.jboss.msc.service.StartException in service jboss.deployment.unit.\"ROOT.war\".PARSE: WFLYSRV0153: Failed to process phase PARSE of deployment \"ROOT.war\"
    Caused by: org.jboss.as.server.deployment.DeploymentUnitProcessingException: WFLYUT0027: Failed to parse XML descriptor \"/content/ROOT.war/WEB-INF/lib/spring-modules-validation-0.9.jar/META-INF/valang.tld\" at [13,23]
    Caused by: javax.xml.stream.XMLStreamException: ParseError at [row,col]:[13,23]
Message: Unexpected value 'body-content' encountered"}}
...
WFLYCTL0186:   Services which failed to start:      service jboss.deployment.unit."ROOT.war".PARSE: org.jboss.msc.service.StartException in service jboss.deployment.unit."ROOT.war".PARSE: WFLYSRV0153: Failed to process phase PARSE of deployment "ROOT.war"
```


<br/>

### problem  
Wildfiy 사용하여 전자정부 애플리케이션 배포시 spring-modules-validation을 parsing하지 못하여 에러 발생.


<br/>

### solution  

1. spring-modules-validation을 가져오는 dependency를 찾아서, spring-modules-validate를 exclusion 처리.

```xml
<!-- examples -->
<dependencies>
  <dependency>
    <groupId>egovframework.rte</groupId>
    <artifactId>egovframework.rte.ptl.mvc</artifactId>
    <version>${egovframework.rte.version}</version>

    <!-- exclusions -->
    <exclusions>
      <exclusion>
        <artifactId>spring-modules-validation</artifactId>
        <groupId>org.springmodules</groupId>
      </exclusion>
    </exclusions>

  </dependency>
</dependencies>
```


<br/>

2. 위와 같은 문제가 없는 spring-modules-validation 추가.

dependencies 내 추가.

```xml
<dependency>
  <groupId>egovframework.rte</groupId>
  <artifactId>spring-modules-validation</artifactId>
  <version>0.9</version>
</dependency>
```
