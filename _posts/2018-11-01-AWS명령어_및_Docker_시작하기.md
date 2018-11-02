---
title: "AWS명령어 및 Docker 시작하기"
categories: 
   - Dev
tags:
    - mac
    - AWS
    - Docker
author_profile: true

toc: true
toc_label: "My Talbe of Contents"
toc_icon: "cog"
toc_sticky: true

last_modified_at:
comments: true
---



# AWS 연동 및 Docker 사용하기 


## AWS scp, ssh 사용하기 

### ssh, scp 명령어 

#### ssh명령어 사용하기

	$ ssh -i <.pem file Root> ubuntu@<인스턴스 퍼블릭 DNS>

내 로컬 저장소에서 aws에 있는 인스턴스로 접속할 때 ssh명령어를 사용한다. 위 명령어 형식에 맞게 터미널에 입력하여 aws가상 서버에 접속한다. 


```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for '/Users/Arcanelux/.ssh/fastcampus.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "/Users/Arcanelux/.ssh/fastcampus.pem": bad permissions
Permission denied (publickey).
```
> aws가상 서버에 접속하기 위해 ssh 명령어를 실행했을 때 위 경고 문구가 나온다면 pem파일을 소유주만 읽을 수 있도록 권한 설정을 해주어야 한다. <br>
> `chmod 400 <pem file>`<br>
> pem 파일 위치에서 명령어를 실행한다.

- aws 가상 서버 locale 설정

aws 가상 서버에 접속 후 기본 설정을 해주어야 한다. 먼저 locale 설정을 해준다.

다음 명령어를 통해 서버에 접속후 아래의 문장들로 채워준다. 그리고 aws 가상 서버에 재접속 한다. 

`sudo vi /etc/default/locale`

	LC_CTYPE="en_US.UTF-8"
	LC_ALL="en_US.UTF-8"
	LANG="en_US.UTF-8"

- aws 가상 서버 기본 설정

```
# 패키지 정보를 업데이트
sudo apt-get update

# 설치되어 있는 패키지들을 의존성 검사하며 업그레이드
sudo apt-get dist-upgrade

# zsh 설치
sudo apt-get install zsh

# oh-my-zsh 설치
sudo curl -L http://install.ohmyz.sh | sh

# Default shell 변경
# shell 변경 후엔 exit로 연결을 종료한 뒤 재연결
sudo chsh ubuntu -s /usr/bin/zsh
```

이렇게 명령어를 입력하고 ssh 명령어를 통해 aws에 재접속하면 다음과 같은 터미널 환경이 보인다.

>  #ssh 명령어 <br>
> 	$ ssh -i <.pem file Root> ubuntu@<인스턴스 퍼블릭 DNS>

![사진](/assets/deploy/photo1.png)


#### scp 명령어 사용하기

scp명령어는 내 로컬 서버에 있는 파일,폴더들을 가상 서버에 옮길 때 사용한다. 예를 들면 내 로컬 서버에서 작업 중이던 Django 파일을 aws가상 서버로 옮길 때 사용한다. scp명령어는 다음 양식에 맞춰서 사용한다. 

	$ scp -i <.pem file Root> -r <전송할 파일의 경로> ubuntu@<인스턴스 퍼블릭 DNS>:<전송받을 aws가상 서버 경로>
	

#### ssh, scp 명령어를 alias를 사용하여 단축어 지정하기

ssh, scp 명령어를 사용하다 보면 가독성이 떨어지고 너무 길기 때문에 귀찮다. 따라서 `.zshrc`에서 alias를 통해 ssh, scp 명령어를 단축어로 지정한다.

먼저 .zshrc 파일을 vim을 통해 열어준다. 

`$ vim ~/.zshrc`

```
# ssh 단축어
alias <ssh단축어>="ssh -i <.pem file Root> ubuntu@<인스턴스 퍼블릭 DNS>"

# scp 단축어
alias <scp단축어>="ssh -i <.pem file Root> -r <전송할 파일의 경로> ubuntu@<인스턴스 퍼블릭 DNS>:<전송받을 aws가상 서버 경로>"
```


## aws S3 사용하기 (with boto3)

아마존 S3(Amazon S3, Simple Storage Service)는 아마존 웹 서비스에서 제공하는 온라인 스토리지 웹 서비스이다. 아마존 S3는 웹 서비스 인터페이스를 통해 스토리지를 제공한다. 이 S3 안에는 저장소 개념인 버킷(bucket)이 존재한다. 버킷은 여러개 만들 수 있다. S3를 사용하기 전에 IAM을 사용하여 권한을 설정해 준다.

