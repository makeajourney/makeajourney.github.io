---
layout: post
title: "'X11/Xlib.h' file not found 에러 발생시 해결 방법"
---

Cpython make 중 에러 발생  

~~~sh
ln -s /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.14.sdk/System/Library/Frameworks/Tk.framework/Versions/8.5/Headers/X11 /usr/local/include/X11
~~~

reference: https://github.com/pinax/pinax-project-teams/issues/1  
