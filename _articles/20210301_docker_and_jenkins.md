---
id: 8
title: "Docker기반의 Jenkins 설치하기"
subtitle: "Docker를 통한 Jenkins 설치하기" 
date: "2021.03.01"
tags: "Docker, Jenkins"
---
안녕하세요, 요즘 DevOps에 관심이 생겨서 Docker에 Jenkins를 설치하고 구동하는 방법에 대해서 알려드리겠습니다. Back-End를 하시는 분들도 어느 정도 DevOps를 하실 줄 아시는 걸로 알고 있습니다.

# Jenkins를 무엇일까요?
젠킨스는 소프트웨어 개발 시 지속적 통합 서비스를 제공하는 툴입니다. CI(Continuous Integration)를 하기 위한 일종의 툴이라고 생각하시거나 표현하시면 됩니다. 다수의 개발자들이 하나의 프로그래밍를 개발할 때 버전 충돌를 방지하기 위해서 각자 작업한 내용을 공유 영역에 있는 저장소에 빈번히 업로드함으로써 지속적 통합이 가능하도록 해 줍니다.  
다른 CI 툴의 경우에는 일정시간마다 빌드를 실행하는 방식이 일반적이었습니다.  
하지만 Jenkins는 정깆거인 빌드에서 한발 나아가서 서브버전으로 Git과 같은 버전관리시스템과 연동하여서 소스의 커밋을 감지하게 되면 자동적으로 자동화 테스트가 포함된 빌드가 작동되도록 설정할 수 있습니다.

# Install Jenkins image
![images](https://raw.githubusercontent.com/Dev-HyunSang/Dev-HyunSang/master/images/20210301_docker/01.png)

```shell
$ docker pull jenkins/jenkins
```
위 명령어를 사용해서 Docker에 Jenkins Image를 설치해 주셔야 합니다.  

![images](https://raw.githubusercontent.com/Dev-HyunSang/Dev-HyunSang/master/images/20210301_docker/02.png)

```shell
$ docker images
```
위 명령어를 통해서 이미지가 제대로 설치가 되었는지 확인 해 주시면 됩니다.

# Docker Container에서 실행
이미지가 제대로 설치가 되었는지 확인하셨다면! 이미지를 이용해서 컨테이너를 만들고 컨테이너에서 Jenkins를 실행 해 주시면 됩니다. 

![images](https://raw.githubusercontent.com/Dev-HyunSang/Dev-HyunSang/master/images/20210301_docker/03.png)

```shell
$ docker run -d -p 8080:8080 jenkins/jenkins
```

위 명령어를 통해서 Jenkins를 실행해 주시면 됩니다!  

# Jenkins 설정하기
![images](https://raw.githubusercontent.com/Dev-HyunSang/Dev-HyunSang/master/images/20210301_docker/07.png)

구동한 Dokcer Server의 IP 주소를 웹 브라우저에 입력하시면 Jenkins으로 들어가실 수 있습니다.  
이제 **Unlock Jenkins**라고 뜹니다.


![images](https://raw.githubusercontent.com/Dev-HyunSang/Dev-HyunSang/master/images/20210301_docker/06.png)

```sehll
$ docker logs CONTAINER ID
```

Jenkins Log를 보게 되면 비밀번호가 나오게 됩니다.  

# Jenkins Plugins Install
![images](https://raw.githubusercontent.com/Dev-HyunSang/Dev-HyunSang/master/images/20210301_docker/08.png)

Jenkins Plugins 설치해 주셔야 합니다.  
사진 기준으로 오른쪽 선택시 **자신이 하나하나 선택해서 플러그인을 설치**합니다.  
윈쪽 선택시 **기본적인 플러그인을 설치**합니다.

![images](https://raw.githubusercontent.com/Dev-HyunSang/Dev-HyunSang/master/images/20210301_docker/09.png)


# Jenkins 계정 설정
계정 설정하는 이미지를 모르고 캡처를 안 했습니다.

- 계정명
- 암호
- 암호확인
- 이름
- 이메일 주소

위 항목들을 입력하시게 되면 정상적으로 Jenkins를 사용하실 수 있습니다.

# 참고하거나 읽어보면 좋은 자료
- [Jenkins 설치하기 (docker 기반)](https://velog.io/@king/Jenkins-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-with-docker-ilk5j8g02g)
- [Docker 를 이용한 CI 구축 -1 ( Docker jenkins 설치)](https://beomseok95.tistory.com/177)
- [Docker를 통한 젠킨스(Jenkins) 설치하기.](http://jmlim.github.io/docker/2019/02/25/docker-jenkins-setup/)