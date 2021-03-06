
# Docker

## Docker란

도커는 컨테이너 기반의 오픈소스 가상화 플랫폼

### 컨테이너

* 컨테이너는 격리된 공간에서 프로세스가 동작하는 기술
* 가상화 기술의 하나이지만 기존 방식과는 차이가 있음

기존의 가상화 방식은 OS 가상화 방식, 예를들어 VMWare나 VirtualBox

간단하지만 너무 무겁고 느리기 때문에 운영에 쓸 수 없음..

이러한 문제점을 해결하고자 CPU 가상화 기술(`HVM`)을 이용한 Kernel-based Virtual Machine과 반가상화 기술이 등장

이러한 기술들은 OpenStack이나 AWS와 같은 클라우드 서비스의 기반 기술이 되었음


![1](/assets/1.png)

리눅스에서는 이 방식을 리눅스 컨테이너라고 하고 **단순히 프로세스를 격리시키기 떄문에 가볍고 빠르게 동장합니다.**

그렇기 떄문에 **하나의 서버에 여러개의 컨테이너를 실행하면 서로 영향을 미치지 않고 독립적으로 실행되어** 마치 가벼운 VM을 사용하는 느낌을줍니다.

새로운 컨테를 만드는 시간은 1~2초로 매우 빠릅니다.

새로 만들어진 컨테이너에서는 다양한 리눅스 명령어를 그대로 사용할 수 있습니다. (ex wsl)

### 이미지(image)

도커에서 이미지란 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것

상태값을 가지고 있으며 변하지 않은 것이 특징

컨테이너란 이 이미지를 실행한 상태라고 볼 수 있으며, 이후 추가되거나 변화되는 값은 컨테이너에 저장됩니다.

같은 이미지라도 여러개의 컨테이너를 생성할 수 있고, 컨테이너의 상태가 바뀌거나 컨테이너가 삭제되더라도 역시 이미지는 변하지 않고 그대로 남아있습니다.

말 그대로 이미지는 컨테이너를 실행하기 위한 모든 정보를 가지고 있기 때문에 더이상 의존성 파일을 컴파일하고 이것저것 설치할 필요가 없습니다.

이제 새로운 서버가 추가되면 미리 만들어 놓은 이미지를 다운받고 컨테이너를 생성만 하면 됩니다. 한 서버에 여러개의 컨테이너를 실행할 수 있고, 수십, 수백, 수천대의 서버도 문제없습니다.

