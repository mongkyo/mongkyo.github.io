---
title: "jupyter notebook connecting to kernel error"
categories: 
  - Dev
tags:
    - jupyter notebook
    - python
    - tornado
    - error
author_profile: true

toc: true
toc_label: "My Talbe of Contents"
toc_icon: "cog"
toc_sticky: true

last_modified_at:
comments: true
---

# jupyter notebook Connecting to kernel error


이번에 새롭게 pyenv폴더를 만들면서 jupter notebook을 설치하였다. 평소 설치 후 잘 되던 jupyter notebook에서 connecting to kernel 에러가 지속적으로 발생하여 에러를 해결하였는데 해결 방법에 대해 공유해 보려고 한다. 

## error report

처음에 jupyter notebook을 실행했을 때 아래의 그림과 같은 상황이였다. connecting to kernel 이라는 주황색 글씨가 떠있는 상태에서 작동이 하지 않았다. 그 이후 jupyter notebook을 실행한 터미널에서 연결 상태를 확인하니 다음과 같은 에러가 있었다. 

![jupyter notebook error](/assets/deploy/error.png)

```
RuntimeWarning: coroutine 'WebSocketHandler.get' was never awaited super(AuthenticatedZMQStreamHandler, self).get(*args, **kwargs)
```

## solution

내 상황에서의 해결방법을 찾아보니 tornado의 버전 문제였다. 이 tornado는 Python 웹 프레임 워크 및 비동기 네트워킹 관련 라이브러리이다. (자세한 내용은 [홈페이지](https://www.tornadoweb.org/en/stable/)에 나와있다.) 따라서 이 tornado를 삭제하고 버전을 맞추어 재설치를 해주어 해결하였다. 문제가 되었던 버전은 6.0.1버전이였는데 이전 프로젝트들이 5.1.1을 사용하고 있었고, 이전 프로젝트들이 여전히 정상적으로 잘 작동되는 것으로 보아 6.0.1버전의 문제라고 판단하였다.(버전이 올라가면서 생긴 일종의 버그(?)같음) 따라서 삭제 후 재설치하는 였는데 코드는 다음과 같다.

```
pip uninstall tornado

pip install tornado==5.1.1
```
