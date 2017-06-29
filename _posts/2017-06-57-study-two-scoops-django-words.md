---
layout: post
title: Two Scoops of Django
subtitle: 용어
category: study
tags: [basic, django, best practice, terms]
---
이전에는 문맥상 이해하며 넘겼었지만,
'Two Scoops of Django'을 다시 보면서 애매하게 알거나 모르는 단어들, 새겨야 할 문구들을 의미와 함께 나열해보았다.

0. Fat Models, Utility Modules, Thin Views, Stupid Template<br/>
모델은 크게, 유틸리티는 모듈로, 뷰는 가볍게, 템플릿은 단순하게라는 철학이다.
비즈니스 로직을 뷰에서 분리하고, 유틸리티로 만들어 재사용성을 높이고, 필요에 따라서는 모델에 로직이 포함되어서 간단히 사용될 수 있도록 한다. 

1. 12팩터 앱
웹 기반 어플리케이션을 디자인할 때에 통합 전략, 앱 방법론이다. 확장과 배포가 가능한 앱을 만드는 것이 목적.
<br/>참고 - [The Twelve-Factor App (한국어)](http://12factor.net/ko/)

2. 안티패턴<br/>
안티패턴(anti-pattern)은 소프트웨어 공학 분야 용어이며, 실제 많이 사용되는 패턴이지만 비효율적이거나 비생산적인 패턴을 의미한다.
<br/>출처 - [안티패턴 - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/안티패턴)

2. 프로비저닝<br/>
프로비저닝(provisioning)은 사용자의 요구에 맞게 시스템 자원을 할당, 배치, 배포해 두었다가 필요 시 시스템을 즉시 사용할 수 있는 상태로 
미리 준비해 두는 것을 말한다.
<br/>출처 - [프로비저닝 - 위키백과, 우리 모두의 백과사전](https://ko.wikipedia.org/wiki/프로비저닝)