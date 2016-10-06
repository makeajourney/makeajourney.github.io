---
title : "리눅스 환경변수 설정, 파이썬에서 읽어오기"
layout : post
---


### Setting Environment Variables

```sh
vi .bashrc
```  


추가 할 변수 작성.  

```sh
<변수명>=<내용>
export <변수명>
```  


저장한 뒤 source로 변경내용 반영  

```sh
source .bashrc
```


### Reading the env variable from python

파이썬에서 읽어오려면 os 모듈을 import 해야한다.  

```
import os

envVar = os.environ['<변수명>']  # 해당 변수의 내용을 가져온다.
envKeys = os.environ.keys()   # 전체 환경변수의 변수명을 다 가져온다.
```  
