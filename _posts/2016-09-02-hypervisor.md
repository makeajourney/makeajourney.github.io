---
title: "Hypervisor"
layout: post
---

# Hypervisor  
host computer 1대에서 운영체제 다수를 동시에 실행하기 위한 논리적 platform  
Virtual Machine Monitor

hypervisor는 native, hosted 두가지 방식으로 사용할 수 있다.  

## Native  
Bare-metal, TYPE1이라고도 부른다.  
해당 하드웨어에 직접 설치되어 실행된다.  
guest OS는 hypervisor 위에서 2번째 수준으로 실행된다.  


## hosted  
TYPE2라고도 부른다.  
host OS 위에서 hypervisor가 실행된다.  
guest OS는 3번째 수준으로 실행된다.


## Visualization  
위 두가지 분류와 별개로, 가상화 방식에 따라 hypervisor를 분류하기도 한다.  


### Full Virtualization
전가상화.  
HW를 모두 가상화하는 방식이다.  
guest OS로 여러가지 OS를 다양하게 이용 가능하다.  
전가상화는 물리적인 가상과 기능을 가진 CPU의 가상화 기술을 기반으로 하기 때문에 CPU의 지원이 필요하다.  
Native 방식이 전가상화를 이용한 방식이다.  


### Para Virtualization  
반가상화.  
HW를 완전하게 가상화하지 않는 방식이다.  
guest OS가 직접 HW를 제어할 수는 없고, hypervisor를 통하게 되어 있으므로 높은 성능을 유지하는 방식이다.  
kernel의 일부 수정이 필요하므로 open-source OS만 사용이 가능하다.  


## Openstack  
Openstack은 전가상화 방식을 사용할 때 KVM, 반가상화 방식을 사용할 때는 QEMU를 hypervisor로 사용하게 되어 있다.  
KVM은 QEMU 위에서 실행된다. 그러므로 KVM은 full, para 모두 지원하는 hypervisor이다.  
