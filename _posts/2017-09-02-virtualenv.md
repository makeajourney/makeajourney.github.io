---
title : "가상 개발 환경 Virtualenv 사용하기"
layout : post
---

## virtualenv

[virtualenv](https://virtualenv.pypa.io/)는 격리된 파이썬 환경을 만들기 위해 사용.  


### virtualenv 설치

```bash
pip install virtualenv
```


### python 가상 개발환경 생성

```bash
virtualenv <가상개발환경이름>
```

예 `virtualenv myvenv`  


### python3 가상개발환경 생성

-p 옵션으로 특정 버전의 파이썬 사용 가능  

```bash
virtualenv -p python3 <가상개발환경이름>
```


### 가상 개발환경 실행

```bash
source <가상개발환경이름>/bin/activate
```


### 가상 개발 환경 실행 종료

```bash
deactivate
```


### 현재 가상 개발환경에 설치된 패키지 리스트

```bash
pip freeze
```

- text file로 출력  

```bash
pip freeze > requirements.txt
```


### requirements.txt로 dependent package 설치

```bash
pip install -r requirements.txt
```
