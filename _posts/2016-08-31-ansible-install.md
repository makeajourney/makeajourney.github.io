---
title: "Ansible 설치, 기본 사용법"
---


## 설치  


### centos에서 yum으로 install  

```{.bash}
sudo yum install ansible
```  

`No package ansible available.` 이라고 나오면,  

```{.bash}
sudo yum install epel-release
sudo yum repolist
```  

Ansible이 Extra Packages for Enterprise Linux (EPEL) repository의 일부이기 때문에 epel-release package를 설치해줘야 한다. 이후 다시 yum install하면 dependency와 함께 모두 잘 설치됨을 알 수 있다.  


<br/>


## inventory setting  

여러개의 target vm을 제어하기 위해서, host들의 DNS 주소나 IP, 설정을 파일로 생성해서 관리해야 한다.  
기본값으로 `/etc/ansible/hosts`에 작성하면 ansible이 알아서 인식한다.  
다른 경로를 기본적으로 인식하게 하고 싶은 경우는, cfg 파일을 변경해야 한다.
다른 경로나 이름의 파일을 사용하고 싶은 경우, `ansible` 혹은 `ansible-playbook` 명령을 사용할 때 `-i` 옵션을 사용하여 명시하면 된다.  

```{.bash}
ansible-playbook -i << inventory file >> << playbook yml >>
```  

여기서 `ansible_hosts`가 inventory 파일이다.  

```
# ansible_hosts
[targets]
10.0.4.239
10.0.4.241

[targets:var]
ansible_user=<< username >>
ansible_ssh_pass=<< password >>
```


inventory 파일 작성법은 [Ansible documentation  - Inventory](http://docs.ansible.com/ansible/intro_inventory.html) 를 참고하면 된다. 바로 다음에 보이는 [Dynamic Inventory](http://docs.ansible.com/ansible/intro_dynamic_inventory.html)를 참고하면 더 Advance된 사용이 가능할 것 같다.  


<br/>


## playbook  

ansible은 configuration, deployment, orchestration을 위한 모듈의 단위로 Playbook을 사용한다. playbook은 YAML로 작성 가능하다.  
playbook은 다시 play 단위로 나뉘어 실행된다. 예를 들어,  

```{.yml}
# helloworld.yml
---
- hosts: localhost
  tasks:
    - name: test
      shell: echo "hello world"

- hosts: target
  tasks:
    - name: test
      shell: echo "hello world"  
```  

위와 같은 playbook을 실행한다.  

```{.yml}
ansible-playbook -i ansible_hosts helloworld.yml
```  

그러면 아래와 같은 결과를 확인할 수 있다.

```{.bash}
PLAY [localhost] ***************************************************************

TASK [setup] *******************************************************************
ok: [localhost]

TASK [hello] *******************************************************************
changed: [localhost]

PLAY [targets] *****************************************************************

TASK [setup] *******************************************************************
ok: [10.0.4.241]
ok: [10.0.4.239]

TASK [hello] *******************************************************************
changed: [10.0.4.239]
changed: [10.0.4.241]

PLAY RECAP *********************************************************************
10.0.4.239                 : ok=2    changed=1    unreachable=0    failed=0   
10.0.4.241                 : ok=2    changed=1    unreachable=0    failed=0   
localhost                  : ok=2    changed=1    unreachable=0    failed=0   
```  

보다시피, 같은 플레이북 내 두번의 플레이가 발생함을 알 수 있다. host가 변경되면서, 다시 새로운 play를 진행한다.  
play가 시작됨과 동시에 ansible은 host의 정보를 모으고 아래 task 실행을 준비하는 `setup` task를 실행한다. 이 작업이 끝나면 아래에 명시된 tasks를 실행하게 된다.  `gather_facts: false`로 설정하여 host의 정보를 모으는 task를 생략할 수 있다.  
한 playbook 내에 여러개의 play를 실행할 때 주의할 점은, play간의 정보를 공유하지 않는다는 것이다. 변수 선언으로 저장한 정보나 register도 play가 변경되면서부터는 사용할 수 없다.  


### example  


1. 파일 생성  

```{.yml}
---
- hosts: targets
  become: yes
  user: centos
  vars:
    motd_warning: 'WARNING: Use by ACME Employees ONLY'
  tasks:
    - name: setup a MOTD
      copy: dest=/etc/motd content={{ motd_warning }}
```  

* hosts : target hosts name (여기서는 group name)  
* become : sudo 권한으로 target을 제어하기 위해 사용.  
* user : target host의 username.  
* vars : 필요한 변수 선언해서 사용 가능하다. 이렇게 선언한 변수들은 `{{ 변수이름 }}` 형식으로 사용할 수 있다.  
* tasks : 수행할 작업들.  


결과  

```{.bash}
PLAY [targets] *****************************************************************

TASK [setup] *******************************************************************
ok: [10.0.4.241]
ok: [10.0.4.239]

TASK [setup a MOTD] ************************************************************
changed: [10.0.4.241]
changed: [10.0.4.239]

PLAY RECAP *********************************************************************
10.0.4.239                 : ok=2    changed=1    unreachable=0    failed=0   
10.0.4.241                 : ok=2    changed=1    unreachable=0    failed=0  
```  


#### Variables (변수 사용)  


위의 예제에서 vars에 변수를 쓰는 대신, 별도의 파일로 분리해서 가져올 수도 있는데, 이 때는 `vars_files :` 라고 작성하며, 아래에 파일 경로와 파일명을 작성한다. 변수 파일은 아래 형식으로 작성되면 된다.  

```{.yml}
---
ntp: 'ntp1.au.example.com'
TZ: 'Austrailia/Sydney'
```  

대화형 프롬프트를 사용할 수도 있다.  

```{.yml}
vars_prompt:
  - name: 'https_passphrase'
    prompt: 'Key Passphrase'
    private: yes
```  