도커 이미지는 [Docker Hub](https://hub.docker.com/)나 [Docker Registry](https://docs.docker.com/registry/)에 저장소를 직접 만들어 관리할 수 있습니다.


### 도커가 빠른 이유

도커 이미지는 컨테이너를 실행하기 위한 모든 정보를 가지고 있기 때문에 보통 용량이 수백메가에 이릅니다. 처음 이미지를 다운 받을 때는 부담이 안되지만, 기존 이미지 파일에 파일 하나 추가했다고, 수백 메가를 다운 받기에는 부담이 너무 큽니다.

이러한 문제를 해결하고자 도커에는 **레이어** 라는 개념을 사용하고 유니온 파일 시스템을 이용하여 여러개의 레이어를 하나의 파일 시스템으로 사용할 수 있게 해줍니다.

즉 A + B + C 에  D가 하나 추가되면 D 레이어만 다운받으면 되기 때문에 굉장히 효율적으로 이미지를 관리할 수 있습니다.

컨테이너를 생성할 때도 레이어 방식을 사용하는데 기존의 이미지 레이어 위에 읽기/쓰기 레이어를 추가합니다. 이미지 레이어를 그대로 사용하면서 컨테이너가 실행중에 생성하는 파일이나 변경된 내용은 읽기/쓰기 레이어에 저장되므로 여러개의 컨테이너를 생성해도 최소한의 용량만 사용합니다.

### 이미지 경로

![2](/assets/2.png)

이미지는 url 방식으로 관리하며 태그를 붙일 수 있습니다. ubuntu 14.04 이미지는 `docker.io/library/ubuntu:14.04` 또는 `docker.io/library/ubuntu:trusty` 이고 `docker.io/library`는 생략가능하여 `ubuntu:14.04` 로 사용할 수 있습니다. 이러한 방식은 이해하기 쉽고 편리하게 사용할 수 있으며 태그 기능을 잘 이용하면 테스트나 롤백도 쉽게 할 수 있습니다.

### Dockerfile

~~~shell
# vertx/vertx3 debian version
FROM subicura/vertx3:3.3.1
MAINTAINER chungsub.kim@purpleworks.co.kr

ADD build/distributions/app-3.3.1.tar /
ADD config.template.json /app-3.3.1/bin/config.json
ADD docker/script/start.sh /usr/local/bin/
RUN ln -s /usr/local/bin/start.sh /start.sh

EXPOSE 8080
EXPOSE 7000

CMD ["start.sh"]
~~~

도커는 이미지를 만들기 위해 `Dockerfile`이라는 파일에 자체 `DSLDomain-specific language` 언어를 이용하여 **이미지 생성 과정을** 적습니다.


### Docker Hub

도커 이미지의 용량은 보통 수백메가로 엄청 큽니다. 이렇게 큰 용량의 이미지를 서버에 저장하고 관리하는 것은 쉽지 않은데 도커는 Docker hub를 통해 공개 이미지를 무료로 관리해 줍니다.

### Command와 API

도커 클라이언트의 커맨드 명령어는 정말 자아아알 만들어져 있습니다. 대부분의 명령어는 직관적이고 사용하기 쉬우며 컨테이너의 복잡한 시스템 구성을 이해하지 못하더라도 편하게 사용할 수 있습니다. 또한 http기반의 Rest API도 지원하여 확장성이 굉장히 좋고 훌륭한 3rd party 툴이 나오기 좋은 환경입니다.

### 유용한 새로운 기능들

도커는 발전속도가 아주 빠른 오픈소스입니다. 사용하면서 부족하다고 느꼈던 부분은 빠르게 개선되고 새로운 버전이 나오면 유용한 기능이 대폭 추가됩니다.





<br/><br/>

## Docker 사용하기

### Docker 설치하기 (Linux)

리눅스에 도커를 설치하는 방법은 자동 설치 스크립트를 이용하는 것이 가장 쉽습니다.

~~~bash
curl -fsSL https://get.docker.com/ | sudo sh
~~~

> 여기서 curl이란? Client URL의 줄임말로, 클라이언트에서 커맨드 라인이나 소스코드로 손 쉽게 웹 브라우저 처럼 활동할 수 있도록 해주는 기술입니다.


### Docker에 권한 추가하기

~~~bash
sudo usermod -aG docker $USER # 현재 접속중인 사용자에게 권한주기
sudo usermod -aG docker your-user # your-user 사용자에게 권한주기
~~~


### 설치 확인하기

Client와 Server 정보가 정상적으로 출력되었다면 설치가 완료된 것 입니다.

~~~bash
docker version
~~~

버전정보가 클라이언트와 서버로 나뉘어져 있습니다. 도커는 하나의 실행파일이지만 실제로 클라이언트와 서버역할을 각각 할 수 있습니다. 도커 커맨드를 입력하면 도커 클라이언트가 도커 서버로 명령을 전송하고 결과를 받아 터미널에 출력해 줍니다.

![3](/assets/3.png)

기본값이 도커 서버의 소켓을 바라보고 있기 때문에 사용자는 의식하지 않고 마치 바로 명령을 내리는 것 같은 느낌을 받습니다. 이러한 설계가 mac이나 windows의 터미널에서 명령어를 입력했을때 가상 서버에 설치된 도커가 동작하는 이유입니다.


### 컨테이너 실행하기

#### 예제

~~~shell
docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]
~~~

| 옵션  |  설명 |
|---|---|
| `-d`  | detached mode 흔히 말하는 백그라운드 모드  |
| `-p`  |  호스트와 컨테이너의 포트를 연결 (포워딩) |
| `-v`  |호스트와 컨테이너의 디렉토리를 연결 (마운트)  |
| `-e` | 컨테이너 내에서 사용할 환경변수 설정  |
| `–name`  | 컨테이너 이름 설정  |
| `–rm` |  프로세스 종료시 컨테이너 자동 제거 |
| `-it` | -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션  |
| `–link`  |  컨테이너 연결 [컨테이너명:별칭] |

#### Ubuntu 16.04 컨테이너 실행해보기

~~~shell
docker run ubuntu:16.04
~~~

> 만약 실패했다면, 도커가 실행되지 않았을 수 있습니다. 아래 명령어를 통해 도커를 실행해주세요.

~~~bash
# Linux
sudo systemctl start docker
sudo systemctl enable docker

# WSL2
sudo /etc/init.d/docker start
~~~

`run` 명령어를 사용하면 사용할 이미지가 저장되어 있는지 확인하고 없다면 다운로드( `pull` )를 한 후 컨테이너를 생성( `create` )하고 시작( `start` ) 합니다.

위 예제는 `ubuntu:16.04` 이미지를 다운받은 적이 없기 때문에 이미지를 다운로드 한 후 컨테이너가 실행되었습니다.

컨테이너는 정상적으로 실행됐지만 뭘 하라고 명령어를 전달하지 않았기 때문에 컨테이너는 생성되자마자 종료됩니다.

컨테이너는 프로세스이기 때문에 실행중인 프로세스가 없으면 컨테이너는 종료됩니다.

이번에는 `/bin/bash` 명령어를 입력해서 `ubuntu:16.04` 컨테이너를 실행해 보겠습니다.

~~~bash
docker run --rm -it ubuntu:16.04 /bin/bash
~~~

~~~bash
# in container
$ cat /etc/issue
Ubuntu 16.04.1 LTS \n \l

$ ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr

$ exit
~~~

#### redis 컨테이너 실행해보기

