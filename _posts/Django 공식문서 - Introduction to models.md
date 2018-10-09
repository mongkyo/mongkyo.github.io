---
title: "Django 공식문서 - Introduction to models"
categories: 
  - Dev
tags:
    - mac
    - Django
    - Document
author_profile: true

toc: true
toc_label: "My Talbe of Contents"
toc_icon: "cog"
toc_sticky: true

last_modified_at:
comments: true
---

## 

- [원본문서](https://docs.djangoproject.com/en/2.0/topics/db/models/)

--

### Models
모델은 데이터에 대한 정보를 나타내는 소스이다. 이 소스들은 가지고 있는 데이터의 필수 필드와 함수(행동)을 포함한다. 일반적으로, 각각의 모델은 데이터베이스 테이블에 매핑된다.

- 기본사항
	- 각각의 모델은 `django.db.models.Model`의 서브 클래스이다.
	- 모델의 각 속성은 데이터베이스의 필드를 나타낸다
	- 이것들을 이용해서 Django는 데이터베이스 액세스 API를 제공한다. 
	
	> 참조: [쿼리만들기](https://docs.djangoproject.com/en/1.10/topics/db/queries/)
	
-

### Quick example

이 document 실습할 폴더 안에서 장고를 실습할 가상 환경을 세팅해준다.

```
# pyenv 설정
$ pyenv virtualenv [설치할 파이썬 버전] [가상 환경 폴더 이름]
$ pyenv local [가상 환경 폴더 이름]

# Django와 ipython 설치
$ pip install django ipython

# git 설정
git ignore
git .gitignore

# requirement.txt파일 생성
freeze > requirement.txt

# Django 시작
$ django-admin startproject config
$ mv config app

>> pyCharm 설정 세팅
> 위치 : /usr/local/var/pyenv/versions/[가상환경 폴더이름]/bin/python

>> manage.py가 있는 폴더 안에서 
$ ./manage.py startapp fields

```
이렇게 설정을 해주면 기본적인 django document를 실습 할 준비가 끝났다. 

`fields`폴더 안에 있는 `models.py` 파일에 다음과 같이 입력한다. 

```
from django.db import models

class Person(models.Model):
	SHIRT_SIZES = (
		('S', 'Small'),
		('M', 'Medium'),
		('L', 'Large'),
	)
	name = models.CharField(max_length=60)
	shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
```
이를 입력 한 후에 데이터를 보낸다. 방법은 아래와 같다. 

```
$ ./manage.py makemigrations
$ ./manage.py migrate
```
1. 마이그레이션 파일 (초안) 생성하기 : makemigrations

2. 해당 마이그레이션 파일을 DB에 반영하기 : migrate

