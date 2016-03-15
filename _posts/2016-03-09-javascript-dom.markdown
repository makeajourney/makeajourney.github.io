---
title : "DOM (Document Object Model)"
layout : post
---


### 문서 객체 모델 (DOM - Document Object Model)
* HTML과 XML 문서에 대한 API(Application Programming Interface).  
* 기본적인 문서 구조와 쿼리 인터페이스 제공.  
* 문서를 Node의 계층 구조 tree로 표현.  
* DOM Level 1이 1998년 10월 W3C 권고가 됨.  
* Internet Explorer 8 및 이전 버전을 제외한 나머지 Internet Explorer, Firefox, Safari, Chrome and Opera의 latest version은 모두 DOM을 정확히 구현.  

### Node의 계층 구조 
Document node는 각 document를 root로 표현.  
HTML page에서 document 요소는 항상 `<html>` element.  
총 12가지의 node type을 가짐.  

<br>

### Node Type
DOM에서 존재하는 node type 은 모두 DOM level 1 Interface를 구현.  
- Node.ELEMENT_NODE (1)  
- Node.ATTRIBUTE_NODE (2)  
- Node.TEXT_NODE (3)  
- Node.CDATA_SECTION_NODE (4)  
- Node.ENTITY_REFERENCE_NODE (5)  
- Node.ENTITY_NODE (6)  
- Node.PROCESSING_INSTRUCTION_NODE (7)  
- Node.COMMENT_NODE (8)  
- Node.DOCUMENT_NODE (9)  
- Node.DOCUMENT_TYPE_NODE (10)  
- Node.DOCUMENT_FRAGMENT_NODE (11)  
- Node.NOTATION_NODE (12)  

 <br>


#### nodeType

```
// IE 9 미만에선 작동하지 않음.
if (someNode.nodeType == Node.ELEMENT_NODE) {
	alert('Node is an element.');
}
```
  
```
// 모든 브라우저에서 동작
if (someNode.nodeType == 1) {
	alert("Node is an element.");
}
```

<br>

#### nodeValue
