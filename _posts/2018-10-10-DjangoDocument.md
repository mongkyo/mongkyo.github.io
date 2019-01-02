---
title: "Django 공식문서 - Relation & Inheritance"
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


## OneToOne Field

OneToOne Field에 `primary_key = True` 속성을 주면 이 속성을 가지고 있는 Class는 독립적인 pk를 갖지 않는다. 상속받은 class, 즉 부모 class의 pk를 일방적으로 따라 쓴다. 따라서 연결되어 있는 부모 class의 값이 지워지면 같이 지워진다.


## ManyToMany 모델

기본적으로 ManyToMany 모델은 Facebook과 같은 서비스의 친구 관계와 같다. 한명의 유저가 여러명의 유저와 친구 관계를 맺을 수도 있고, 그 한명의 유저는 다른 유저의 친구목록에도 들어가기 때문에다. 즉 친구 목록(친구들)을 갖는 주체로, 다른 유저의 친구 목록에 있는 친구들 중 한명으로도 쓰일 수 있다.



```python
ex)

from django.db import models


class FacebookUser(models.Model):
    name = models.CharField(max_length=50)
    friends = models.ManyToManyField(
        'self',
    )

    def __str__(self):
        """
        User.name (친구: a,b,c)
        QuerySet순회 및 문자열 포매팅
        :return:
        """
        friend_list = self.friends.all()
        friend_list_str = ', '.join([friend.name for friend in friend_list])
        return f'{self.name} (친구: {friend_list_str}' 
```
위의 코드를 보면 `Pizaa`클래스에 `Topping`클래스와 ManyToMany 관계를 형성해주는 코드를 넣는다. 이 ManyToMany에 대한 속성을 어떤 모델이 갖는지는 중요하지 않다. 하지만 반드시 한 모델에만 쓰여야 한다. 

다음으로는 DB에 객체를 생성해보자. manage.py가 있는 파일에서 `./manage.py shell`명령어를 실행하면 된다. 이때 해당 폴더(가상 폴더)에 `ipython`이 있으면 좋다. 또한 `django-extensions`도 있으면 좋으니 설치해준다. 이 `django-extensions`는 `./manage.py shell`실행시 import항목들을 같이 실행시켜준다. 

	user1, user2, user3 = [Facebook.objects.create(name=name) for name in 'user1 user2 user3'.split()]

만약 iptyhon에서 나갔다가 다시 들어갔을 때 한번에 변수에 넣고 싶을 때는 아래와 같은 방법을 사용한다. 위의 코드를 다시 입력하여 실행하면 데이터가 추가로 들어가기 때문에 아래와 같은 방법을 사용한다.

```
1) 변수의 수와 생성된 데이터의 수가 같을 때,
	user1, user2, user3 = Facebook.objects.all()
2) 변수의 수보다 데이터의 수가 더 많을 때,
	user1, *args = Facebook.objects.all()
```

