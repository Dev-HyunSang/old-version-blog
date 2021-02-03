---
id: 3
title: "Docker 컨테이너 실행하기"
subtitle: "차근차근 Docker 컨테이너 실행해 보기"
date: "2021.02.03"
tags: "Docker"
---
# Docker 설치는 어떻게 하나요?
[**MacOS에 Docker 설치하기**](https://www.parkhyunsang.com/article/1.html)에서 MacOS에 Docker을 설치하는 방법에 대해서 다루었습니다.  
[**초보를 위한 도커 안내서 - 설치하고 컨테이너 실행하기**](https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html)에서 Linux에서 Docker을 설치하는 방법을 다루고 있습니다. Linux에서 Docker을 사용하시는 분들은 봐 보시길 바라겠습니다.

# 컨테이너 실행하기
드디어 Docker 설치를 끝내셨다면! 컨테이너의 위대함 그리고 강력함을 보여드리겠습니다.

도커를 실행하는 명령어는 아래와 같습니다.

```shell
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
```

|옵션|설명|
|:----------------:|:----------------:|
|-d|detached mode 흔히 말하는 백그라운드 모드
|-p|호스트와 컨테이너의 포트를 연결 (포워딩)|
|-v|호스트와 컨테이너의 디렉토리를 연결 (마운트)|
|-e|컨테이너 내에서 사용할 환경변수 설정|
|-name|컨테이너 이름 설정|
|–rm|프로세스 종료시 컨테이너 자동 제거|
|-it|-i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션|
|–link	|컨테이너 연결 [컨테이너명:별칭]|

직관적인 옵션으로 몇번 실행해보면서 자연스럽게 익숙해지게 됩니다.

## Ubuntu 18.04 Container
시작은 가볍게 Ubuntu 18.04 컨테이너를 생성하고 컨테이너 내부에 들어가는 방법에 대해서 알려 드리겠습니다.

```shell
$ docker run Ubuntu:18.04
```

```run``` 명령어를 사용하게 되면 사용할 이미지가 저장되어 있는지 확인합니다.  
만약 사용할 이미지가 없다면 다운로드(pull)를 하게 되며, 컨테이너를 생성(create)하고 시작(start)하게 됩니다.  

위 명령어를 실행해서 예제를 실행하게 되면 ```Ubuntu:18.04``` 이미지를 다운받은 적이 없기 때문에 이미지를 다운로드 한 후 컨테이너가 실행되었습니다. 컨테이너는 정상적으로 실행됐지만 뭘 하려고 명령어를 전달하지 않았기 때문에 컨테이너는 생성되자마자 종료됩니다.  
컨테이너는 프로세스이기 때문에 실행중인 프로세스가 없으면 컨테이너는 종료됩니다.

이번에는 ```/bin/bash``` 명령어를 입력해서 ```Ubuntu:18.04```를 실행해 보겠습니다.

```shell
➜  ~ docker run --rm -it ubuntu:16.04 /bin/bash
Unable to find image 'ubuntu:16.04' locally

➜  ~ docker run --rm -it ubuntu:18.04 /bin/bash
root@b3ee8a0c03f8:/# cat /etc/issue
Ubuntu 18.04.5 LTS \n \l

root@b3ee8a0c03f8:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
```

컨테이너 내부에 들어가기 위해 Bash 쉘을 실행하고 키보드 입력을 위해서 ```-it``` 옵션을 줍니다. 추기적으로 프로세스가 종료되면 컨테이너가 자동으로 삭제되도록 ```--rm``` 옵션도 추가하였습니다.

이번에는 바로 전에 이미지를 다운 받았기 때문에 이미지를 다운로드 하는 화면 없이 바로 실행 되었습니다. ```cat /etc/issue```와 ```ls```를 실행보면 Ubuntu 리눅스인 걸 알 수 있습니다. 아주아주 가벼운 가상머신 같죠?

```exit```로 bash 쉘을 종료하면 컨테이너도 같이 종료됩니다. 모든 OS에서 ```exit```를 입력하게 되면 터미널이 종료됩니다.

## redis container
2번째 컨테이너는 ```redis```입니다. [reids](https://redis.io/)는 메모리 기반의 다양한 기능을 가진 스토리지입니다. 6379 포트를 사용하여서 통신하면서 telnet 명령어로 테스트해 볼 수 있습니다. redis 컨테이너는 data ched mode(백그라운드 모드)로 실행하기 위해 ```-d``` 옵션을 추가하고 ```-p``` 옵션을 추가하여 컨테이너의 포트를 호스트의 포트로 연결해 보겠습니다.

```shell
➜  ~ docker run -d -p 1234:6379 redis
Unable to find image 'redis:latest' locally
latest: Pulling from library/redis
a076a628af6f: Already exists
f40dd07fe7be: Pull complete
ce21c8a3dbee: Pull complete
ee99c35818f8: Pull complete
56b9a72e68ff: Pull complete
3f703e7f380f: Pull complete
Digest: sha256:0f97c1c9daf5b69b93390ccbe8d3e2971617ec4801fd0882c72bf7cad3a13494
Status: Downloaded newer image for redis:latest
d026ce8b6789b1399d5bb7e7a856ca37a9ecc9ab7a83cd4c4a637da553f13292
```

```shell
➜  ~ telnet localhost 1234
Trying ::1...
Connected to localhost.
Escape character is '^]'.
set mykey hello
+OK
get mykey
$5
hello
```
```-d``` 옵션을 주었기 때문에 컨테이너를 실행하자마자 컨테이너의 ID를 보여주고 있습니다.
바로 쉘로 응답이 돌아왔기 때문에 컨테이터가 종료된 것이 아닌 백그라운드 모드를 동작하고 있고 컨테이너 ID를 이용하여서 컨테이너를 제어할 수 있습니다. ```-p``` 옵션을 이용하여서 호스트의 ```1234포트```를 컨테이너의 ```6379포트```로 연결 하였고 ```localhost:1234```로 접속하게 되면 redis를 이용할 수 있습니다.

테스트 결과로 redis에 접속하여서 새로운 키를 저장하고 불러오는 테스트를 진행하였습니다. 위 테스트를 성공하였습니다. 실행이 간단하고 물론이고 호스트의 포트만 다르게 하면 하나의 서버에 여러개의 redis 서버를 띄우는 것도 매우매우 간단하고 편합니다!

## MySQL 8.0 Container
다음으로 진행할 것은! 바로 ```MySQL 서버```입니다. 가장 흔하게 사용하는 데이터베이스 서버입니다. ```-e``` 옵션을 이용하여서 환경변수를 설정하고 ```--name``` 옵션을 이용하여 컨테이너 읽기 어려운 ID 대신 아주아주 읽기 쉬운 이름을 부여할 예정입니다!

[MySQL Docker hub](https://hub.docker.com/_/mysql/)에 접속하면 간단한 사용법과 환경변수에 대한 설명이 있습니다.  
여러가지 설정값이 있는데 패스워드 없이 root계정을 만들기 위해서 ```MYSQL_ALLOW_EMPTY_PASSWORD``` 환경변수를 설정합니다. 컨테이너의 이름의 ```mysql```로 할당하고 백그라운드 모드로 띄위기 위해서 ```-d``` 옵션을 줍니다.  
포트는 ```3306포트```를 호스트로 그대로 사용하였습니다.

```shell
➜  ~ docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  mysql:8.0
Unable to find image 'mysql:8.0' locally
8.0: Pulling from library/mysql
a076a628af6f: Already exists
f6c208f3f991: Pull complete
88a9455a9165: Pull complete
406c9b8427c6: Pull complete
7c88599c0b25: Pull complete
25b5c6debdaf: Pull complete
43a5816f1617: Pull complete
1a8c919e89bf: Pull complete
9f3cf4bd1a07: Pull complete
80539cea118d: Pull complete
201b3cad54ce: Pull complete
944ba37e1c06: Pull complete
Digest: sha256:feada149cb8ff54eade1336da7c1d080c4a1c7ed82b5e320efb5beebed85ae8c
Status: Downloaded newer image for mysql:8.0
a04ae9b255754275493ee459f37418052f8b66f28057fb36601c2f9b972bce34
```

```sehll
➜  ~ docker exec -it a04ae9b255754275493ee459f37418052f8b66f28057fb36601c2f9b972bce34 /bin/sh; exit

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

mysql> exit
```
순식간에 MySQL 서버가 설치되어서 실행이 되었습니다. 이번 테스트는 호스트 OS에서 MySQL 클라이언트가 설치 되어 있어야 합니다.(**저는 클라이언트 설치가 힘들어서 도커 내부에서 진행하였습니다.**)  

처음 접속시 에러가 날 수도 있습니다, MySQL 서버가 최초로 실행되면서 준비 작업을 하기 위해서 발생되는 에러입니다. 조금 기달리시고 접속을 하시게 되면 접속이 되실 겁니다.

## WordPress Container
이번에는 유명한 워드프로세스를 실행해 보겠습니다. 워드프로세스는 데이터베이스가 필요하기 때문에 조금 복잡한 형태를 띄지만 크게 어렵지 않습니다. 바로 전에 생성했던 MySQL 컨테이너에 워드프로세스 데이터베이스를 만들고 WordPress 컨테이너를 실행할 때 ```--link``` 옵션을 이용하여서 MySQL 컨테이너와 연결해 보겠습니다.

```--link``` 옵션은 환경변수와 IP 정보를 공유하는데 링크한 컨테이너의 IP 정보를 ```/etc/hosts```에 자동으로 입력하므로 워드프로세스 컨테이너가 MySQL 데이터베이스의 정보를 알 수 있게 됩니다.

워드프로세스용 데이터베이스를 생성하고 워드프레스 컨테이너를 실행합니다. 호스트의 ```8080포트```를 컨테이너의 ```80포트```로 연결하고 MySQL 컨테이너와 연결한 후 각종 데이터베이스 설정 정보를 환경변수로 입력합니다.

```shell
$ mysql -h127.0.0.1 -uroot
create database wp CHARACTER SET utf8;
grant all privileges on wp.* to wp@'%' identified by 'wp';
flush privileges;
quit

docker run -d -p 8080:80 \
  --link mysql:mysql \
  -e WORDPRESS_DB_HOST=mysql \
  -e WORDPRESS_DB_NAME=wp \
  -e WORDPRESS_DB_USER=wp \
  -e WORDPRESS_DB_PASSWORD=wp \
  wordpress
```
컨테이너가 제대로 실행되었는지 웹 브라우저로 확인해봅니다.

워드프레스가 실행되었습니다! 단지 이미지를 다운받고 적절한 환경변수를 입력하여 컨테이너를 실행했을 뿐입니다. 워드프레스 컨테이너 내부는 apache2와 php가 설치되어 있지만 추상화되어 있어 실행과정에선 드러나지 않습니다.

# 도커 기본 명령어
## 컨테이너 목록 확인하기(ps)
컨테이너 목록을 확인하는 명렁어는 아래와 같습니다.
```shell
docker ps [OPTIONS]
```
일단 기본적인 옵션인 ```-a, --all``` 옵션만 살펴 보겠습니다.

**Output:**
```shell
➜  ~ docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED        STATUS         PORTS                               NAMES
a04ae9b25575   mysql:8.0   "docker-entrypoint.s…"   22 hours ago   Up 6 seconds   0.0.0.0:3306->3306/tcp, 33060/tcp   mysql
c7b3011f6508   nginx       "/docker-entrypoint.…"   45 hours ago   Up 1 second    0.0.0.0:80->80/tcp                  nginx-server
```
```ps``` 명령어의 경우에는 실행중인 컨테이너의 목록을 보여줍니다. [detached mode(백그라운드에서 작동하는 컨테이너)](https://stackoverflow.com/questions/34029680/docker-detached-mode)로 실행중인 컨테이너들이 보입니다. 어떤 이미지 기반으로 만들어졌고 어떤 포트와 연결이 되어 있는지 등의 간단한 내용을 보여줍니다.

이번에는 ```-a``` 옵션을 추가로 실행 해 보겠습니다!

**Output:**
```shell
docker ps -a
```

```shell
➜  ~ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED        STATUS                     PORTS                               NAMES
a04ae9b25575   mysql:8.0      "docker-entrypoint.s…"   22 hours ago   Up 3 minutes               0.0.0.0:3306->3306/tcp, 33060/tcp   mysql
d026ce8b6789   redis          "docker-entrypoint.s…"   29 hours ago   Exited (255) 8 hours ago   0.0.0.0:1234->6379/tcp              wonderful_swartz
df8bea7fa16e   ubuntu:18.04   "/bin/bash"              33 hours ago   Exited (255) 8 hours ago                                       loving_turing
e4aa9d812ea3   ubuntu:18.04   "/bin/bash"              33 hours ago   Exited (0) 33 hours ago                                        frosty_jackson
c7b3011f6508   nginx          "/docker-entrypoint.…"   45 hours ago   Up 3 minutes               0.0.0.0:80->80/tcp                  nginx-server
```

맨 처음 실행했다가 종료된 컨테이너(Exited (0))이 추가로 보입니다.  
컨테이너는 종료되어도 삭제가 되지 않습니다. 종료된 건 다시 시작할 수 있으며 컨테이너의 읽기/쓰기 레이어는 그대로 존재하게 됩니다.  
명시적으로 삭제를 하면 깔끔하게 컨테이너가 제거 되게 됩니다. 따라서

## 컨테이너 중지하기(stop)
실행중인 컨테이너를 중지하는 명령어는 아래와 같습니다.

```shell
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```
옵션은 특별한 건 없습니다, 실행중인 컨테이너를 하나 또는 여러개를 중지할 수 있습니다.

앞에서 실행하고 있는 컨테이너 중 하나를 선택해서 중지 시켜 보겠습니다.  
컨테이너 ID를 이용하거나 이름을 입력하면 됩니다. 그럼 저는 MySQL의 컨테이너 ID과 ```ps``` 명령어를 이용해서 확인하고 중지 시켜 보겠습니다.

```
➜  ~ docker ps
CONTAINER ID   IMAGE       COMMAND                  CREATED        STATUS          PORTS                               NAMES
a04ae9b25575   mysql:8.0   "docker-entrypoint.s…"   22 hours ago   Up 13 minutes   0.0.0.0:3306->3306/tcp, 33060/tcp   mysql
c7b3011f6508   nginx       "/docker-entrypoint.…"   45 hours ago   Up 13 minutes   0.0.0.0:80->80/tcp                  nginx-server
➜  ~ docker stop a04ae9b25575
a04ae9b25575
➜  ~ docker ps -a
```
잠시 동안 기달리게 되면 선택하신 컨테이너가 종료가 됩니다. ```ps -a``` 명령어를 입력하여 종료가 되었는지 확인합니다.

## 컨테이너 제거하기 (rm)
종료된 컨테이너를 완전히 제거하는 명령어는 아래와 같습니다.

```shell
docker rm [OPTIONS] CONTAINER [CONTAINER...]
```

종료 명령어와 비슷하게 제거 명령어도 특별한 옵션은 없습니다. 종료된 컨테이너를 하나 또는 여러개 삭제할 수 있습니다. 종료된 nginx 컨테이너와 mysql 컨테이너를 삭제해 보겠습니다.

```shell
➜  ~ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED        STATUS                     PORTS                    NAMES
a04ae9b25575   mysql:8.0      "docker-entrypoint.s…"   22 hours ago   Exited (0) 6 minutes ago                            mysql
d026ce8b6789   redis          "docker-entrypoint.s…"   29 hours ago   Exited (255) 8 hours ago   0.0.0.0:1234->6379/tcp   wonderful_swartz
df8bea7fa16e   ubuntu:18.04   "/bin/bash"              33 hours ago   Exited (255) 8 hours ago                            loving_turing
e4aa9d812ea3   ubuntu:18.04   "/bin/bash"              34 hours ago   Exited (0) 33 hours ago                             frosty_jackson
c7b3011f6508   nginx          "/docker-entrypoint.…"   45 hours ago   Up 19 minutes              0.0.0.0:80->80/tcp       nginx-server
➜  ~ docker rm a04ae9b25575 c7b3011f6508
a04ae9b25575
c7b3011f6508
```

컨테이너가 말금히 삭제되었습니다. 호스트 OS는 아무런 흔적도 남아있지 않고 컨테이너만 격리된 상태로 실행되었다가 삭제 되었습니다. 시스템이 꼬일 걱정이 없습니다! 아주 좋아요 ㅠㅠ

## 이미지 목록 확인하기 (images)
도커가 다운로드한 이미지 목록을 보는 명령어는 다음과 같습니다. 따라서

```shell
docker images [OPTIONS] [REPOSITORY[:TAG]]
```

간단하게 도커 이미지 목록을 확인해보겠습니다.

```shell
docker images
```

**Output:**
```shell
➜  ~ docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       18.04     c090eaba6b94   11 days ago   63.3MB
mysql        8.0       c8562eaf9d81   13 days ago   546MB
redis        latest    621ceef7494a   2 weeks ago   104MB
nginx        latest    f6d0b4767a6c   2 weeks ago   133MB
```

이미지 주소와 태그, ID, 생성시점, 용량이 보입니다. 이미지가 너무 많이 쌓이면 용량을 차지하기 때문에 사용하지 않는 이미지는 지우것이 좋습니다.

## 이미지 다운로드 하기 (pull)
이미지를 다운로드하는 명령어는 다음과 같습니다.

```shell
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

```ubuntu:20.04```를 다운받아보겠습니다.

```shell
docker pull ubuntu:20.04
```

```shell
➜  ~ docker pull ubuntu:20.04
20.04: Pulling from library/ubuntu
83ee3a23efb7: Pull complete
db98fc6f11f0: Pull complete
f611acd52c6c: Pull complete
Digest: sha256:703218c0465075f4425e58fac086e09e1de5c340b12976ab9eb8ad26615c3715
Status: Downloaded newer image for ubuntu:20.04
docker.io/library/ubuntu:20.04
```

```run``` 명령어를 입력하면 이미지가 없을 때 자동으로 다운받으니 ```pull``` 명령어를 언제 쓰는지 궁금할 수 있는데 ```pull```은 최신버전으로 다시 다운 받습니다. 같은 태그지만 이미지가 업데이트 된 경우는 ```pull``` 명령어를 통해 새로 다운받을 수 있습니다.

## 컨테이너 산책하기
도커에 대한 아주아주아주아주아주 기본적인 명령어를 위에서 살펴보았습니다.
사실 위에서 다룬 명령어들과 이번 살펴볼 ```log```, ```exec``` 명령어를 익히면 도커에서 사용하는 명령어는 거의 다 익혔다고 할 수 있습니다. 다른 명령어는 필요에 따라서 하나하나 살펴보기만 하시면 됩니다!

### 컨테이너 로그 보기 (logs)
컨테이너가 정상적으로 동작하는지 확인하는 좋은 방법은 로그를 확인하는 것입니다. 로그를 확인하는 방법은 아래와 같습니다!

```shell
docker logs [OPTIONS] CONTAINER
```
기본 옵션과 ```-f```, ```--tail``` 옵션을 살펴보겠습니다.

기존에 생성해 놓은 redis 컨테이너 로그를 확인해 보겠습니다.

```shell
docker ps
docker logs ${REDIS_CONTAINER_ID}
```

**Output:**
```shell
➜  ~ docker logs f44867afeff6
1:C 01 Feb 2021 12:54:09.581 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 01 Feb 2021 12:54:09.581 # Redis version=6.0.10, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 01 Feb 2021 12:54:09.582 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
                _._
           _.-``__ ''-._
      _.-``    `.  `_.  ''-._           Redis 6.0.10 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 1
  `-._    `-._  `-./  _.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |           http://redis.io
  `-._    `-._`-.__.-'_.-'    _.-'
 |`-._`-._    `-.__.-'    _.-'_.-'|
 |    `-._`-._        _.-'_.-'    |
  `-._    `-._`-.__.-'_.-'    _.-'
      `-._    `-.__.-'    _.-'
          `-._        _.-'
              `-.__.-'

1:M 01 Feb 2021 12:54:09.584 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 01 Feb 2021 12:54:09.585 # Server initialized
1:M 01 Feb 2021 12:54:09.586 * Ready to accept connections
```

컨테이너에서 실행한 로그가 쭈욱 보입니다. 아무 옵션을 주지 않았을 때는 전체 로그를 무시하게 전부 다 출력합니다. 너무 많으면 보기 어렵죠?  
```--tail``` 옵션으로 마지막 10줄만 출력해 보겠습니다.

```shell
docker logs --tail 10 ${DOCKER_CONTAINER_ID}
```

# 컨테이너 명령어 실행하기 (exec)
컨테이너를 관리하다 보면 실행중인 컨테이너에 들어가보거나 컨테이너의 파일을 실행하고 싶을 때가 있습니다. 컨테이너에 ```SSH```를 설치하면 되지 않을까?라는 생각을 할 수도 있습니다. 하지만 ```SSH```는 권장하지 않습니다, 예전에는 nsenter라는 프로그램을 이용하였는데 Docker에 ```exec```라는 명령어에 흡수가 되었습니다.

```shell
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

다른 명령어인 ```run``` 명령어와 유사합니다. 차이는 ```run```은 새로 컨테이너 만들어서 실행하게 되고 ```exec```는 실행중인 컨테이너에 명령어를 내리는 정도입니다.

일단, 가볍게 실행중인 ```MySQL``` 컨테이너에 접속해보겠습니다.

**실행할려고 하였지만 오류가 나옵니다. 왜 오류가 나올까요?**  
Facebook Group Korea Docker User Group에 게시물을 올려서 친절하게 해결 하였습니다. 
```shell
➜  ~ docker run mysql:8.0
2021-02-02 13:13:08+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.23-1debian10 started.
2021-02-02 13:13:08+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2021-02-02 13:13:08+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.23-1debian10 started.
2021-02-02 13:13:08+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
	You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD
```

위와 같이 ```You need to specify one of MYSQL_ROOT_PASSWORD, MYSQL_ALLOW_EMPTY_PASSWORD and MYSQL_RANDOM_ROOT_PASSWORD```라고 뜨면서 안 되는 경우에는 아래 명령어로 실행을 시키고 다시 접속하게 되면 정상적으로 들어가지게 됩니다.

```shell
docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  mysql:8.0
```

# 컨테이너 업데이트는 어떻게 해요?
기존에 있던 컨테이너를 새로운 버전으로 업데이트는 하는 과정을 살펴 보겠습니다.

![](https://subicura.com/assets/article_images/2017-01-19-docker-guide-for-beginners-2/container-update.png)

도커에서 컨테이너를 업데이트 하려고 하면 새 버전의 이미지를 다운(Pull) 받고 기존의 컨테이너를 삭제(Stop, rm)한 후 새로운 이미지를 기반으로 새로운 컨테이너를 실행(run)하면 됩니다.  

컨테이너를 삭제한다는 건! 컨테이너에서 생성된 파일이 사라진다는 뜻이고 데이터베이스라면 그 동안 쌓였던 데이터가 모두 사라진다는 것이고 웹 애플리케이션이라면 그 동안 사용자가 업로드한 이미가 모두 사라진다는 것...

이런 상황에서 도커 도임했다가 퇴사를 방지하기 위해서 컨테이너 삭제지 유지해야하는 데이터는 반드시 컨테이너 내부가 아닌 외부 스토리지에 저장해야 합니다. 가장 좋은 방법은! AWS S3 같은 클라우드 서비스를 이용하고 그렇지 않으면!! 데이터 볼륨을 컨테이너에 추가하여서 사용해야 합니다. 데이터 볼륨을 사용하면 해당 디렉토리는 컨테이너와 별도로 저장되고 컨테이너를 삭제하게 되더라도! 데이터가 지워지지 않습니다.

데이터 볼륨을 사용하는 방법은 몇가지가 있는데 여기서는 호스트의 디렉토리를 마운트해서 사용하는 방법에 대해 알아보겠습니다! ```run``` 명령어에서 소개한 옵션중에 ```-v``` 옵션을 드디어 사용해 보도록 하겠습니다! MySQL이라면 ```/var/lib/mysql``` 디렉토리에 모든 데이터베이스 정보가 담기므로 호스트의 특정 디렉토리를 연결해주면 됩니다.

```shell
# before
docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  mysql:5.7

# after
docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  -v /my/own/datadir:/var/lib/mysql \ # <- volume mount
  mysql:5.7
```

위 샘플은 호스트의 ```/my/own/datadir``` 디렉토리를 컨테이너의 ```/var/lib/mysql``` 디렉토리로 마운트 하였습니다. 이제 데이터베이스 파일은 호스트의 ```/my/own/datadir``` 디렉토리에 저장되고 컨테이너를 삭제해도 데이터는 사라지지 않습니다. 최신버전의 MySQL 이미지를 다운받고 다시 컨테이너를 실행할 때 동일한 디렉토리를 마운트하게 되면 그대로 데이터를 사용할 수 있습니다.

# Docker Compose
지금까지는 도커를 커맨드라인에서 명령어를 통해서 작업하였습니다. 방금 전까지 했던 작업들은 엄청나게 간단한 작업을 하였기 때문에 명령어도 길지 않지만 컨테이너 조합이 많아지고 여러가지 설정이 추가되면 명령어가 금방 복잡해집니다.  

도커는 복잡한 설정을 쉽게 관리하기 위해 YAML 방식의 설정파일을 이용한 Docker Compose라는 툴을 제공합니다. 깊게 파고들면 은근 기능이 많고 복잡한데 이번에는 아주 가볍게 다루고 지나가도록 하겠습니다.

## 설치하기
Docker for Mac 또는 Docker for Window를 설치 하였다면 자동적으로 설치가 되셨을 겁니다. 리눅스의 경우에는 아래와 같은 명령어를 사용하셔서 설치 하실 수 있습니다.

```shell
curl -L "https://github.com/docker/compose/releases/download/1.9.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
docker-compose version
```

## wordpress 만들기
기존에 명령어로 만들었던 wordpress를 compose를 이용해 만들어 보겠습니다. 

먼저 빈 디렉토리를 하나 만들고 ```docker-compose.yml```파일을 만들어 설정을 입력합니다.

```yml
version: '2'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: wordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     volumes:
       - wp_data:/var/www/html
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_PASSWORD: wordpress
volumes:
    db_data:
    wp_data:
```

우와우와... 아주아주 손쉽게 워드프로세스가 만들어졌습니다. 단지 명령어를 설정파일로 바꿨을 뿐인데 가독성과 편리성이 향상되었습니다.

Docker Compose의 다른 기능과 생소한 설정 내용들은 아직까지 공부해야할 내용입니다. 스스로 한 번 공부 해 보세요!
도커에 대해서 이해를 하셨다면! Docker Compose 또한 쉽게 사용할 수 있을 것입니다.

# 참고하면 좋은 문서 또는 출처
[초보를 위한 도커 안내서 - 설치하고 컨테이너 실행하기](https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html)를 보고 정리한 내용입니다. 

- [Docker Docs-Compose file](https://docs.docker.com/compose/compose-file/)
- [Microsoft-Docker Compose 사용](https://docs.microsoft.com/ko-kr/visualstudio/docker/tutorials/use-docker-compose)