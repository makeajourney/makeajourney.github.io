---
title : "STS(Eclipse)에서 Maven 프로젝트를 처음 생성했을 시 에러 대처 방법"
layout : post
---


1. `POM.xml` 의 dependencies 안에 아래 내용 작성   


    ```xml
    <dependency>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-resources-plugin</artifactId>
        <version>2.4.3</version>
    </dependency>
    ```  


2. Package Explorer 에서 Project folder 우클릭 > `Run As` > `Maven Install`  


3. 다시 해당 Project folder 우클릭 > `Refresh` (`F5`)  


4. 다시 해당 Project folder 우클릭 > `Maven` > `Update Project` (`Alt` + `F5`)
