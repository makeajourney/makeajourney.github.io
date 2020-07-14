---
layout: page
title: About
permalink: /about/
---

# 김소연 (Soyoun Kim)  

email : <makeajourney@gmail.com>  
blog : <http://makeajourney.github.io>  
github : <https://github.com/makeajourney>  

## Experiences  

- [마카롱팩토리](https://mycle.co.kr/), 서버 개발자, 2019.07~2020.02
    * 모바일 서비스 "마카롱" API Server 신규 기능 개발 / 유지보수 (Kotlin, Spring Boot, MySQL, angular1)
    * 신규 프로젝트 "마카롱 파트너스" API Server, Admin site 신규 개발 (API Server: Kotlin, Spring Boot, MySQL, Admin: React.js, TypeScript)  
    * Amazon Web Service ECS 기반으로 한 전반적인 운영 / 배포 / 모니터링

- [넥스트매치](http://nextmatch.kr), 서버 개발자, 2017.07~2018.03  
    * 모바일 서비스 "아만다" API Server, admin site 신규 기능 개발 / 유지보수 (ruby, rails, postgresql, angular1)  
    * 신규 채팅 프로젝트 개발 (Go lang, mysql)  

- [세림티에스지](www.selim.co.kr), 기술연구소 연구원, 2016.08~2016.11  
    * object storage(ceph, glusterfs) 검증  
    * container 기반(docker, kubernetes) 검증  
    * operation automation 관련 orchestration script 작성(ansible playbook)  
    * spark(scala) 기반 전력 수요량 예측 모델 개발  


## Education

- 충남대학교 공과대학 컴퓨터공학과, 학사, 2011.03~2017.08(졸업)  


## Community  

- [파이콘 한국](https://www.pycon.kr) 준비위원회 2016.12~2018.09  
  관련 post : [파이콘 준비위원회, 그리고 6개월](http://blog.pycon.kr/2017/06/18/pycon-staff-retrospection)  


## Project  

### 커뮤니티 투표 시스템(실습 프로젝트)  

- description :  
    현장실습 때 보름간의 프로젝트로 만든 커뮤니티 투표 시스템 입니다.  
    전자정부 프레임워크 기반에서 수행했습니다. jsp와 servlet만 아는 상태에서 2주라는 짧은 시간 내에 Spring Framework를 사용하여야 했기 때문에 거대하고 복잡한 Spring을 모두 완전히 이해할 수는 없었지만, 덕분에 단 시간 내에 Spring을 습득할 수 있는 좋은 기회였습니다.  
    MVC 디자인 패턴이 적용되었으며, login, CRUD, Multipart file upload 기능이 구현되었습니다.  
- spec : CentOS7, Java, Spring 3.0, MySQL DB, MyBatis, Maven, apache tomcat  
- Source code: [github repo](https://github.com/makeajourney/vote)  
- design documentation:
  [기능 설계](https://drive.google.com/file/d/0B2OC1aS74bTTV1YwTkd5WmNPY28),
  [DB 설계](https://drive.google.com/file/d/0B2OC1aS74bTTa3hOTkNJZ09vVFU),
  [UI 설계](https://drive.google.com/file/d/0B2OC1aS74bTTa3JubHJjMkVjN1U)  

### 키워드를 사용한 맞춤형 커뮤니티 신문(졸업 프로젝트)  

- description:
    학부 졸업프로젝트로 수행된 프로젝트입니다.  
    python library인 scrapy를 사용하여 데이터를 크롤링하고, 이를 mysql db에 적재한 후, 파이썬 자연언어처리 도구인 KoNLPy library를 이용해 가공을 거쳐 다시 mysql에 적재합니다. 이는 crontab을 사용해 일정 주기별로 동작하도록 설계되었습니다.  
    앞에서 가공된 데이터를 web을 통해 service하는 형태입니다.  
    웹은 기본적인 html, css, js를 front-end에 사용하였고, jsp와 servlet을 사용하여 server-side가 구현되었습니다.  
- spec: Python 2.7, Java,  
- Source code: [Crawler](https://github.com/MJ-Youn/crawler), [Web](https://github.com/makeajourney/hub)  
- 논문(final Report): [google drive](https://drive.google.com/file/d/0B2OC1aS74bTTaWhZWndRbHZQeWs/view?usp=sharing)  
- design documentation :  
[문제 정의서](https://drive.google.com/open?id=0B2OC1aS74bTTWlhZNXk0UHRBZGs),
[유스케이스명세서](https://drive.google.com/open?id=0B2OC1aS74bTTWUxSTWd2ZlhLaFk),
[클래스다이어그램](https://drive.google.com/open?id=0B2OC1aS74bTTd1hvOFVEOWdoV00),
[시퀀스다이어그램](https://drive.google.com/open?id=0B2OC1aS74bTTSUZjVG1Rb3lIc0U),
[액티비티다이어그램](https://drive.google.com/open?id=0B2OC1aS74bTTbElmeHpkaklsMGc)

## have used  

- ansible  
  playbook script: <https://github.com/makeajourney/ansible_study>  
- data processing: [파이썬을 이용한 공공데이터 분석](https://drive.google.com/open?id=0B2OC1aS74bTTbzJJYm00QjJ0RzQ),  [R을 이용한 공공데이터 분석](https://drive.google.com/open?id=0B2OC1aS74bTTdDliMnhNUm9kb1U)  

## interested in  

### django  

### Scala  

- Study "Functional Programming in Scala" [github](https://github.com/kpug/fpis)  
