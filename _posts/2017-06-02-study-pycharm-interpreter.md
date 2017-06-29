---
layout: post
title: 파이참에서 도커 디버깅 환경 셋팅하기
subtitle: remote interpreter를 docker container로 추가
category: study
tags: [advanced, pycharm, docker, docker-compose]
---
회사에서 프로비저닝 도구로 Docker를 사용하고 있다. 익숙치 않은 가상화도구 덕분에 파이참이라는 훌륭한 IDE를 사용함에도
python 앱 개발을할때는 늘 우리 친구 pdb를 사용할 수 밖에 없었다. ~~(사실은 Docker가 아니더라도 방법은 알지 못하였지만..)~~

아무튼 더 이상 pdb만으로 개발하는 것에는 한계가 있다고 생각하게 되어서, IDE를 이용하여 line by line으로 디버깅을 하며 개발할 필요가 생겨서
설정을 해보았다.

자세히는 모르지만 Docker의 container는 process의 개념에 가까운 것으로 알고있다. 그래서인지는 모르겠지만 파이참에서
remote 인터프리터를 Docker로 설정하는 것이 다른 가상화도구들보다 까다로운 듯이 보인다.

또한, 이 remote 인터프리터를 설정할 때, docker명령어가 아닌 docker-compose명령어를 사용하여야 하고, 대상은 container가 아닌 image를 
선택해야만 한다.<br />
레퍼런스: [Configuring Remote Interpreters via Docker](https://www.jetbrains.com/help/pycharm/2017.1/configuring-remote-interpreters-via-docker.html)


따라서, docker도 잘 모르는 나이지만, docker-compose를 검색하고, 간략히 알아보고 설치하는 과정이 먼저 필요했다. 여기까지를 정리하면 다음과 같다.


**docker-compose란?**

 - docker-compose는 multi-container가 필요한 application을 편하게 사용하기 위한 tool이다. 아래 레퍼런스 참조.

**docker-compose를 셋팅한 이유?**

 - 파이참에서 docker interpreter를 사용하기 위해서
 

**docker-compose 레퍼런스**

- [docker-compose를 이용한 개발환경 구축하기](http://ggoals.tistory.com/61)

- [Get started with Docker Compose](http://blog.naver.com/PostView.nhn?blogId=alice_k106&logNo=220633836094&parentCategoryNo=7&categoryNo=&viewDate=&isShowPopularPosts=true&from=search)

- [Docker Compose 간단 사용 후기](https://docs.docker.com/compose/gettingstarted/#step-3-define-services-in-a-compose-file)


위에 링크한 **docker-compose 레퍼런스**들을 참조하여 docker-compose.yml파일을 각자의 application에 맞게 프로젝트 루트에 설정하고,
docker-compose 명령어를 실행해서 잘 작동하는지 확인해보자. 기존의 여러개의 contatiner를 시작하기 위해 docker run 명령어에 썼던 option들을 
전부 하나의 docker-compose.yml파일로 옮긴다고 보면된다.

이후 [Configuring Remote Interpreters via Docker](https://www.jetbrains.com/help/pycharm/2017.1/configuring-remote-interpreters-via-docker.html) 
링크를 참조하여 docker-compose를 이용하여 docker 이미지로 파이참의
remote 인터프리터를 설정한다. 설정 과정에서 Certificates-folder는 사용하지 않으면 비우고, Docker-compose executable을 설치한
docker-compose의 경로로만 지정해주면된다.

여기까지 설정됐다면 이제 가장 중요한 파이참 인터프리터를 이용한 run/debug 설정을 해야한다. 먼저 레퍼런스를 살펴보기 바란다.

**파이참 인터프리터를 이용한 run/debug 설정 레퍼런스**

- [Run/Debug Configuration: Django Test](https://www.jetbrains.com/help/pycharm/2017.1/run-debug-configuration-django-test.html)

- [PyCharm Professional 버전에서 Docker로 Remote Debugging](http://bryan7.tistory.com/864)

- [About network of Docker Container](https://stackoverflow.com/questions/36489696/cannot-link-to-a-running-container-started-by-docker-compose)

두번째 링크([PyCharm Professional 버전에서 Docker로 Remote Debugging](http://bryan7.tistory.com/864))를 주로 보면서, 첫번째 링크인
JetBrain공식 문서를([Run/Debug Configuration: Django Test](https://www.jetbrains.com/help/pycharm/2017.1/run-debug-configuration-django-test.html))
같이 보는 것이 좋다.
세번째 문서([About network of Docker Container](https://stackoverflow.com/questions/36489696/cannot-link-to-a-running-container-started-by-docker-compose))는
docker-compose를 사용하면서 변경되어야 하는 container의 network설정에 대한 내용이다.

세가지 문서를 모두 참조하면 인터프리터 설정이 가능할 것이다. 이제 편하게 디버깅하면서 개발할 수 있는 환경이 되었다.
아래는 예시로, 내가 Django앱에 설정한 Debug를 보여준다.

1. run/debug configuration 디테일<br />
빨간 box부분만 설정해주면 된다. 자세한 설명은 **파이참 인터프리터를 이용한 run/debug 설정 레퍼런스**의 두번째 링크를 참조한다.<br />
![run_debug_config](https://github.com/seongwoopark/seongwoopark.github.io/blob/master/img/1_run_debug_config.png?raw=true)<br /><br />
`Name`: configuration파일의 이름을 적당히 정해주면 된다.<br />
`Target`: 디버그를 실행할 Target파일의 경로를 정한다. Working Directory 아래에서부터의 경로를 정한다.<br />
`Environment variables`: 장고 app 실행시 필요한 환경변수를 설정한다. 아래 2번 이미지 참조.<br />
`Python Interpreter`: 위에서 docker-compose로 설정한 인터프리터를 설정한다.<br />
`Working Directory`: Working Directory를 설정한다. 나는 프로젝트 루트를 설정했다.<br />
`Path mappings`: 공유할 디렉토리의 Local Path(Host Path)와 Remote Path(Guest Path)를 설정한다.<br />
나는 Local의 프로젝트 루트와 Remote의 프로젝트 루트를 설정했다. 아래 3번이미지 참조.<br />
`Docker Container settings`: Docker container에 설정했던(docker-compose.yml에 설정했던) option들을 설정한다. 아래 4번이미지 참조.<br />

2. Environment variables 디테일<br />
![envrionment_vars](https://github.com/seongwoopark/seongwoopark.github.io/blob/master/img/2_envrionment_vars.png?raw=true)<br /><br />
`PYTHONBUFFERED`: 값은 1로 기본적으로 설정되어있다. 나는 유지했다.<br />

3. Path mappings 디테일<br />
![path_mappings](https://github.com/seongwoopark/seongwoopark.github.io/blob/master/img/3_path_maping.png?raw=true)<br /><br />

4. Docker Container settings 디테일<br />
![docker_contatiner_settings](https://github.com/seongwoopark/seongwoopark.github.io/blob/master/img/4_docker_contatiner_settings.png?raw=true)<br /><br />
`Network Mode`: **파이참 인터프리터를 이용한 run/debug 설정 레퍼런스**의 세번째 링크를 참조한다. docker-compose up 명령어를 실행 후
생성되는 container network의 이름을 설정한다. 이를 bridge나 다른 이름으로 설정할 경우 Links 부분에서 참조를 하지 못한다.<br />

이상으로 파이참에서 docker-compose를 이용하여 remote 인터프리터를 설정하고, 이를 이용하여 디버깅 설정하는 방법을 알아보았다.<br />
혹시 보시는 분들 중에 설정하다가 문제가 발생한다면, 자유롭게 댓글 남겨주시기 바랍니다.