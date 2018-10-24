---
title: "01 - Django 개발환경, "
categories: 
  - Django
tags:
    - mac
    - Django
    - Pycharm CE
author_profile: true

toc: true
toc_label: "My Talbe of Contents"
toc_icon: "cog"
toc_sticky: true

last_modified_at:
comments: true
---

# Django 시작하기


#### [__Djangogirls tutorial__](https://tutorial.djangogirls.org/ko/django_models/) 참고

- - - -

## 1. setting 

 
- terminal안에 Django를 사용할 폴더를 만들어주고 가상환경을 설정해준다

```
폴더 설정 
project/
	Django/
		Djangogirls-tutorial/
			.git
			.gitignore
```

- 가상환경 만들기

```
$ pyenv virtural [파이썬 버전,3.6.6] [가상환경 폴더 이름:fc-djangogirls]
$ pyenv local [사용할 가상환경 폴더 이름:fc-djangogirls]
```	

- 이 `Djangogirls-tutorial`폴더 안에서 장고를 설치한다

```
$ pip install --upgrade pip
$ pip install django~=1.11.0 

# Django버전 1.11중에 최신버전을 설치한다는 뜻이다. 
# 현재 최신버전의 Django가 더 나왔지만 Djangogirls-tutorial에서 설명하는 Django의 버전이 1.11버전이기 때문에 맞춰서 설정해준다
```
> pycharm에서 실행할 경우에 가상환경 폴더 안의 py파일들이 정상적으로 작동하지 않는다면 preferences(`cmd`+`,`)에서 `Project Interpreter`에서 `show all`에서 가상환경 상의 python을 추가해준다. 이때 `Existing environment` 안에서 설정을 해줘야 한다

위치 : `/usr/local/var/pyenv/versions/[가상환경 폴더이름]/bin/python` 

-
## 2. Djangogirls-Tutorial Start

프레임워크 :  특정 기능들을 모아놓은 프로그램, 프로그램을 만들기 위한 프로그램

- Django = 프레임워크

- urlresolver : url해석기


- http://(어떤도메인)/posts/<br>
	-> 블로그의 모든 Post목록
	
- http://(어떤도메인)/posts/1/<br>
	-> 1번 Post를 보여줌

- Django시작하기
--
Django가 설치가 되면 `django-admin.py`명령어를 사용 할 수 있게된다
이를 통해 mysite라는 새로운 프로젝트를 시작한다 

```
$ django-admin.py startproject mysite

# 명령어를 실행하고 나면 아래와 같은 tree구조를 가진 폴더 [mysite]가 생성된다

djangogirls-tutorial-practice		<- 프로젝트 폴더
└── mysite							<- Django폴더
    ├── manage.py					<- Django유틸리티 모듈
    └── mysite						<- Django설정 패키지
        ├── __init__.py
        ├── settings.py				<- Django설정 모듈
        ├── urls.py
        └── wsgi.py
     
        
```

첫번째 `mysite` 폴더는 `app`으로 바꿔준다 
두번째 `mysite`는 `config`로 바꿔준다

> 이름을 바꿔줄때는 이 `mysite`가 패키지 이름으로 경로로 참조되고 있는 경우가 있기 때문에 이 패키지를 참조하고 있는 파일들의 `mysite`부분들을 다 같이 바꿔줘야한다. 이러한 기능을 __PyCharm__에서는 제공하고 있는ㄴ데 폴더이름에서 우클릭하면 `Refactor`가 있다 이를 통해 바꿔준다 

정상적으로 여기까지 잘 따라왔다면 아래 명령어를 통해 확인이 가능하다

	$ python manage.py runserver

Django에서 class를 생성하면 DB에 자동으로 저장한다

	$ python manage.py startapp blog

config위에 blog라는 폴더가 생성된다 

	$ python manage.py migrate 
	
DB변경사항을 실제 DB에 저장시키는 것이다 이 명령어를 실행하고 나면 app폴더 안에 db.sqlite3라는 파일이 생성된다 이 파일은 SQLite를 통해 확인해볼수있다 

그럼 blog 폴더 안의 models.py에 다음 코드를 작성해 보자