> [redis란?](https://medium.com/@jyejye9201/%EB%A0%88%EB%94%94%EC%8A%A4-redis-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-2b7af75fa818)

`redis`는 메모리기반의 다양한 기능을 가진 스토리지입니다. 6379 포트로 통신하며 `telnet` 명령어로 테스트해 볼 수 있습니다. `redis` 컨테이너는 `detached mode`(백그라운드 모드)로 실행하기 위해 `-d` 옵션을 추가하고 `-p` 옵션을 추가하여 컨테이너의 포트를 호스트의 포트로 연결해보겠습니다.

~~~bash
docker run -d -p 1234:6379 redis
~~~

~~~bash
# redis test
$ telnet localhost 1234
set mykey hello
# +OK
get mykey
# $5
# hello
quit
~~~

`-d` 옵션을 주었기 때문에 컨테이너를 실행하자마자 컨테이너의 ID(5dff91d2…)를 보여주고 바로 쉘로 돌아왔습니다. **컨테이너는 종료된 것이 아니라 백그라운드 모드로 동작하고 있고 컨테이너 ID를 이용하여 컨테이너를 제어할 수 있습니다.** **-p** 옵션을 이용하여 호스트의 1234포트를 컨테이너의 6379포트로 연결하였고 localhost의 1234포트로 접속하면 하면 redis를 사용할 수 있습니다.

테스트 결과 redis에 접속하여 새로운 키를 저장하고 불러오는데 성공했습니다. 실행이 간단한건 물론이고 호스트의 포트만 다르게 하면 하나의 서버에 여러개의 redis 서버를 띄우는 것도 매우 간단합니다.



#### MySQL 컨테이너 실행해보기

3번째 실행할 컨테이너는 `MySQL 서버`입니다. 가장 흔하게 사용하는 데이터베이스인데 이번에는 `-e` 옵션을 이용하여 **환경변수를 설정하고** `--name` 옵션을 이용하여 **컨테이너에 읽기 어려운 ID 대신 쉬운 이름을 부여할 예정입니다.**

[MySQL Docker hub](https://hub.docker.com/_/mysql/) 페이지에 접속하면 간단한 사용법과 환경변수에 대한 설명이 있습니다. 여러가지 설정값이 있는데 패스워드 없이 root계정을 만들기 위해 `MYSQL_ALLOW_EMPTY_PASSWORD` 환경변수를 설정합니다. 그리고 컨테이너의 이름은 `mysql`로 할당하고 백그라운드 모드로 띄우기 위해 `-d` 옵션을 줍니다. 포트는 `3306포트`를 호스트에서 그대로 사용하겠습니다.

~~~bash
docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  mysql:5.7
~~~

~~~bash
# MySQL test
$ mysql -h127.0.0.1 -uroot

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql> quit
~~~







<br/><br/>

## Docker 기본 명령어

### 컨테이너 목록 확인 (ps)

~~~shell
docker ps [OPTIONS] # -a, -all
~~~


### 컨테이너 중지 (stop)

~~~shell
docker stop [OPTIONS] CONTAINER [CONTAINER...]
~~~

#### 사용 예제

~~~shell
docker ps # get container ID
docker stop ${TENSORFLOW_CONTAINER_ID}
docker ps -a # show all containers
~~~

### 컨테이너 제거 (rm)

~~~shell
docker rm [OPTIONS] CONTAINER [CONTAINER...]
~~~

#### 사용예제

~~~shell
docker ps -a # get container ID
docker rm ${UBUNTU_CONTAINER_ID} ${TENSORFLOW_CONTAINER_ID}
docker ps -a # check exist
~~~

> 중지된 컨테이너를 일일이 삭제 하는 건 귀찮은 일입니다. `docker rm -v $(docker ps -a -q -f status=exited)` 명령어를 입력하면 중지된 컨테이너 ID를 가져와서 한번에 삭제합니다.

### 이미지 목록 확인 (images)

~~~shell
docker images [OPTIONS] [REPOSITORY[:TAG]]
~~~

### 이미지 다운로드 (pull)

~~~shell
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
~~~

### 이미지 삭제하기 (rmi)

~~~shell
docker rmi [OPTIONS] IMAGE [IMAGE...]
~~~

#### 사용예제

~~~shell
docker images # get image ID
docker rmi ${TENSORFLOW_IMAGE_ID}
~~~

### 컨테이너 로그 보기 (logs)

> 컨테이너가 정상적으로 동작하는지 확인하는 좋은 방법은 로그를 확인하는 것 입니다. 로그를 확인하는 방법은 다음과 같습니다.

~~~shell
docker logs [OPTIONS] CONTAINER #-f, --tail
~~~

#### 사용예제

~~~shell
docker ps
docker logs ${MYSQL_CONTAINER_ID}
~~~

#### 좀 더 자세히

프로그램마다 로그 파일은 제각각 생길텐데 어떻게 저 로그가 나올까 라는 의문이 생깁니다. 도커는 로그파일을 자동으로 알아채는게 아니라 표준 스트림Standard streams 중 stdout, stderr를 수집합니다. 따라서 컨테이너에서 실행되는 프로그램의 로그 설정을 파일이 아닌 표준출력으로 바꾸어야 합니다. 단지 출력 방식만 바꾸는 것으로 모든 컨테이너는 로그에 대해 같은 방식으로 관리할 수 있게 됩니다.

또하나 중요한 점은 컨테이너의 로그파일은 json 방식으로 어딘가에 저장이 됩니다. 로그가 많으면 은근히 파일이 차지하는 용량이 커지므로 주의해야합니다. 도커는 다양한 플러그인을 지원하여 json이 아닌 특정 로그 서비스에 스트림을 전달할 수 있습니다. 어느 정도 앱의 규모가 커지면 기본적인 방식 대신 로그 서비스를 이용하는 걸 고려해야 합니다.

요약해서 docker의 로그는 표준 스트림을 가져와서 연결한 후 json으로 만들어 보관하는고 있다. 그러므로 모든 컨테이너에 대한 로그가 생기는 것이 가능한 것, 하지만 앱이 커질 수록 해당 로그의 용량이 커질 수 있으니, 해당 연결된 표준 스트림이 json이 되기 전에 다른 곳으로 연결하는 행동이 필요할 수도?


### 컨테이너 명령어 실행하기 (exec)

컨테이너를 관리하다 보면 실행중인 컨테이너에 들어가보거나 컨테이너의 파일을 실행하고 싶을 때가 있습니다. 컨테이너에 SSH를 설치하면 되지 않을까? 라고 생각할 수 있지만 SSH는 권장하지 않습니다. 하지 말라고 하면 꼭 하는 분들이 있던데 제발.. 예전에는 nsenter라는 프로그램을 이용하였는데 docker에 exec라는 명령어로 흡수되었습니다.

~~~shell
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
~~~

`run` 명령어와 유사해 보입니다. 차이는 `run`은 새로 컨테이너를 만들어서 실행하고 `exec`는 실행중인 컨테이너에 명령어를 내리는 정도입니다.

~~~shell
docker exec -it mysql mysql -uroot

# MySQL test
$ mysql -uroot

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| wp                 |
+--------------------+
5 rows in set (0.00 sec)

mysql> quit
~~~




<br/><br/>

## 컨테이너 업데이트

![4](/assets/4.png)

도커에서 컨테이너를 업데이트 하려면 새 버전의 이미지를 다운(`pull`)받고 기존 컨테이너를 삭제(`stop`, `rm`) 한 후 새 이미지를 기반으로 새 컨테이너를 실행(`run`)하면 됩니다. 배포와 관련된 자세한 사항은 다음글에서 이야기하고 여기선 그냥 그렇구나 하고 이해합시다.

컨테이너를 삭제한다는 건 컨테이너에서 생성된 파일이 사라진다는 뜻입니다. 데이터베이스라면 그동안 쌓였던 데이터가 모두 사라진다는 것이고 웹 어플리케이션이라면 그동안 사용자가 업로드한 이미지가 모두 사라진다는 것입니다.

이런 상황을 방지하기 위해 컨테이너 삭제시 **유지해야하는 데이터는 반드시 컨테이너 내부가 아닌 외부 스토리지에 저장해야 합니다.** 가장 좋은 방법은 **AWS S3같은 클라우드 서비스를 이용하는 것이고** 그렇지 않으면 **데이터 볼륨 Data volumes을 컨테이너에 추가해서 사용해야 합니다.** 데이터 볼륨을 사용하면 해당 디렉토리는 컨테이너와 별도로 저장되고 컨테이너를 삭제해도 데이터가 지워지지 않습니다.

데이터 볼륨을 사용하는 방법은 몇가지가 있는데 여기서는 호스트의 디렉토리를 마운트해서 사용하는 방법에 대해 알아봅니다. `run` 명령어에서 소개한 옵션중에 `-v` 옵션을 드디어 사용해 보겠습니다. `MySQL`이라면 `/var/lib/mysql` 디렉토리에 모든 데이터베이스 정보가 담기므로 호스트의 특정 디렉토리를 연결해주면 됩니다.

~~~shell
# before
docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  mysql:5.7

# after
docker run -d -p 3306:3306 \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  --name mysql \
  -v /my/own/datadir:/var/lib/mysql \
  mysql:5.7
~~~

위 샘플은 호스트의 `/my/own/datadir` 디렉토리를 컨테이너의 `/var/lib/mysql` 디렉토리로 마운트 하였습니다. 이제 데이터베이스 파일은 호스트의 `/my/own/datadir` 디렉토리에 저장되고 컨테이너를 삭제해도 데이터는 사라지지 않습니다. 최신버전의 MySQL 이미지를 다운받고 다시 컨테이너를 실행할 때 동일한 디렉토리를 마운트 한다면 그대로 데이터를 사용할 수 있습니다. 만세!




<br/><br/>

## Docker Compose

지금까지 도커를 커맨드라인에서 명령어로 작업했습니다. 지금은 간단한 작업만 했기 때문에 명령이 길지 않지만 컨테이너 조합이 많아지고 여러가지 설정이 추가되면 명령어가 금방 복잡해집니다.

도커는 복잡한 설정을 쉽게 관리하기 위해 `YAML` 방식의 설정파일을 이용한 `Docker Compose`라는 툴을 제공합니다. 깊게 파고들면 은근 기능이 많고 복잡한데 이번에는 아주 가볍게 다루고 지나가도록 하겠습니다.

### 설치하기

~~~bash
curl -L "https://github.com/docker/compose/releases/download/1.9.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
# test
docker-compose version
~~~

### wordpress 예제

~~~bash
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
    wp_data
~~~

위 처럼 만들고 아래 처럼 실행하기만 하면 끝!

~~~shell
docker-compose up
~~~



<br/><br/>

## 도커 이미지 만들기

> 도커는 이미지를 만들기 위해 컨테이너의 상태를 그대로 이미지로 저장하는 단순하고 무식(?)한 방법을 사용합니다.

![5](/assets/5.png)

[dockerfile 예제](https://github.com/search?utf8=%E2%9C%93&q=dockerfile%EF%BF%BC)


### Web Application Sample

> 이제 Ruby로 만들어진 간단한 웹 애플리케이션을 도커라이징Dockerizing(=도커 이미지를 만듦)해보겠습니다.

~~~shell
mkdir web-application-sample
cd web-application-sample
# Gemfile, app.rb 만들기
~~~

Gemfile

~~~shell
source 'https://rubygems.org'
gem 'sinatra'
~~~

app.rb

~~~shell
require 'sinatra'
require 'socket'

get '/' do
  Socket.gethostname
end
~~~

~~~shell
apt install ruby-bundler
bundle install            # install package
bundle exec ruby app.rb   # Run sinatra
~~~

### docker로 ruby 실행하기

~~~shell
docker run --rm \
-p 4567:4567 \
-v $PWD:/usr/src/app \
-w /usr/src/app \
ruby \
bash -c "bundle install && bundle exec ruby app.rb -o 0.0.0.0"
~~~

호스트의 디렉토리를 루비가 설치된 컨테이너의 디렉토리에 마운트한다음 그대로 명령어를 실행하면 로컬에 개발 환경을 구축하지 않고 도커 컨테이너를 개발환경으로 사용할 수 있습니다. 어으썸!

> 도커를 개발환경으로 사용하면 개발=테스트=운영이 동일한 환경에서 실행되는 놀라운 상황이 펼쳐집니다. 이 부분은 재미있는 내용이 많지만, 주제에서 벗어나므로 이 정도만 언급하고 다음 기회에 더 자세히 알아봅니다.

서버가 정상적으로 실행됐으면 웹 브라우저에서 테스트해봅니다. `http://localhost:4567`


### Ruby Application Dockerfile

![6](/assets/6.png)

도커는 이미지를 만들기 위해 `Dockerfile`이라는 이미지 빌드용 `DSLDomain Specific Language` 파일을 사용합니다. 단순 텍스트 파일로 일반적으로 소스와 함께 관리합니다.

우리는 초보니까 Ruby 웹 애플리케이션을 ubuntu에 배포하는 과정을 먼저 살펴보겠습니다.

| 순서  | 작업  |
|---|---|
| 1  | ubuntu 설치  |
| 2  | ruby 설치  |
| 3  | 소스 복사  |
| 4  | Gem 패키지 설치  |
| 5  | Sinatra 서버 실행  |

이 과정을 그대로 쉘 스크립트로 옮겨봅니다.

~~~shell
# 1. ubuntu 설치 (패키지 업데이트)
apt-get update

# 2. ruby 설치
apt-get install ruby
gem install bundler

# 3. 소스 복사
mkdir -p /usr/src/app
scp Gemfile app.rb root@ubuntu:/usr/src/app  # From host

# 4. Gem 패키지 설치
bundle install

# 5. Sinatra 서버 실행
bundle exec ruby app.rb
~~~

이 과정을 Dockerfile로 옮겨봅니다.

~~~shell
# 1. ubuntu 설치 (패키지 업데이트 + 만든사람 표시)
FROM       ubuntu:16.04
MAINTAINER subicura@subicura.com
RUN        apt-get -y update

# 2. ruby 설치
RUN apt-get -y install ruby
RUN gem install bundler

# 3. 소스 복사
COPY . /usr/src/app

# 4. Gem 패키지 설치 (실행 디렉토리 설정)
WORKDIR /usr/src/app
RUN     bundle install

# 5. Sinatra 서버 실행 (Listen 포트 정의)
EXPOSE 4567
CMD    bundle exec ruby app.rb -o 0.0.0.0
~~~

차이점은 도커 빌드 중엔 키보드를 입력할 수 없기 때문에 (y/n)을 물어보는 걸 방지하기 위해 `-y` 옵션을 추가한 것 정도입니다.

빌드 파일을 작성했으니 이제 이미지를 만들어 봅니다.

~~~shell
docker build -t app .
~~~

빌드 명령어를 실행하면 Dockerfile에 정의한 내용이 한 줄 한 줄 실행되는 걸 볼 수 있습니다. 실제로 명령어를 실행하기 때문에 빌드 시간이 꽤 걸리는 걸 알 수 있습니다. 최종적으로 `Successfully built xxxxxxxx` 메시지가 보이면 정상적으로 이미지를 생성한 것입니다.

그럼 이미지가 잘 생성되었는지 확인해보겠습니다.

~~~shell
docker images
~~~

와..와우! 드디어 첫 번째 도커 이미지를 생성했습니다! 이미지를 생성했으니 잘 동작하는지 컨테이너를 실행해보겠습니다.

~~~shell
docker run -d -p 8080:4567 app
docker run -d -p 8081:4567 app
docker run -d -p 8082:4567 app
~~~

`localhost:8080`, `localhost:8081`, `localhost:8082` 접속하면, 각각 실행된 3개의 웹서버에서 실행된 웹 어플리케이션에 접속 가능!


<br/><br/>

## Dockerfile 기본 명령어

이미지를 만드는 데 사용한 Dockerfile의 기본적인 명령어를 살펴보겠습니다.

### FROM

~~~shell
FROM <image>:<tag>
FROM ubuntu:16.04
~~~

베이스 이미지를 지정합니다. 반드시 지정해야 하며 어떤 이미지도 베이스 이미지가 될 수 있습니다. tag는 될 수 있으면 latest(기본값)보다 구체적인 버전(16.04등)을 지정하는 것이 좋습니다. 이미 만들어진 다양한 베이스 이미지는 `Docker hub`에서 확인할 수 있습니다.

### MAINTAINER

~~~shell
MAINTAINER <name>
MAINTAINER subicura@subicura.com
~~~

Dockerfile을 관리하는 사람의 이름 또는 이메일 정보를 적습니다. 빌드에 딱히 영향을 주지는 않습니다.

### COPY

~~~shell
COPY <src>... <dest>
COPY . /usr/src/app
~~~

파일이나 디렉토리를 이미지로 복사합니다. 일반적으로 소스를 복사하는 데 사용합니다. target디렉토리가 없다면 자동으로 생성합니다.

### ADD

~~~shell
ADD <src>... <dest>
ADD . /usr/src/app
~~~

`COPY` 명령어와 매우 유사하나 몇가지 추가 기능이 있습니다. `src`에 파일 대신 `URL`을 입력할 수 있고 `src`에 압축 파일을 입력하는 경우 자동으로 압축을 해제하면서 복사됩니다.

### RUN

~~~shell
RUN <command>
RUN ["executable", "param1", "param2"]
RUN bundle install
~~~

가장 많이 사용하는 구문입니다. 명령어를 그대로 실행합니다. 내부적으로 /bin/sh -c 뒤에 명령어를 실행하는 방식입니다.

### CMD

~~~shell
CMD ["executable","param1","param2"]
CMD command param1 param2
CMD bundle exec ruby app.rb
~~~

도커 컨테이너가 실행되었을 때 실행되는 명령어를 정의합니다. 빌드할 때는 실행되지 않으며 여러 개의 `CMD`가 존재할 경우 가장 마지막 `CMD`만 실행됩니다. 한꺼번에 여러 개의 프로그램을 실행하고 싶은 경우에는 `run.sh` 파일을 작성하여 데몬으로 실행하거나 `supervisord`나 `forego`와 같은 여러 개의 프로그램을 실행하는 프로그램을 사용합니다.

### WORKDIR

~~~shell
WORKDIR /path/to/workdir
~~~

`RUN`, `CMD`, `ADD`, `COPY`등이 이루어질 기본 디렉토리를 설정합니다. 각 명령어의 현재 디렉토리는 한 줄 한 줄마다 초기화되기 때문에 RUN cd /path를 하더라도 다음 명령어에선 다시 위치가 초기화 됩니다. 같은 디렉토리에서 계속 작업하기 위해서 `WORKDIR`을 사용합니다.

### EXPOSE

~~~shell
EXPOSE <port> [<port>...]
EXPOSE 4567
~~~

도커 컨테이너가 실행되었을 때 요청을 기다리고 있는(Listen) 포트를 지정합니다. 여러개의 포트를 지정할 수 있습니다.

### VOLUME

~~~shell
VOLUME ["/data"]
~~~

컨테이너 외부에 파일시스템을 마운트 할 때 사용합니다. 반드시 지정하지 않아도 마운트 할 수 있지만, 기본적으로 지정하는 것이 좋습니다.

### ENV

~~~shell
ENV <key> <value>
ENV <key>=<value> ...
ENV DB_URL mysql
~~~

컨테이너에서 사용할 환경변수를 지정합니다. 컨테이너를 실행할 때 -e옵션을 사용하면 기존 값을 오버라이딩 하게 됩니다.

여기까지 Dockerfile에서 가장 많이 사용하는 명령어에 대해 알아보았습니다. 모든 명령어가 궁금하신 분은 [공식문서](https://docs.docker.com/engine/reference/builder/)를 참고하세요.





<br/><br/>

## Build 분석

도커 빌드는 `임시 컨테이너 생성` > `명령어 수행` > `이미지로 저장` > `임시 컨테이너 삭제` > `새로 만든 이미지 기반 임시 컨테이너 생성` > `명령어 수행` > `이미지로 저장` > `임시 컨테이너 삭제` > … 의 과정을 계속해서 반복한다고 볼 수 있습니다. 명령어를 실행할 때마다 이미지 레이어를 저장하고 다시 빌드할 때 Dockerfile이 변경되지 않았다면 기존에 저장된 이미지를 그대로 캐시처럼 사용합니다.

이러한 레이어 개념을 잘 이해하고 있어야 최적화된 이미지를 생성할 수 있습니다.

### 도커 이미지 리팩토링

사실 앞에서 만든 이미지는 몇 가지 최적화 문제가 있습니다. 다시 한땀 한땀 살펴보겠습니다.

### Base Image

위에서 만든 `Ruby` 애플리케이션 이미지는 `ubuntu`를 베이스로 만들었지만 사실 훠어어얼씬 간단한 ruby 베이스 이미지가 존재합니다. 기존에 ruby를 설치했던 명령어는 ruby 이미지를 사용하는 것으로 간단하게 생략할 수 있습니다.

~~~shell
# before
FROM ubuntu:16.04
MAINTAINER subicura@subicura.com
RUN apt-get -y update
RUN apt-get -y install ruby
RUN gem install bundler

# after
FROM ruby:2.3
MAINTAINER subicura@subicura.com
~~~

ruby외에도 nodejs, python, java, go등 다양한 베이스 이미지가 이미 존재합니다. 세부적인 설정이 필요하지 않다면 그대로 사용하는게 간편합니다.

기존 소스에서 소스파일이 수정되면 캐시가 깨지는 부분은 다음과 같습니다.

~~~shell
COPY . /usr/src/app    # <- 소스파일이 변경되면 캐시가 깨짐
WORKDIR /usr/src/app
RUN bundle install     # 패키지를 추가하지 않았는데 또 인스톨하게 됨 ㅠㅠ
~~~

복사하는 파일이 이전과 다르면 캐시를 사용하지 않고 그 이후 명령어는 다시 실행됩니다. ruby gem 패키지를 관리하는 파일은 Gemfile이고 Gemfile은 잘 수정되지 않으므로 다음과 같이 순서를 바꿀 수 있습니다.

~~~shell
COPY Gemfile* /usr/src/app/ # Gemfile을 먼저 복사함
WORKDIR /usr/src/app
RUN bundle install          # 패키지 인스톨
COPY . /usr/src/app         # <- 소스가 바꼈을 때 캐시가 깨지는 시점 ^0^
~~~

gem 설치 하는 부분을 소스 복사 이전으로 옮겼습니다. 이제 소스가 수정되더라도 매번 gem을 설치하지 않아 더욱 빠르게 빌드할 수 있습니다. 요즘 언어들은 대부분 패키지 매니저를 사용하므로 비슷한 전략으로 작성하면 됩니다.






<br/><br/>

## 이미지 저장소

![7](/assets/7.png)

도커는 빌드한 이미지를 서버에 배포하기 위해 직접 파일을 복사하는 방법 대신 도커 레지스트리`Docker Registry`라는 이미지 저장소를 사용합니다. 도커 명령어를 이용하여 이미지를 레지스트리에 `push`하고 다른 서버에서 `pull`받아 사용하는 구조입니다.

git을 사용하는 느낌?

도커 레지스트리는 오픈소스로 무료로 설치할 수 있고 설치형이 싫다면 도커(Docker Inc.)에서 서비스 중인 Docker Hub를 사용할 수 있습니다.



### Docker Hub

도커 허브는 도커에서 제공하는 기본 이미지 저장소로 ubuntu, centos, debian등의 베이스 이미지와 ruby, golang, java, python 등의 공식 이미지가 저장되어 있습니다. 일반 사용자들이 만든 이미지도 50만 개가 넘게 저장되어 있고 다운로드 횟수는 80억 회를 넘습니다.

회원가입만 하면 대용량의 이미지를 무료로 저장할 수 있고 다운로드 트래픽 또한 무료입니다. 단, 기본적으로 모든 이미지는 공개되어 누구나 접근 가능하므로 비공개로 사용하려면 유료 서비스를 이용해야 합니다. (한 개는 무료)

회원가입을 하고 앞에서 만든 Ruby 웹 애플리케이션 이미지를 저장해보겠습니다.


### 회원가입

[도커 허브 사이트](https://hub.docker.com/)에 접속하면 쉽게 회원가입을 할 수 있습니다.


### 로그인

도커에서 도커 허브 계정을 사용하려면 로그인을 해야합니다.

~~~shell
docker login
~~~

### 이미지 태그

도커 이미지 이름은 다음과 같은 형태로 구성됩니다.

~~~shell
[Registry URL]/[사용자 ID]/[이미지명]:[tag]
~~~

Registry URL은 기본적으로 도커 허브를 바라보고 있고 사용자 ID를 지정하지 않으면 기본값(library)을 사용합니다. 따라서 `ubuntu` = `library/ubuntu` = `docker.io/library/ubuntu` 는 모두 동일한 표현입니다.

도커의 `tag` 명령어를 이용하여 기존에 만든 이미지에 추가로 이름을 지어줄 수 있습니다.

~~~shell
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
~~~

#### 앞에서 만든 app 이미지에 계정정보와 버전 정보를 추가해보겠습니다.

> 1

~~~shell
docker tag app subicura/sinatra-app:1
~~~

`subicura`라는 ID를 사용하고 이미지 이름을 `sinatra-app`으로 변경했습니다. 첫 번째 버전이므로 태그는 1을 사용합니다. 이제 push명령을 이용해 도커 허브에 이미지를 전송해 봅니다.

> 2

~~~shell
docker push subicura/sinatra-app:1
~~~

> 3

성공적으로 이미지를 도커 허브에 푸시하였습니다. 도커 허브에 저장된 50만 개의 이미지에 새로운 이미지가 하나 추가되었습니다!

이제 어디서든 subicura/sinatra-app:1이미지를 사용할 수 있습니다.



### Private Docker Registry

도커 이미지를 비공개로 저장하려면 Docker Cloud를 유료($7 for 5 repos/month)로 사용하거나 레지스트리 서버를 자체적으로 구축해야합니다.

도커 레지스트리는 도커를 이용하여 쉽게 만들 수 있습니다. 도커 이미지를 저장할 서버를 도커 스스로 만들어서 도커 이미지를 관리하다니 뭔가 므-_-흣합니다.

~~~shell
docker run -d \
-v $PWD/registry:/var/lib/registry \
-p 5000:5000 \
distribution/registry:2.6.0
~~~

저장된 이미지는 파일로 관리되기 때문에 호스트의 디렉토리를 마운트하였습니다. (S3 저장소를 사용할 수도 있습니다) 이제 레지스트리 서버의 아이피와 포트정보를 이미지명에 추가하면 바로 사용할 수 있습니다.

~~~shell
docker tag app localhost:5000/subicura/sinatra-app:1
docker push localhost:5000/subicura/sinatra-app:1
~~~

앞에서 만든 이름에 localhost:5000/를 추가했습니다. 레지스트리 서버에 파일이 잘 저장되었나 마운트한 디렉토리를 한번 살펴봅니다.

~~~shell
apt install tree
tree registry
~~~

이미지가 레이어별로 이쁘게 저장된 걸 확인할 수 있습니다. 이렇게 개인 저장소를 만드는 법은 매우 간단합니다. 이제 내부적으로 이미지를 관리하고 여러 서버에 배포할 수 있습니다.


### 보안

도커 레지스트리는 일반적인 HTTP 프로토콜을 사용하여 이미지를 전송합니다. 따라서 SSL(HTTPS)을 사용하지 않으면 이미지 내용이 유출될 수 있습니다. 이런 보안 이슈 때문에 도커는 기본적으로 로컬(localhost) 서버를 제외하곤 HTTP 사용을 금지하고 있으며 이런 보안위험을 무시하려면 도커 엔진을 실행할 때 허용 옵션을 넣어야 합니다.




<br/><br/>

## 배포하기

드디어 도커 안내서의 마지막 주제, 서버관리의 꽃! 배포deploy에 대해 알아보겠습니다.

### 컨테이너 배포 방식으로

컨테이너를 배포하는 방식은 기존에 애플리케이션을 배포하는 방식과 큰 차이가 있습니다.

기존에 애플리케이션을 배포하는 방식은 사용하는 언어, 프레임워크, 웹(or WAS)서버, 리눅스 배포판, 개발자의 취향에 따라 각각 다른 방식을 사용했습니다.

새로운 서버를 셋팅하고 한 번에 배포를 성공한다는 건 굉장히 힘든 일이었고 의존성 라이브러리가 제대로 설치되었는지 검증하기도 매우 어려웠습니다.

ftp, rsync, ant, gradle, capistrano, fabric, chef, puppet, ansible등 다양한 배포툴이 저마다의 장점을 가지고 등장하였고 배포하는 방식을 하나로 정의한다는 건 거의 불가능했습니다.

하지만, 컨테이너를 사용하면 어떤 언어, 어떤 프레임워크를 쓰든 상관없이 배포 방식이 동일해지고 과정 또한 굉장히 단순해집니다. 그냥 이미지를 다운받고 컨테이너를 실행하면 끝입니다.

음…

네, 그게 답니다.

서버에 접속해서 컨테이너를 실행할 줄 안다면 이미 배포하는 법을 알고 있는 겁니다. 참 쉽죠?



<br/><br/>

## Reference

* [초보자를 위한 도커 안내서](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
* [도커를 이용한 웹서비스 무중단 배포하기](https://subicura.com/2016/06/07/zero-downtime-docker-deployment.html)

