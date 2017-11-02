---
layout: post
title: Introduction to Operating System
subtitle: from Udacity
category: study
tags: [basic, lessons, operation system]
---
<h4>Operating System 이란?</h4>
![capture](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-11-02-study-intro_os_1.png)
"An operating system is the layer of systems software sits between the complex hardware and all of the applications."
운영체제는 복잡한 하드웨어와 모든 어플리케이션 사이에 위차하는 시스템 소프트웨어 레이어이다.<br/>
기능들은 다음과 같다.<br/>
- hide hardware complexity: os는 하드웨어들을 추상화한다.<br/> e.g.) 스토리지를를 file로, 네트워크를 socket으로
- resource management: os는 리소스들을 관리한다.<br/> e.g.) cpu 스케쥴링, 메모리 관리 등
- provide isolation & protection to applications: os는 어플리케이션들에게 독립된 공간을 제공한다.
- directly has privileged access to hardware: 직접 액세스할 수 있는 권한이 있다.
- manages hardware on behalf of applications according to policies: 어플리케이션의 동작에서 하드웨어들을 미리정의된 정책에 맞게 관린한다.

<br/>
하드웨어 자원들에 대해 쉽게 이름을 붙이고, : 추상화(Abstractions)<br/>
미리정의된 로직, 알고리즘 또는 룰에 맞게, : 정책(Policies)<br/>
운영체제와 관련되어 있는 요소들을 관리한다. : 관리(Arbitration or Management)<br/>

<h4>OS Elements</h4>
Abstractions:<br/>
process, thread, file, socket, memory page
Mechanisms: <br/>
create, schedule, open, write, allocate
Policies:<br/>
least recently used(LRU), earliest deadline first(EDF)
<br/><br/>Examples<br/>
![capture](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-11-02-study-intro_os_2.png)

<h4>OS Protection Boundary: User/Kernel</h4>
user level(unprivileged mode): applications<br/>
kernel level(privileged mode): OS kernel, privileged direct hardware access<br/>
<br/>
user-kernel switch(transition)<br/>
![capture](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-11-02-study-intro_os_3.png)


<h4>OS Types</h4>
Monolithic OS<br/>
![capture](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-11-02-study-intro_os_4.png)
Kernel이 memory management, device driver, file management, process/thread, scheduling, random i/o filesystem, sequential i/o filesystem
등의 모든 기능을 다 갖는 구조

Modular OS<br/>
![capture](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-11-02-study-intro_os_5.png)
Kernel이 기능을 모듈별로 관리하고, 모듈을 외부로 부터 설치할 수 있는 구조

Microkernel<br/>
![capture](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/img/2017-11-02-study-intro_os_6.png)
Kernel이 오직 application들을 실행하기 위한 기본적인 address space, threads 관리하는 기능만을 갖고, 나머지 기능(filesystem, disk driver 등)은 전부 user 레벨에서 실행되는 구조

<h4>Reference</h4>
[Introduction to Operating Systems by  Georgia Institute of Technology; Offered at Georgia Tech as CS 8803](https://www.udacity.com/course/introduction-to-operating-systems--ud923)<br/>
<br/><br/><br/>