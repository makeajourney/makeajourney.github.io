---
title : "CentOS Epel repository 추가"
layout : post
---



CentOS7 기준.


```sh
yum install wget
wget -r --no-parent -A 'epel-release-*.rpm' http://dl.fedoraproject.org/pub/epel/7/x86_64/e/ 
rpm -Uvh dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-*.rpm
```


```sh
rpm -ivh http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-2.noarch.rpm
```

참고 :  
Install EPEL repo on CentOS 7 and RHEL 7  
<http://sharadchhetri.com/2014/09/07/install-epel-repo-centos-7-rhel-7/>  