이 IAM(AWS Identity and Access Management)은 AWS 리소스에 대한 액세스를 안전하게 제어할 수 있는 웹 서비스다. IAM을 사용하여 리소스를 사용하도록 인증(로그인) 및 권한 부여(권한 있음)된 대상을 제어한다. [IAM 사이트](https://console.aws.amazon.com/iam/)에서 설정한다. 

먼저 amazon사이트에 로그인 한 후 [IAM 사이트](https://console.aws.amazon.com/iam/)에 접속하면 IAM리소스 화면이 보인다. 여기서 사용자를 눌러준다. 그리고 `기존 정책 연결`에서 정책 필터에 `s3`를 검색하여 __`AmazonS3FullAccess`__를 선택한다. 이 __AmazonS3FullAccess__를 선택하면 `액세스 키 ID`와 `비밀 액세스 키`가 나오는데 이 두개는 화면이 넘어가면 다시 확인이 __불가능__하기 때문에 따로 저장해준다. 

이제 boto3를 사용할 차례이다. 이 boto3는 Python 개발자가 Python 용 Amazon Web Services (AWS) SDK를 사용하여 S3 및 EC2와 같은 Amazon 서비스를 사용하는 소프트웨어를 작성할 수있게 도와준다. Boto는 사용하기 쉬운 객체 지향 API와 저급 직접 서비스 액세스를 제공한다. boto3의 사용법은 다음과 같다

```
# 터미널에서 `~` 위치로 이동한다.
$ cd ~

# .aws 파일을 만들고 그 안에 credentials파일과 config파일을 vim을 통해 만들고
# 다음의 값들을 입력해준다 
$ mkdir .aws
$ vim credentials
~/.aws/credentials
[default]
aws_access_key_id = YOUR_ACCESS_KEY
aws_secret_access_key = YOUR_SECRET_KEY
	
$ vim config
[default]
region=ap-northeast-2
output=json

```
이렇게 설정해 놓으면 .aws에 있는 파일들을 boto3가 자동으로 읽어온다.

Django환경 폴더에서 다음 두개의 파일을 설치해준다

```
$ pip install boto3
$ pip install ipython
```

그리고 ipython을 실행하여 다음 명령어들을 입력해준다.

```
[1]: import boto3
[2]: client.create_bucket(
		  Bucket='<bucket 이름>',
		  CreateBucketConfiguration={
		  		'LocationConstraint': 'ap-northeast-2'
		  }
) 
```
여기에 파일을 올리기 위해서는 다음과 같은 명령어를 사용해 준다 

```
s3 = boto3.resource('s3')
s3.meta.client.upload_file('<올릴 파일 경로>', '<bucket 이름>', '<올릴 파일 이름>' ExtraArgs={'ACL': 'public-read'})
```
> ExtraArgs 속성을 안넣어주면 올라간 파일을 읽지 못한다. 따라서 ExtraArgs속성을 추가해 파일에 대한접근 권한을 받아서 읽어온다. 


## Docker 


### Docker의 정의

먼저 도커(Docker)를 설치하기에 앞서 간략하게 말하면, 도커는 컨테이너 기반의 오픈소스 가상화 플랫폼이다.  여기서 말하는 __컨테이너__는 __이미지__와 더불어서 도커에서 가장 중요한 개념이다. 

컨테이너는 서버에서 이야기하는 컨테이너와 비슷하다. 다양한 프로그램, 실행환경을 컨테이너로 추상화하고 동일한 인터페이스를 제공하여 프로그램의 배포 및 관리를 단순하게 해준다.

이미지는 컨테이너 실행에 필요한 파일과 설정 값등을 포함하고 있는 것으로 상태값을 가지지 않고 변하지 않는다. 

컨테이너는 이미지를 실행한 상태라고 볼 수 있고 추가되거나 변하는 값은 컨테이너에 저장된다. 따라서 같은 이미지에서 여러개의 컨테이너를 생성할 수 있고 컨테이너의 상태가 바뀌거나 컨테이너가 삭제되더라도 이미지는 변하지 않고 남아있다. 

### Docker 설치하기

도커를 설치하기 위해서는 [도커 홈페이지](https://www.docker.com/get-started)에서 다운받으면 된다. brew cask 설치를 지원하는 것도 같은데 정확히는 잘 모르겠다. 
`brew cask install docker`명령어로도 잘 설치가 됐던 것 같다.

설치를 확인하기 위해 아래의 코드를 입력해 본다.

	docker version

위의 코드를 입력하면 아래와 같이 나온다. 

Output:

```
Client:
 Version:           18.06.1-ce
 API version:       1.38
 Go version:        go1.10.3
 Git commit:        e68fc7a
 Built:             Tue Aug 21 17:21:31 2018
 OS/Arch:           darwin/amd64
 Experimental:      false

Server:
 Engine:
  Version:          18.06.1-ce
  API version:      1.38 (minimum version 1.12)
  Go version:       go1.10.3
  Git commit:       e68fc7a
  Built:            Tue Aug 21 17:29:02 2018
  OS/Arch:          linux/amd64
  Experimental:     true

```
Client와 Server 정보가 정상적으로 출력되면 설치가 완료된 것이다.

> 이렇게 버전정보가 클라이언트와 서버로 나누어져 있는 것은 도커가 하나의 실행파일이지만 실제로는 클라이언트와 서버역할을 각각 할 수 있기 때문이다. 도커 커맨드를 입력하면 도커 클라이언트가 도커 서버로 명령을 전송하고 결과를 받아 터미널에 출력해준다.


![docker client-host](/assets/deploy/docker-host.png)

### Docker 실행하기

도커 설치가 정상적으로 다 됐으면 다음 명령어를 사용하여 도커를 실행시켜본다. 

	$ docker run --rm -it ubuntu:18.04 /bin/bash
	
위 명령어는 컨테이너 내부에 들어가기 위해 `bash 쉘`을 실행하고 키보드 입력을 위해 `-it`옵션을 준다. 그리고 `--rm`옵션은 프로세스가 종료되면 컨테이너가 자동으로 삭제되도록 옵션을 추가해준 것이다. 도커 환경에서 빠져 나올때는 `exit`라고 치면 원래 터미널로 돌아온다. (컨테이너도 같이 종료) 

추가적으로 `-it`와 `--rm`과 같은 명령어들을 한번 살펴보자. 

{:.table.table-key-value-60}
옵션 | 설명
---- | ----
-d | detached mode 흔히 말하는 백그라운드 모드
-p | 호스트와 컨테이너의 포트를 연결 (포워딩)
-v | 호스트와 컨테이너의 디렉토리를 연결 (마운트)
-e | 컨테이너 내에서 사용할 환경변수 설정
--name | 컨테이너 이름 설정
--rm | 프로세스 종료시 컨테이너 자동 제거
-it | -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션
--link | 컨테이너 연결 [컨테이너명:별칭]


### Dockerfile

우리는 pyCharm에서 사용했던 Django코드들을 Docker에 보내 실행할 것이다. 일단 Dockerfile을 만들어 준다. Dockfile의 위치는 `app`폴더와 같은 위치에 만들어준다. 

```
ec2-deploy
│ 
├── Dockerfile
├── README.md
├── app
│   ├── config
│   ├── manage.py
│   │   │   
│   │   │   
│   │   │ 
├── requirements-dev.txt
└── requirements.txt

```

> 위 파일중에 `requirements.txt` 와 `requirements-dev.txt` 파일을 나눈 것을 볼 수 있는데 이렇게 파일을 나눈 이유는 내 로컬 환경에서 `ipython`같은 패키지를 설치하였을 때 같이 설치되는 파일들이 있다. 이들을 `pip freeze`명령어를 사용해 저장하면 쓰지 않는 패키지까지 같이 저장이된다. docker에서 `requirements.txt` 파일을 사용할 때 이렇게 필요없는 패키지들을 빼주기 위해서 따로 `requirements-dev.txt` 파일을 만들어서 관리해 준다. 

이제 __Dockerfile__을 만들었으면 그 안에 내용을 추가해준다. 이 Dockerfile의 역할을 Docker container을 실행할 때 이 Dockerfile안에 있는 명령어들을 사용하여 컨테이너에 필요한 환경들을 설치 및 설정해 준다. 

`Dockerfile`

```
# Docker를 ubuntu(v.18.04)에서 실행
FROM    		ubuntu:18.04
MAINTAINER 	dreamong91@gmail

# -y 속성은 설치 도중 yes/no 가 나왔을 때 자동으로 yes를 선택하는 옵션
RUN				apt -y update
RUN 			apt -y dist-upgrade
RUN				apt -y install python3-pip

# 현재 Django의 위치(ec2-deploy)의 파일들을 docker의 /srv/project/ 경로에 복사
COPY 			.	/srv/project/
# /srv/project 로 이동
WORKDIR		/srv/project
# 이동한 위치에서 requirements.txt 안에 pip3 패키지들을 설치
RUN				pip3 install -r requirements.txt

WORKDIR		/srv/project/app
# 이동한 위치에서 runserver 실행
CMD				python3 manage.py runserver 0:8000
```

위의 Dockerfile 설정을 한 후에 아래의 명령어를 통해 docker의 컨테이너에 필요한 환경을 셋팅한다.

	$ docker build -t ec2-deploy -f Dockfile .
	
이렇게 하면 Dockerfile 안의 내용들이 자동으로 실행된다. 그 후 Docker의 로컬 서버와 Client의 로컬 서버를 연결해준다. 명령어는 다음과 같다 

	$ docker run --rm -it -p 7000:8000 ec2-deploy
	
위 명령어를 실행하면 Client(현재 사용중인 내 컴퓨터)의 `localhost:7000`과 Docker의 `localhost:8000`이 연결된다. 현재 작업중인 Client 환경에서 `localhost:7000`를 실행하면 Django 파일들이 실행된 화면이 나타난다.

![Start Django](/assets/deploy/StartDjango.png)

