---
layout: post
title: 파이참에서 docker를 이용한 디버깅 환경 구축
subtitle: remote 인터프리터에 docker를 설정
category: study
tags: [advanced, pycharm, docker]
---
<h4>1. 서론</h4>

회사에서 [프로비저닝](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EB%B9%84%EC%A0%80%EB%8B%9D)과
[가상화](https://ko.wikipedia.org/wiki/%EA%B0%80%EC%83%81%ED%99%94) tool로 docker를
사용하고 있다. 이런 이유때문에 pycharm이라는 좋은 IDE를 사용함에도 python개발을 할때는
단순 print 함수나 pdb를 사용할 수 밖에 없었다.<br/>
생산성 향상을 위해 IDE를 이용하여 line by line으로 디버깅 하면서 개발하는 것이 좋다는 판단이 들어서
pycharm의 디버그 기능을 온전히 이용할 수 있게 설정을 해보았다.<br/>

<h4>2. pycharm remote 인터프리터로 docker machine 설정</h4>

**참고:**<br/>
[Configuring Remote Interpreters via Docker](https://www.jetbrains.com/help/pycharm/2017.1/configuring-remote-interpreters-via-docker.html)<br/>

docker의 container는 process의 개념과 유사한 것으로 알고있다. 그래서 pycharm에서
remote 인터프리터를 docker로 설정하는 것이 다른 가상화 도구(vagrant)보다 까다로운 듯이 보인다.
아무튼, 설정을 위해 알아본 결과, 위 링크를 참조하여 그대로 설정한다. 아래는 설정 흐름을 순서대로 나타낸 것이다.<br/>
![img5](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/media/2017-06-02-study-pycharm_interpreter_5.png?raw=true)<br/>
링크에 자세히 나와있지만, docker machine이 not available 할 경우 위 그림의 2번 과정인 New 버튼을 누르고나서 설정하는 부분에 대해서는 설명되어 있지 않다.
그림의 3번 라디오 버튼을 누르고, 체크가 완료되면 4번 박스 부분에 "Connection successful"이라고 display되면 문제가 없는 것이다.
<br/>**Note: 이 과정에서 문제가 발생하면 2-1과정을 진행한 후 다시 이 부분으로 돌아온다.**<br/>
이후, 5번 OK 버튼을 눌러 처리하면, 6, 7, 8 박스에 각각 현재 설치되어 있는 docker와 docker image 상태에 맞게 자동 설정된다.
<br/>**Note: 이 과정에서 문제가 발생하면 docker 설치, docker image 존재 여부, image안에 있는 python 인터프리터 경로 등을 확인한다.**<br/>

<h4>2-1. docker-compose</h4>

docker machine 설정에서 Unix socket을 이용한 Docker daemon과의 연결이 문제가 있을 경우,
docker 설치를 다시 하거나 docker-compose를 설치하면 문제가 해결되었다.
**docker-compose는** 간단히 설명하면 multi-container(또는 single-container)가 필요한 application을 편하게 사용하기 위한 도구이다.<br/>
**참고:**<br/>
[docker-compose를 이용한 개발환경 구축하기](http://ggoals.tistory.com/61)<br/>
[Docker Compose 간단 사용 후기](http://blog.naver.com/PostView.nhn?blogId=alice_k106&logNo=220633836094&parentCategoryNo=7&categoryNo=&viewDate=&isShowPopularPosts=true&from=search)<br/>
[Official: Get started with Docker Compose](https://docs.docker.com/compose/gettingstarted/#step-3-define-services-in-a-compose-file)<br/>

위에 링크들을 참조하여 docker-compose.yml파일을 각자의 application에 맞게 프로젝트 루트에 생성하고,
docker-compose 명령어를 실행해서 잘 작동하는지 확인해보자. single docker contatiner를 시작하기 위해 `docker run` 명령어에 썼던 option들을
전부 docker-compose.yml파일로 옮긴다고 보면된다.

<h4>4. run/debug 설정</h4>

여기까지 설정됐다면 이제 **run/debug configuration**을 해야한다. 먼저 아래 링크들을 참고한다.

**참고:**<br/>
[Official: Run/Debug Configuration: Django Test](https://www.jetbrains.com/help/pycharm/2017.1/run-debug-configuration-django-test.html)<br/>
[PyCharm Professional 버전에서 Docker로 Remote Debugging](http://bryan7.tistory.com/864)<br/>
[About network of Docker Container](https://stackoverflow.com/questions/36489696/cannot-link-to-a-running-container-started-by-docker-compose)<br/>

두번째 링크([PyCharm Professional 버전에서 Docker로 Remote Debugging](http://bryan7.tistory.com/864))를 주로 보면서, 첫번째 링크인
JetBrain공식 문서를([Run/Debug Configuration: Django Test](https://www.jetbrains.com/help/pycharm/2017.1/run-debug-configuration-django-test.html))
같이 보는 것이 좋다.
세번째 문서([About network of Docker Container](https://stackoverflow.com/questions/36489696/cannot-link-to-a-running-container-started-by-docker-compose))는
docker-compose를 사용하면서 변경되어야 하는 container의 network설정에 대한 내용이다.

세 가지 문서를 모두 참조하면 인터프리터 설정이 가능할 것이다. 이제 편하게 디버깅하면서 개발할 수 있는 환경이 되었을 것이다.

<h4>5. run/debug 설정 예시</h4>

만약 설정이 완료되지 않았을 경우와 나중에 리마인드할 경우를 대비하여 예시로,
내가 설정한 run/debug configuration 과정을 적어보았다.

1. run/debug configuration 디테일<br/>
빨간 box부분만 설정해주면 된다. 자세한 설명은 *4. run/debug 설정의 두번째 링크*를 참조한다.<br/>
![img1](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/media/2017-06-02-study-pycharm_interpreter_1.png?raw=true)<br/><br/>
**Name:** configuration파일의 이름을 적당히 정해주면 된다.<br/>
**Target:** 디버그를 실행할 Target파일의 경로를 정한다. Working Directory 아래에서부터의 경로를 정한다.<br/>
**Environment variables:** 장고 app 실행시 필요한 환경변수를 설정한다. 아래 2번 이미지 참조.<br/>
**Python Interpreter:** 위에서 docker-compose로 설정한 인터프리터를 설정한다.<br/>
**Working Directory:** Working Directory를 설정한다. 나는 프로젝트 루트를 설정했다.<br/>
**Path mappings:** 공유할 디렉토리의 Local Path(Host Path)와 Remote Path(Guest Path)를 설정한다.<br/>
나는 Local의 프로젝트 루트와 Remote의 프로젝트 루트를 설정했다. 아래 3번이미지 참조.<br/>
**Docker Container settings:** Docker container에 설정했던(docker-compose.yml에 설정했던) option들을 설정한다. 아래 4번이미지 참조.<br/>

2. Environment variables 디테일<br/>
![img2](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/media/2017-06-02-study-pycharm_interpreter_2.png?raw=true)<br/><br/>
**PYTHONBUFFERED:** 값은 1로 기본적으로 설정되어있다. 나는 유지했다.<br/>

3. Path mappings 디테일<br/>
![img3](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/media/2017-06-02-study-pycharm_interpreter_3.png?raw=true)<br/><br/>

4. Docker Container settings 디테일<br/>
![img4](https://raw.githubusercontent.com/seongwoopark/seongwoopark.github.io/master/media/2017-06-02-study-pycharm_interpreter_4.png?raw=true)<br/><br/>
**Network Mode:** *4. run/debug 설정의 세번째 링크*를 참조한다. `docker-compose up -d` 명령어를 실행 후
생성되는 container network의 이름을 설정한다. 이를 bridge나 다른 이름으로 설정할 경우 Links 부분에서 참조를 하지 못한다.<br/>

이상으로 docker로 개발할때, pycharm에서 remote 인터프리터를 설정하고, 이를 이용하여 디버깅 설정까지하는 방법이다.<br/>