---
title : "Django - 시작하기(개발 환경 구성, 프로젝트 생성)"
layout : post
---


# Django 개발 시작하기  


<br/>

## Python 설치  

3.4 이상의 버전 설치 필요.  

<https://www.python.org/downloads/> 에서 다운로드 가능.  


<br/>

## 가상 개발환경 구성  


<br/>

### 가상 개발환경 생성  

```sh
python3 -m venv <가상환경이름>
```


<br/>

### 가상 개발환경 실행

```sh
source <가상환경이름>/bin/activate
```

실행이 제대로 되면 콘솔 프롬프트 앞쪽에 가상 개발환경의 이름이 적혀있는 것을 확인할 수 있다.  


* 가상 개발환경 종료   

```
deactivate
```

<br/>

## Django 설치

```sh
pip install django==1.8
```


<br/>

## Django project 생성

```sh
django-admin startproject <프로젝트이름> .
```

- 주의사항  
프로젝트 이름 뒤에 `.`을 빠뜨리지 말고 작성할 것.  
가상 개발환경 내에서 생성할 것.  
