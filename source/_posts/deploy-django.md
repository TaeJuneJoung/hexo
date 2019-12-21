---
title: AWS] Django AWS EC2 Deploy - 1
date: 2019-12-03 00:22:45
tags: ['AWS', 'Deploy', 'EC2']
categories: ['AWS']
---

## 목적

Django를 AWS EC2에 배포하여 접근하기



## 용어

**EC2**(Elastic Compute Cloud): 가상 인스턴스를 운영하는 서비스

**인스턴스**(Instance): AWS에서 가상 서버를 부르는 용어

가상서버: CPU와 메모리를 가진 클라우드 내 서버

**IAM**(Identity and Access Management): 사용자 엑세스 및 암호화 키 관리

**보안 그룹**(Security Group): 인스턴스에 대한 트래픽을 제어하는 가상 방화벽 역할 



## AWS 설정

### IAM 생성

{% asset_img slug 01.png %}

AWS에서 IAM을 먼저 생성하고 진행 해야한다.

{% asset_img slug 02.png %}

진행하며 사용자가 생성되면 `Access Key ID`와 `Secret access key`가 나온다.
**`Secret access key`는 절대 노출되어서는 안된다**.

{% asset_img slug 03.png %}

**키 페어**를 생성하면 `pem`파일이 만들어진다.

{% asset_img slug 04.png %}

{% asset_img slug 05.png %}

아래 보이는 규칙 추가를 이용해서 port를 열어줄 것이다.

{% asset_img slug 06.png %}



```bash
chmod 400 pem파일
```



## 인스턴스에 접속하기

생성한 가상 컴퓨터 인스턴스에 `ssh`를 사용하여 접속

```bash
ssh -i pem파일 유저명@EC2퍼플릭DNS주소
```

```bash
> Are you sure you want to continue connecting (yes/no)?
```

가 나오면 yes를 입력하여 접속하면 된다.



### 기본설정

#### update package

```bash
sudo apt-get update

# 패키지 의존성 검사 및 업그레이드
sudo apt-get dist-upgrade
```

다른 화면이 나올 경우에는 `Enter`를 누르면 된다.



#### Install python-pip

```bash
sudo apt-get install python-pip
```

#### Install zsh

```bash
sudo apt-get install zsh
```

#### Install oh-my-zsh

```bash
sudo curl -L http://install.ohmyz.sh | sh
```

#### Change Default shell

```bash
sudo chsh ubuntu -s /usr/bin/zsh
```

#### Install pyenv requirements

```bash
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils
```

#### Install pyenv

```bash
curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
```

#### pyenv 설정을 .zshrc 기록

```bash
vi ~/.zshrc
export PATH="/home/ubuntu/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```

위의 내용이 `.zshrc`에 기록해야하는 것인지 bash에 한줄 한줄 써서 진행해야 하는것이 판별 필요

진행할때는 기록하고 하였으나 에러가 발생하여 한줄 한줄 bash에 작성하여 진행하여 해결함



## Django 설정

```bash
sudo chown -R ubuntu:ubuntu /srv/
```

### git clone을 통한 프로젝트 가져오기

```bash
git clone 프로젝트git주소
```

예시 Git 프로젝트 주소
https://github.com/TaeJuneJoung/Python-Deploy/tree/deploy-test



### pyenv 설치 및 virtualenv

```bash
cd 프로젝트폴더
pyenv install 3.7.5 #파이썬 버전
pyenv virtualenv deploy_ec2
pyenv local deploy_ec2
```



#### Install python library

```bash
sudo apt-get install python-dev python-setuptools
```



#### pyenv 접속

```bash
pyenv shell 3.7.5

pip --version

pip install django

# requirements.txt가 있다면
pip install -r requirements.txt
```



### runserver 테스트

- 0:8000으로 지정필요
-  웹 브라우저에서 <퍼블릭 DNS:8000=""> 로 접속하기 위해서는 보안그룹(security group) 설정이 필요 

```bash
python manage.py runserver 0:8000
```





## 참고 사이트

 https://nachwon.github.io/django-deploy-1-aws/ 

 https://wayhome25.github.io/django/2018/03/03/django-deploy-03-ec2/ 

 https://jiyeonseo.github.io/2016/07/27/install-pyenv/ 