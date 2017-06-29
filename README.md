# seongwoopark.github.io

> *Copyright 2017 [SeongWoo Park(https://seongwoopark.github.io/)*

**This repository is used as a [my personal website](https://seongwoopark.github.io/) for the resume, portfolio and blog.**

Basically, it is based on Jekyll which is a simple, blog-aware, static site generator. Jekyll is a package from Ruby language.
To make this repository, I forked [Beautiful Jekyll](https://github.com/daattali/beautiful-jekyll) theme. This theme is very useful template to blog.
I also used [GraphBerry](http://www.graphberry.com/) template for the landing page of my web site and to custom basic style of Beautiful Jekyll theme.

If you visit [my personal website](https://seongwoopark.github.io/), you can see my personal logo. It is designed by [SungHo Lim](http://color-lim.com/). Specially thanks.

**CAUTION!**
Do not fork this repository. Please just refer this if you want. I recommend you to fork [Beautiful Jekyll](https://github.com/daattali/beautiful-jekyll) theme.


### Development
My jekyll site has been developed on the following environment.

- Virtual Environment Tool: Docker CE(Community Edtion)
- Host OS: Ubuntu 16.04 LTS
- Container OS: Alpine Linux
- Container image: jekyll/jekyll:latest

FYI, Cheetsheet:
```
git clone https://github.com/seongwoopark/seongwoopark.github.io.git

cd seongwoopark.github.io.git
```
For instant disposable docker container
```
docker run --rm --label=jekyll --volume=$(pwd):/srv/jekyll -it -p 127.0.0.1:4000:4000 jekyll/jekyll jekyll serve
```
For reusable docker container
```
docker run --label=jekyll --volume=$(pwd):/srv/jekyll -it -p 127.0.0.1:4000:4000 jekyll/jekyll /bin/bash

jekyll serve
```
And then, check it at [http://localhost:4000](http://localhost:4000)


### Reference docs

- Official Guide Documents - [Jekyll Docs](https://jekyllrb.com/)
- Free Templates - [GraphBerry](http://www.graphberry.com/)
- Useful Jekyll blog theme - [Beautiful Jekyll](https://github.com/daattali/beautiful-jekyll)
- Official Docker - [Docker](https://www.docker.com/)
- Docker image for Jekyll - [Repository](https://github.com/jekyll/docker)
- Wiki Docs of Docker image for Jekyll - [Jekyll Docker wiki](https://github.com/jekyll/docker/wiki)
