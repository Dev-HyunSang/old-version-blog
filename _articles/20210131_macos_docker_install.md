---
id: 1
title: "MacOS에 Docker 설치하기"
subtitle: "MacOS에 Docker 설치하는 방법"
date: "2021.01.31"
tags: "Docker"
---

# Docker 설치하기 - Mac
Docker를 설치하는 방법에 대해서 알려드리겠습니다. Mac OS 사용자 분들에게만 해당하는 내용입니다. [brew](https://brew.sh/index_ko)을 이용해서 손 쉽게 설치하실 수 있습니다.    
```shell
$ brew install docker
```
위 명령어를 사용하여서 설치해 주시면 됩니다. 아주아주 간단하죠!

```shell
$ docker version
```
설치된 도커에 대해서 버전을 확인하면 됩니다. 확인이 되신다면 도커가 정상적으로 설치 되었습니다.

**서버를 실행시키려면 도커 애플리케이션이 필요합니다.**  
터미널을 이용해서 Docker Compose를 시작할 수 있지만 더 손 쉽게 시작할려면 도커 어플리케이션을 설치 해 주시면 됩니다.
[**Install Docker Desktop on Mac**](https://docs.docker.com/docker-for-mac/install/)로 들어가서 설치해 주시면 됩니다.

Docker Desktop을 설치하기 되면 손 쉽게 Docker를 실행, 정지, CLI 접속 등을 손 쉽게 할 수 있습니다.