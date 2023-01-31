---
layout: post
title:  Docker image 기본 명령어
date:   2023-01-31 15:24
image:  
tags:   Docker
---
## 01. Docker image와 Dockerfile

- **Docker image**: 코드뿐만 아니라 프로그램에 의존한 모든 것을 함께 패키징한 데이터이다.  
- **Dockerfile**: Docker image를 쉽게 만들어질 수 있도록 제공하는 템플릿이다.  
<img src="/images/docker_image.png" width="80%">  
위의 그림이 Docker image와 Dockerfile을 쉽게 설명되어 있는 이미지이다.  
Dockerfile을 Docker image로 빌드를 하고 image를 실행한 상태가 바로 Docker container이다.  ([이미지 출처: 오웬의 개발 이야기](https://devowen.com/249))
<br/>
<br/>

***
## 02. Dockerfile 

1) Dockerfile 만들기
- 임의의 폴더 안에 Dockerfile이라는 빈 파일을 만든다.  

```bash
# Dockerfile 이라는 빈 파일을 생성합니다.
$ touch Dockerfile
```
<br/>

2) Dockerfile 기본 명령어
- FROM
    - Dockerfile 이 base image로 어떠한 이미지를 사용할 것인지를 명시하는 명령어이다.  
base image를 이용하여 이후로 이미지를 만드는 것이다.   

```docker
FROM <image>[:<tag>] [AS <name>]

# 예시
# ubuntu라는 이미지를 불러온다.
FROM ubuntu
FROM ubuntu:18.04
FROM nginx:latest AS ngx
```  
<br/>

- COPY
    - \<file>의 파일 또는 디렉토리를 \<dest> 경로에 복사하는 명령어이다.

```docker
COPY <file>... <dest>
# 예시
# a.txt란 파일을 /some-directory/경로에 b.txt라는 이름으로 복사한다.
COPY a.txt /some-directory/b.txt
# my-directory라는 디렉토리를 some-directory-2라는 경로에 복사를 한다.
COPY my-directory /some-directory-2
```
<br/>

- RUN
    - 커맨드를 도커 컨테이너에서 실행하는 것을 명시하는 명령어이다.

```docker
RUN <command>
RUN ["executable-command", "parameter1", "parameter2"]

# 예시
# 파이썬 패키지인 torch를 설치하는 커맨드
RUN pip install torch
RUN pip install -r requirements.txt
```
<br/>

- CMD
    - 커맨드를 도커 컨테이너가 시작될 때 실행하는 것을 명시하는 명령어이다.
    - 도커 이미지 하나당 하나의 CMD만 실행할 수 있다는 점에서 RUN 명령어와 다르다.

```docker
CMD <command>
CMD ["executable-command", "parameter1", "parameter2"]
CMD ["parameter1", "parameter2"] # ENTRYPOINT 와 함께 사용될 때
# 예시
# 컨테이너가 실행될 때 main.py를 실행한다.
CMD python main.py
```
<br/>

- WORKDIR
    - 이후에 작성될 명령어를 컨테이너 내의 어떤 디렉토리에 수행할 것인지를 명시하는 명령어이다.

```docker
WORKDIR /path/to/workdir
# 예시
WORKDIR /home/demo
```
<br/>

- ENV
    - 컨테이너 내부에서 지속적으로 사용될 environment variable 값을 설정하는 명령어이다.

```docker
ENV <key> <value>
ENV <key>=<value>
# 예시
# default 언어 설정
RUN locale-gen ko_KR.UTF-8
ENV LANG ko_KR.UTF-8
ENV LANGUAGE ko_KR.UTF-8
ENV LC_ALL ko_KR.UTF-8
```
<br/>

- EXPOSE
    - 컨테이너에서 뚫어줄 포트/프로토콜을 지정할 수 있다.
    - 프로토콜을 지정하지 않으면 TCP가 default로 지정된다.

```docker
EXPOSE <port>
EXPOSE <port>/<protocol>
# 예시
EXPOSE 8080
```
<br/>

- Dockerfile 작성 예시  

```docker
# base image 를 ubuntu 18.04 로 설정한다.
FROM ubuntu:18.04
# 도커 컨테이너에서 apt-get update 명령을 실행한다.
RUN apt-get update
# 도커 컨테이너가 시작될 때, "Hello FastCampus" 를 출력합니다.
# echo는 "Hello FastCampus"를 그대로 출력을 하는 명령어이다.
CMD ["echo", "Hello FastCampus"]
```
<br/>

***
## 03. Dockerfile을 Docker image로 bulid하고 container로 run하기
- dockerfile을 image로 build하기
    - `docker build`으로 build가 가능하다.  

```bash
$ docker build 
# 현재 경로에 있는 모든 파일(.)을 my-image라는 이름과 v1.0.0라는 태그로 이미지를 빌드를 한다.
$ docker build -t my-image:v1.0.0 .
```
<br/>
- image로 container를 run하기
    - `docker run`으로 run이 가능하다.

```bash
# docker run <image 이름>
$ docker run 
# my-image:v1.0.0라는 이미지를 run
$ docker run my-image:v1.0.0
```
<br/>

***
## 04. Docker image 저장소  
- Docker image 저장소에는 대표적으로 Docker registry와 Docker HUB가 있다. 
- Docker registry는 사용자가 registry를 구축하여 내부망에서 사용하거나 외부망과 연결하여 사용할 수 있다.
- Docker HUB는 GitHub처럼 Official하게 공유하는 것이다.

1) Docker registry
```bash
$ docker run -d -p 5000:5000 --name registry registry

$ docker ps
# 정상적으로 registry 이미지가 registry 라는 이름으로 생성되었음을 확인할 수 있습니다.
# localhost:5000 으로 해당 registry 와 통신할 수 있습니다.

# my-image 를 방금 생성한 registry 를 바라보도록 tag 합니다.
$ docker tag my-image:v1.0.0 localhost:5000/my-image:v1.0.0

$ docker images | grep my-image
# localhost:5000/my-image:v1.0.0 로 새로 생성된 것을 확인할 수 있습니다.

# my-image 를 registry 에 push 합니다. (업로드합니다.)
$ docker push localhost:5000/my-image:v1.0.0

# push가 되었는지 확인한다.
# localhost:5000 이라는 registry 에 어떤 이미지가 저장되어 있는지 리스트를 출력하는 명령
$ curl -X GET http://localhost:5000/v2/_catalog

# 출력 : {"repositories":["my-image"]}

# my-image 라는 이미지 네임에 어떤 태그가 저장되어있는지 리스트를 출력하는 명령
$ curl -X GET http://localhost:5000/v2/my-image/tags/list

# 출력 : {"name":"my-image","tags":["v1.0.0"]}
```
<br/>
<br/>
\<출처\>  
[Docker image 이미지 출처: 오웬의 개발 이야기](https://devowen.com/249)<br/>
[코드 출처: fastcampus](https://jaeyeon-kim.notion.site/Docker-1-2-be5f796e34ac4903af0c97e59f1eb98e)