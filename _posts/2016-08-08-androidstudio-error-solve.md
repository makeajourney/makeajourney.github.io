---
title : "Android Studio에서 app 실행 안될 때"
layout : post
---


```
Error:java.lang.UnsupportedClassVersionError: com/android/dx/command/Main : Unsupported major.minor version 52.0
```

위와 같은 에러 발생.  

```
Go to File-> Project Structure->App -> Build Tool Version. Change it to 23.0.3
```
 
을 통해 해결.  

이후에는  

```
Session 'app': Error Launching activity
```
메시지가 출력되면서, build는 되었지만 launching이 안됨.  
이는 프로젝트 폴더에서 `.idea`, `gradle` 폴더를 삭제한 뒤 다시 import를 했더니 app이 잘 launching 되었다.
