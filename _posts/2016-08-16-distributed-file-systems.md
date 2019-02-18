---
title : "분산파일시스템(Distributed File Systems)"
layout : post
---

[어떤 분산 파일 시스템을 사용해야 하는가?](http://d2.naver.com/helloworld/258077) - Naver D2  


### NFS & CIFS


#### NFS(Network File System)  
Linux/Unix 환경에서 사용.  
1984년에 Sun Microsystems에서 개발한 분산 파일 시스템.  
현재 주류를 이루는 것은 NFSv3.  
성능과 보안을 개선한 NFSv4가 2003년에 나왔으며, 2010년에는 클러스터 기반으로 확장할 수 있는 NFSv4.1이 발표.  


#### CIFS(Common Internet File System)  
Microsoft Windows 환경에서 사용.  
IBM에서 개발한 SMB(Server Message Block)를 바탕으로 보안 등의 기능을 개선해 Microsoft가 Windows에 적용한 분산 파일 시스템.


NFS와 CIFS는 POSIX 표준을 준수한다. 그렇기 때문에 NFS나 CIFS를 사용하는 애플리케이션은 로컬 파일 시스템처럼 분산 파일 시스템을 사용할 수 있다.  
NFS나 CIFS를 사용할 때는 성능과 가용성을 위해 NAS(Network Attached Storage)를 사용.

결론 :  
  - 로컬 파일 시스템과 동일한 기능 제공  
  - NAS를 사용하는 경우가 많기 때문에 NAS에 대한 높은 구매 비용 필요  
  - NAS는 OwFS와 HDFS에 비하여 확장성이 크게 떨어짐  


-----


#### HDFS(Hadoop Distributed File System)  
Google은 웹 페이지 정보를 크롤링해 저장할 수 있는 고유의 분산 파일 시스템인 GFS(Google File System)에 대한 정보를 2003년 논문으로 발표, HDFS는 GFS를 모델로 해서 만들어진 오픈소스.  
대용량의 파일을 청크(chunk)라는 단위로 분할해 데이터노드(Datanode)에 3개씩 분산 저장.  
하나의 파일에 대한 복제본이 3개씩 있고, 이 청크의 크기 단위는 보통 64MB.  
청크가 어느 데이터노드에 저장되었는지에 대한 메타데이터는 네임노드(Namenode)에 저장.  
MapReduce 프레임워크를 이용해 분산 저장된 파일을 읽어 연산할 수 있다.  
Java로 작성되어 있기 때문에 JAVA api 사용 가능. JNI(Nava Native Interface)를 이용한 C API도 사용가능.
서드 파티에서 HDFS에 대한 FUSE 마운트 기능을 제공.  

결론 :  
  - 크기가 큰 파일이 청크 단위로 나뉘어 여러 데이터노드에 분산 복제 저장됨.  
  - 청크 크기는 보통 64MB이고, 각각의 청크는 3개의 복제본이 존재하며, 서로 다른 데이터 노드에 청크가 저장됨.  
  - 이 청크들에 대한 정보는 네임노드에 저장돼 있음.  
  - 대용량의 파일을 저장하는 데 유리하며, 파일의 개수가 많으면 네임노드의 부담이 커짐.  
  - 네임노드가 SPOF(Single Point Of Failure). 네임노드에 장애가 발생하면 운영 불가 상황이 발생하며 수동 복구 필요.  




---

파일의 크기가 크지 않고 파일의 개수가 많은 경우 OwFS  
파일의 크기가 크고 개수가 많지 않은 경우, 변경될 가능성이 낮은 경우 HDFS  
빈번한 수정이 필요한 경우, NAS  




---



#### GFS2
GFS2의 네임노드는 싱글 마스터가 아니라 분산 구조를 따름.  
메타데이터는 수정이 가능한 BigTable과 같은 데이터베이스에 저장.  
이러한 방법으로 파일 개수의 제한이나 네임노드 장애 취약성을 개선.  
청크 크기를 1MB로 줄일 수 있다.  



#### Swift 
OpenStack에서 사용하는 분산 객체 저장소(Object Storage).  
Amazon S3와 같이 마스터 서버를 따로 두지 않는 구조를 사용.  
Account, Container, Object라는 3단계 객체 구조를 사용.  
  - Account 객체는 계정과 같은 개념으로 Container 객체를 관리.  
  - Container 객체는 디렉터리와 같이 Object 객체를 관리.  
  - Object 객체는 파일에 해당하는 객체로 이 Object 객체에 접근하려면 Account 객체와 Container 객체에 차례로 접근해야 한다.  

Swift는 프락시서버를 두고 REST API를 제공.  
객체가 할당된 위치 결정을 위한 정적 테이블인 Ring 정보를 공유하고 원하는 객체의 위치를 찾아가게 된다.  




#### Ceph  
메타데이터 서버들이 클러스터 형태로 동작하며 동적으로 부하 정도에 따라 메타데이터별로 담당하는 네임스페이스 영역을 조정할 수 있다는 것이 특징.  
다른 분산 파일 시스템과 달리 POSIX와 호환하기 때문에 분산 파일 시스템에 저장된 파일에 접근할 때 로컬 파일 시스템처럼 접근할 수 있다는 것이 장점.  
REST API 지원.  
POSIX 호환 형태이고 커널 마운트를 지원.  




#### pNFS (Parallel Network File System)  
NFS의 다른 버전.  
파일 내용과 파일의 메타데이터를 분리해 처리.  
하나의 파일을 여러 곳에 나눠 분산 저장하는 기능 제공.  