```
from django.db import models
from django.utils import timezone


class Post(models.Model):
    author = models.ForeignKey(
        'auth.User',
        on_delete=models.CASCADE
    )
    title = models.CharField(max_length=200)
    text = models.TextField(blank=True)
    created_date = models.DateTimeField(default=timezone.now)
    published_date = models.DateTimeField(blank=True, null=True)


	def publish(self):
	    self.published_date = timezone.now()
	    self.save()
	
	
	def __str__(self):
	    return self.title

```

>class가 models.Model를 상속 받는 이유는 어떤 모델 클래스가 있을때 데이터베이스와 연동되는 것은 자동이 아니다 그 연동시켜주는 역할을 models.Model이 한다 

이렇게 작성한 코드들이 데이터베이스에 입력되기 위해서는 settings파일에 INSTALLED_APPS의 항목에 blog를 추가해준다 그리고 다음 코드를 실행한다

	$ python manage.py makemigrations blog

> blog라는 어플리케이션에 대해서 데이터베이스 변경사항을 나타낸다. 변경사항이 바로 DB에 적용되는 것이 아니라 한번 기록되고 나서 다음 코드를 실행하면 DB에 적용이 된다.

	$ python manage.py migrate 


```
admin.py 파일에 입력

from django.contrib import admin

from .models import Post

admin.site.register(Post)
```

작성하고 프로젝트 폴더로 돌아와서 다시 runserver를 실행한다 
	
	$ python manage.py runserver

> 위 코드를 실행하고 나서 도메인에 `127.0.0.1:8000/adim` 을 입력한다. 그럼 관리자 창이 뜬다
> 그리고 이 관리자 창에서 로그인 하기 위해 정보를 등록한다. 
> 등록한 정보를 통해 관리자 페이지에 들어간다.

	$ python manage.py createsuperuser
	

## View.py

model.py는 만들었으니까 view.py를 만들어보자 

```
from django.http import HttpResponse

def post_list(request):
	"""
	param request: 실제 HTTP요청에 대한 정보를 가진 객체
	"""
	return HttpResoponse('Post List')
	
```
> httpresponse를 쓰면 돌려주는 형식을 http형식으로 만들어서 돌려준다

- 구조 

```
runserver (웹 서버)
Django application
	urlresolver  (config.urls)
```
127.0.0.1:8000  ->  runserver  ->  urlresolver

> 	-> r'^$' 와 매치
> 
> 	-> 해당 요청을 blog.views.post_list에 보냄
> 
> 	-> 함수에서는 받은 요청을 처리한 결과를 리턴
> 
> 	-> 리턴된 결과는 브라우저에 표시

이를 코드로 구현해보자 <br>
(매번 요청받을때마다 요청 시간을 변경하여 브라우저에 표시한다)

```
from django.http import HttpResponse
from django.utils import timezone


def post_list(request):
    # 현재 지역에 맞는 날짜&시간 객체 할당
    current_time = timezone.now()
    """

    :param request: 실제 HTTP요청에 대한 정보를 가진 객체
    :return:
    """
    return HttpResponse(
        '<html>'
        '<body>'
        '<h1>Post list</h1>'
        '<p>{}</p>'
        '</body>'
        '</html>'.format(
            # 날짜&시간 객체가 가진 정보를 문자열로 반환
            current_time.strftime('%Y. %m. %d<br>%H:%M')
        )
    )
```    

데이터베이스에는 UTC로 저장, TIME_ZONE 지정하면 장고와 관련된 모듈에만 적용된다.
> config/settings.py에서 Time_zone을 변경하여도 데이터베이스에는 그대로 UTC기준으로 들어간다. 이 변경된 내용은 admin에서 확인 할 수 있다. 

--

 Django의  **MTV**
========


MVC 모델 | MTV모델(Django)
--------|--------
Model ( `Models.py` ) | 테이블을 정의
Template( `./template/*.html` ) | 사용자가 보게 될 화면의 모습을 정의
View ( `Views.py` ) | 어플리케이션의 제어 흐름 및 처리 로직을 정의
	

이 MTV모델에서 보통 테이블 설계(`Model`)를 먼저 작성한 후, 화면 설계(`Tamplate`,`View`)를 하는 것이 일반적이다. 또한 화면 설계 내에서도 UI화면을 생각하면서 로직을 풀어나가는 것이 더 쉽기 때문에 template에서 view순서로 코딩한다. 하지만 로직이 간단한 경우에는 view를 먼저 코딩한 후 template를 작성한다.