2)번의 경우 `args`에 `user1`에 할당되고 남은 모든 데이터들이 들어간다. 통상적으로 변수명을 `args`라고 해준다. (user1, *args, user2도 가능하다. 이때 user1, user2에는 양 끝 값이 들어간다.)
이 `*`(asterisk)에 관련된 추가적인 내용은 [이 곳](https://mingrammer.com/understanding-the-asterisk-of-python/)을 참조하자

---

## Extra field on ManyToMany relationship


두 모델 사이의 관계와 데이터를 나타낼 때 ManyToManyField로 나타다낸다. 이 관계의 세부사항을 추가할 때 중간 모델을 사용하여 연결한다. 이 중간 모델은 `though`를 사용한다.

```python
from django.db import models


class Person(models.Model):
    name = models.CharField(max_length=128)

    def __str__(self):
        return self.name


class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(
        Person,
        through='Membership',
        related_name='group_set',
        related_query_name='group',
    )

    def __str__(self):
        return self.name


class Membership(models.Model):
    person = models.ForeignKey(
        Person,
        on_delete=models.CASCADE,
    )
    group = models.ForeignKey(
        Group,
        on_delete=models.CASCADE,
    )
    date_joined = models.DateTimeField()
    invite_reason = models.CharField(max_length=64)
```
이 중간 모델을 정의할 때, 명시적으로 ManyToMany관계에 참여하는 모델들의 Foreignkey를 지정한다. 그리고 중간 모델에는 몇가지 제한 사항이 있다. 

- 중간 모델의 제한 사항
	-  중간 모델은 __반드시__ 원본 모델에 대해 __단 한개__의 Foreignkey Field를 가져야 한다.
	-  자기 자신에 대해 ManyToMany관계를 가지는 경우 중간 모델에서 동일한 모델에 대한 Foreignkey Field를 2개 선언할 수 있다.
	-  자기 자신에게 ManyToMany관계를 가지고 중간 모델을 직접 선언하는 경우, ManyToMany Field에 `symmetrical` 옵션을 __False__로 설정한다 

----


## Recursive symmetrical False
symmetrical 속성은 인스타그램과 같이 내가 팔로우를 한다고 해서 자동으로 상대방과 연결되는 개념이 아닌, 일방적으로 나만 팔로우를 할 수 있는 기능을 말한다. 코드를 통해 예를 들어보자 

```python
from django.db import models


class InstagramUser(models.Model):
    # A, B
    # A가 B를 follow한 경우
    #   A는 B의 follower  (팔로워)
    #   B는 A의 following (팔로우)
    name = models.CharField(max_length=50)

    following = models.ManyToManyField(
        'self',
        symmetrical=False,
        related_name='followers',
    )
    followers = models.ManyToManyField(
        'self',
        symmetrical=False,
        related_name='following',
    )

    def __str__(self):
        return self.name
```
위와 같은 파일을 생성하고 아래의 코드를 실행해보자.

```
[1] 박보영, 수지 = [Instagram.objects.create(name=name) for name in '박보영 수지'.split()]

[2] 박보영.following.add(수지) 		# 박보영의 follow에 수지를 추가

[3] 박보영.following.all() 			# 박보영이 follow하는 목록
>> <QuerySet [<InstagramUser: 수지>]>

# 거꾸로 출력하기
[4] 수지.instagramuser_set.all()		# 수지를 follow하는 사람들 목록
# related_name을 사용하여 출력
[4-1] 수지.followers.all()
>> <QuerySet [<InstagramUser: 박보영>]>
```
> `related_name`의 기본값은 '클래스명의 `소문자형_set`이다. 이 `related_name`을 통해서 역으로 참조하는 것이 가능하다. 위의 함수에서는 `related_name`에 `followers`와 `following`을 쓰고 있다.


---

## intermediate model (중간 모델)

일반적인 ManyToMany Field와 달리 add(), create(), set() 명령어 들을 사용할 수 없다. 만약 사용하게 되면 아래와 같은 오류가 뜬다.
	
	ex)
	beatles.objects.add(ringo)
	>> Error! Use many_to_many Membership's Manager instead

이러한 오류를 바로 잡기 위해 아래의 `intermediate model을 사용한다. 

```python
Membership.objects.create(
			person = ringo,
			group = beatles,
			date_joined = date(1962, 8, 16),
			invite_reason = 'Needed a New drummer.'
```

이렇게 되는 이유는 Person과 Group의 관계를 설정할 때 중간 모델의 필드값을 명시해줘야하기 때문이다. 단순히 add(), create(), set()을 사용하는 경우에 `'person`과 `group`속성 값은 알 수 있지만, __`date_joined`__값과 __`invite_reason`__값은 알 수 없기 때문이다. 따라서 중간 모델의 속성들을 직접 정의한 경우에는, 반드시 중간 모델을 사용하여 생성하는 방법 밖에 없다.
