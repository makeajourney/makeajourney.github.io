---
layout: post
title: JSTL 사용하기
category: Java Web 
---

## JSTL(JSP Standard Tag Library)  
JSP 확장 태그
: View 내부에서 JSP code 의 사용 빈도를 줄이고 가독성을 높이기 위해 사용.

<br>

### 사용 전 준비해야 할 것
* JSTL API  
  <http://jstl.java.net> > Download > JSTL API  
  javax.servlet.jsp.jstl-api-1.2.1.jar 파일 다운로드.
* JSTL Implementation  
  <http://jstl.java.net> > Download > JSTL Implementation  
  javax.servlet.jsp.jstl-1.2.1.jar 파일 다운로드.

위 두 파일을 다운로드 받은 후, 프로젝트 폴더의  WebContent/WEB-INF/lib 폴더로 파일 복사(Copy).

<br>

###  태그 라이브러리 선언
`<%@ taglib uri="사용할 태그 라이브러리 URI" prefix="접두사" %>`  

<br>
  
  
### 각 태그 라이브러리 별 uri  


* core : http://java.sun.com/jsp/jstl/core  
* XML : http://java.sun.com/jsp/jstl/xml  
* Internationalization : http://java.sun.com/jsp/jstl/fmt  
* SQL : http://java.sun.com/jsp/jstl/sql  
* Functions : http://java.sun.com/jsp/jstl/functions  

<br>

<table>
  <thead>
    <tr>
      <th>Area</th>
      <th>Subfunction</th>
      <th>tags</th>
      <th>Prefix</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="4">Core</td>
      <td>Variable support</td>
      <td>remove, set</td>
      <td rowspan="4">c</td>
    </tr>
    <tr>
      <td>Flow control</td>
      <td>choose(when, otherwise), forEach, forTokens, if</td>
    </tr>
    <tr>
      <td>URL management</td>
      <td>import(param), redirect(param), url(param)</td>
    </tr>
    <tr>
      <td>Miscellaneous</td>
      <td>catch, out</td>
    </tr>
    <tr>
      <td rowspan="3">XML</td>
      <td>Core</td>
      <td>out, parse, set</td>
      <td rowspan="3">x</td>
    </tr>
    <tr>
      <td>Flow control</td>
      <td>choose(when, otherwise), forEach, if</td>
    </tr>
    <tr>
      <td>Transformation</td>
      <td>transform(param)</td>
    </tr>
    <tr>
      <td rowspan="3">I18N</td>
      <td>Locale</td>
      <td>setLocale, requestEncoding</td>
      <td rowspan="3">fmt</td>
    </tr>
    <tr>
      <td>Message formatting</td>
      <td>bundle, message(param), setBundle</td>
    </tr>
    <tr>
      <td>Number and date formatting</td>
      <td>formatNumber, formatDate, parseDate, parseNumber, setTimeZone, timeZone</td>
    </tr>
    <tr>
      <td rowspan="2">Database</td>
      <td>Setting the data source</td>
      <td>setDataSource</td>
      <td rowspan="2">sql</td>
    </tr>
    <tr>
      <td>SQL</td>
      <td>query(dateParam, param), transaction, update(dateParam, param)</td>
    </tr>
    <tr>
      <td rowspan="2">Functions</td>
      <td>Collection length</td>
      <td>length</td>
      <td rowspan="2">fn</td>
    </tr>
    <tr>
      <td>String manipulation</td>
      <td>toUpperCase, toLowerCase, substring, substringAfter, substringBefore, trim, replace, indexOf, startsWith, endsWith, contains, containsIgnoreCase, split, join, escapeXml</td>
    </tr>
  </tbody>
</table>

<br>

### JSTL Core tag

<br>

#### Declaration the tag library  

`<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>`

<br>

####  \<c:out>

`<c:out value="출력할 값" default="기본값"/>`  
`<c:out value="출력할 값">기본값</c:out>`  
value 값이 null이면 기본값 출력.

<br>

####  \<c:set>

변수를 생성하거나, 기존 변수의 값을 덮어쓸 때 사용. 생성된 변수는 보관소에 저장됨.

```
// value 속성을 사용하여 값 설정
<c:set var="변수명" value="값" scope="page|request|session|application"/>

// tag contents를 사용하여 값 설정
<c:set var="변수명 scope="page|request|session|application">값</c:set>
```  

scope 속성을 생략하면 JspContext(page)에 보관됨.

객체의 프로퍼티 값 설정  
`<c:set target="${객체명}" property="프로퍼티명" value="값"/>`  
이 때 해당 프로퍼티의 세터의 리턴 타입이 void이어야 함.

<br>

####  \<c:remove>
set 태그와 반대로, 보관소에 저장된 값을 제거.

```
// value 속성을 사용하여 값 설정
<c:remove var="변수명" scope="page|request|session|application"/>
```

<br>

#### \<c:if>
test 속성 값이 참이면, 구문 실행.

```
<c:if test="조건" var="변수명" scope="page|request|session|application">
조건이 참일 때 실행할 구문
</c:if>
```

<br>

#### \<c:choose>
다른 언어의 switch와 유사.

```
<c:choose>
  <c:when test="참 거짓 값"></c:when>
  <c:when test="참 거짓 값"></c:when>
  ...
  <c:otherwise></otherwise>
</c:choose>
```

<br>

#### \<c:forEach>
반복적인 작업 수행.

```
<c:forEach var="변수명" items="목록데이터" begin="시작인덱스" end="종료인덱스">
반복 수행할 작업
</c:forEach>
```
items 속성의 값으로 다음의 값이 올 수 있음.
- 배열
- java.util.Collection 구현체 (ArrayList, LinkedList, Vector, EnumSet...)
- java.util.Iterator 구현체
- java.util.Enumeration 구현체
- java.util.Map 구현체 
- Comma(,) 구분자로 나열된 문자열

<br>

#### \<c:forTokens>
문자열을 특정 구분자(delemeter)로 분리하여 반복작업 수행

```
<c:forTokens var="item" items="문자열변수" delims="구분자">
반복 수행할 작업 내용
</c:forTokens>
```

<br>

#### \<c:url>
매개변수를 포함한 URL을 만들 때 사용.

```
<c:url var="url이름" value="url">
  <c:param name="매개변수 이름 1" value="매개변수 값"/>
  <c:param name="매개변수 이름 2" value="매개변수 값"/>
  ...
</c:url>
```

<br>

#### \<c:import>
해당 url의 응답 결과를 가져옴.

```
<c:import url="URL 주소"/>
```

<br>

#### \<c:redirect>
HttpServletResponse의 sendRedirect() 호출  

```
<c:redirect url="redirect 할 url"/>
```
