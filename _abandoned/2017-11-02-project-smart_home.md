---
layout: post
title: Smart Home
subtitle: 자취방 ver. - 아이디어 스케치
category: project
tags: [project, idea, plan]
---
<h4>계기</h4>
- 스마트한 집에 살고 싶다. 그런 최신 아파트나 집을 살 돈은 없다. 살던 곳을 바꿀수밖에.
- 겨울이다. 자취방은 중앙난방이 아니어서 씻을때 보일러를 온수모드로 킨 후 5분이 지나야 따뜻한 물이 나온다. 귀찮다.
- 겨울이다. 자취방은 반지하다. 생각보다 춥다. 꽤 춥다. 생각보다 난방비(가스비)가 많이 나온다. 효율적으로 난방을 하고싶다.
- 자취방은 원룸이다. 원름 형광등 스위치가 현관문 옆에 없고, 현관과 꽤 멀다. 늘 저녁에 들어오면 어두운 와중에 더듬더듬 스위치를 찾느라 고생이다. 짜증난다.
- 도시가스를 쓰는 가스렌지를 사용한다. 나는 깜빡깜빡을 자주한다. 가스렌지 밸브, 원룸 형광등, 화장실 전등, 보일러를 틀어놓고 2~3일을 나갔다 올때가 있다.
이럴때 돈아까워 죽겠다.
- 기타 등등의 이유가 있지만 무엇보다 나는 아이언맨 덕후이다. 그 중 자비스를 진짜 좋아한다. 자비스 커스텀 비스무리하게 사용해보고싶다.
- 하지만 일을 크게 벌이고 싶지는 않다.(참고 - [어느 흔한 파이썬 개발자의 집 소개](https://www.pycon.kr/2016apac/program/45))

<h4>계획</h4>
**Don't Reinvent The Wheel**<br/>
최대한 안뜯어내고 일 안벌이기 위해서는.. 완성품을 알아보자,<br/>
[프로타](https://prota.info/)에서 나에게 딱 맞는 스타터 킷을 제공하는 것 같다. 디자인도 굉장히 마음에 든다.
하지만 아쉬운것들이 몇가지.<br/>
1. 센서 지원없음
2. 헤드로 사용되는 프로타S가 나중에 다른 심심풀이 DIY를 하기위해 커스텀 가능한지 의문(케이스 디자인과 자유도사이에서 갈등 중)
3. actuator들의 실사용 예제가 없음 -> 불안, 마이크로봇은 일반 가정용 전기 스위치에 사용하려면 on/off를 위해 2개의 봇이 필요, 트위스트의 경우는 심지어 내년(18년)말 출시 예정
4. [프로타파이 홍보글](http://cafe.naver.com/pipc/8271)을 보면 회사가 14년도에 창업했는데, 아직 자리를 못잡은 것 같다.

음성인식쪽에서는 완성품이 요새 하도 많이 나와서...(알렉사를 시작으로 줄줄이..) 생략..
- sensor:
아직 없음
- head(or server or hub):
프로타S 또는 라즈베리파이3 + 프로타 OS([참고](https://opentutorials.org/module/1174/8471))
- actuator:
마이크로봇(버튼식), 트위스트(원형식)

**Do It Yourself**<br/>
다음으로 완성품과 좀 더 먼 것들에 대해서 알아보자. 완성되고나면 라즈베리파이 + 빵판 + 각종 센서 + 선들이 미관상 좋지 않을 것으로 예상된다.<br/>
[MQTT 프로토콜 분석과 테스트](http://www.hardcopyworld.com/gnuboard5/bbs/board.php?bo_table=lecture_rpi&wr_id=60) + 
[Home Assistant](https://home-assistant.io/)의 좋은 글을 참고하여 Base라인으로 시작할 수 있을 것 같다.
[라즈베리파이 카페](http://cafe.naver.com/pipc)도 참고해서 진행해야 할 것 같은데, 내가 학부때(2년전)만 해도 활발했던 카페가 지금은 거의 죽어있는 듯 하다.
음성인식을 위해서는 주제와 좀 동떨어지지만 일단 이글을 보았다. [라즈베리파이에게 말을 걸다(1) : Naver TTS(음성합성) 활용 현재 온도 말하기](http://blog.naver.com/PostView.nhn?blogId=cosmosjs&logNo=220979297204&categoryNo=0&parentCategoryNo=56&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView)
더 알아봐야 될 듯하다.
- sensor:
마이크, 무선 조도 센서, 무선 온도 센서 등
- head(or server or hub):
라즈베리파이3 등
- actuator:
서보모터 등




