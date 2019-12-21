---
title: Deploy] EndMovie Deploy - Linux
date: 2019-12-12 22:22:52
tags: ['Linux', 'Deploy', 'Docker Compose']
categories: ['Docker']
---

> 참고] 배포과정은 버전에 따라 환경에 따라 다를 수 있습니다. 해당 방법은 어디까지나 삽질을 해보고 이런 방도로도 실행이 가능하다라는 정리문서이지 답이 아님을 밝힙니다.





## 목적

프로젝트를 먼저 Linux(Mint)환경에서 실행해보고 Docker-Compose를 통해 AWS EC2에 Deploy해보자.




## 환경

- Linux Mint(ubuntu 16.04)

아래의 버전들은 프로젝트 버전에 맞추는게 좋다. 필자는 배포에만 신경썼기에 버전이 프로젝트와는 다르게 왔다갔다하는 부분이 있다. 설치할 때 버전을 설정하면 프로젝트 버전과 같게 설치할 수 있다.

- Python 3.6.8
- Node 12.x.x



```bash
sudo apt-get update
```



## Deploy DB

DB는 Docker Image를 통해 설치해보자.

```bash
curl -fsSL https://get.docker.com/ | sudo sh
```

Docker를 먼저 다운로드 받는다. 이렇게만 할 경우 docker를 사용하기 전에 `sudo`를 붙여줘야 하는 귀차니즘이 발생하긴 한다.

### sudo 없이 사용하기

```bash
sudo usermod -aG docker $USER # 현재 접속중인 사용자에게 권한주기
```



먼저, sudo를 사용한 버전으로 소스를 작성하도록 하겠다.

### Install PostgreSQL

기본적인 기능만 사용한다면 `alpine`을 사용하면 좋다. 최소한의 기능으로 경량화된 버전이다. 단, 많은 기능을 사용한다면 경량화 버전에서 설치를 계속해줘야하기에 불편한 점도 있다.

```bash
docker pull postgres
docker run -d -p 5432:5432 --name pgsql -e POSTGRES_PASSWORD=비밀번호 postgres
```

Docker 볼륨을 생성하여 데이터를 유지해야할 경우는 volume 옵션이 필요하다.

```bash
docker volume create pgdata
docker run -d -p 5432:5432 --name pgsql -it --rm -v pgdata:/var/lib/postgresql/data postgres
```



#### PostgreSQL Create Database

```bash
docker exec -it pgsql bash

#PostgreSQL
psql -U postgres
CREATE DATABASE 데이터베이스이름;
```





## Django Settings

먼저, Docker를 사용하지 않고 세팅해보자

파이썬의 버전이 너무 거슬린다. 이전의 방법도 있지만 전체적 설정에서 바꿔보자.

```bash
sudo update-alternatives --config python
```

만약 위의 명령어를 작성하였는데 `update-alternatives: error: no alternatives for python`가 Error가 발생하였다면 **alternative**가 설정된 것이 없는 것이니 설정을 해주자.

```bash
which python #파이썬의 경로
ls /usr/bin/ | grep python #파이썬 리스트(경로는 기본이라면...)
```

### alternative 설정

```bash
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 2
```

이후에 `sudo update-alternatives --config python`를 다시 입력하면 버전 선택을 할 수 있다.



이렇게 설정이 다 끝나고 나서 프로젝트에 있는 라이브러리들을 다운받으려고 했더니 pip가 없었다.

python3를 기본적으로 사용할 것이기에 pip3만 다운로드 받는다. 이렇게 설치하여도 pip 명령어로 실행할 수 있었다. 아래의 코드가 문제가 생길시에는 sudo를 붙여서 진행하면 된다.

```bash
apt-get install python3-pip
```



다 끝났다면 라이브러리를 설치하고 진행하면 된다.



## Vue Settings

yarn을 apt-get에서 바로 다운받거나 다른 방도가 있긴 하지만, 다른 파일을 다운 받아달라는 오류가 발생하여 npm에서 설치하는 방식으로 진행함

```bash
sudo apt-get install npm
npm install -g yarn

sudo apt-get install nodejs
```



nodejs를 다운받았는데 버전이 8이다. 프로젝트 설정된 버전보다 낮아 진행할 수 없는 문제가 발생했다. nodejs의 버전을 올려주자.



```bash
# 강제 캐시 삭제
sudo npm cache clean --force
# n모듈 설치
sudo npm install -g n
# n모듈 이용한 Node.js 설치
sudo n stable
# npm 설치
sudo npm install -g npm

# vue-cli설치
yarn add vue-cli

yarn install
yarn serve
```





## Docker Compose

그러면 이제 본격적으로 docker-compose를 이용해서 Linux환경에서 실행해보자.

docker로 하나하나 돌리는 방법도 있겠지만 옵션을 하나하나 주면서 확인해보고 하기에 너무 불편하다. 그러기에 docker-compose를 사용하기로 한다.



### Install docker-compose

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```



### settings docker-compose

```yml
version: '3.5'

services:
  db:
    image: postgres
    restart: always
    ports:
      - 5432:5432
    networks:
      - end-network
    volumes:
      - ./db:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=end_movie
      - POSTGRES_UESR=postgres
      - POSTGRES_PASSWORD=0525
      - POSTGRES_INITDB_ARGS=--encoding=UTF-8

  front:
    image: node
    container_name: front
    restart: always
    ports:
      - 8080:8080
    working_dir: /usr/src/app
    volumes:
      - ./front:/usr/src/app
    command: >
      bash -c "yarn install && yarn serve"
    environment:
      - LC_ALL=C.UTF-8
    tty: true
    networks:
      - end-network

  back:
    build:
      context: .
      dockerfile: ./back/Dockerfile
    restart: always
    ports:
      - 8000:8000
    working_dir: /usr/src/app
    volumes:
      - ./back:/usr/src/app
    command: >
      bash -c "python manage.py makemigrations && python manage.py migrate && python manage.py runserver 0.0.0.0:8000"
    environment:
      - LC_ALL=C.UTF-8
    tty: true
    networks:
      - end-network


networks:
  end-network:
    name: end-network
```

docker에서 django image는 지원하지 않는다는 내용을 확인하였다. 그래서 Python images를 받아서 진행할까 하다가 Dockerfile로 먼저 진행하였다.

```dockerfile
FROM python:3
ENV PYTHONUNBUFFERED 1

WORKDIR /usr/src/app
ADD ./back/requirements.txt /usr/src/app/
RUN pip install -r requirements.txt

EXPOSE 8000
```





## 느낀점

DevOps의 길은 알면 쉽고 모르면 너무나도 힘들다...

Deploy로만 2일이 걸릴 줄이야...

더붙여야할 기능이라면 더 있지만... CI/CD를 다뤄보면서 진행하자...

이제 본격적인 AWS Deploy로 넘어가도록 하겠다.



## 참고 자료

 https://subicura.com/2017/01/19/docker-guide-for-beginners-2.html 

 https://judo0179.tistory.com/48 

 https://seongkyun.github.io/others/2019/05/09/ubuntu_python/ 

 https://d2fault.github.io/2018/04/30/20180430-install-and-upgrade-nodejs-or-npm/ 

 https://docs.docker.com/compose/install/ 

 https://www.44bits.io/ko/post/almost-perfect-development-environment-with-docker-and-docker-compose 

 https://github.com/raccoonyy/django-sample 