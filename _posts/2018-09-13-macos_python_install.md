---
title: "2018.09.10 - masOS에 python 설치하기"
categories: 
   - Dev
tags:
    - mac
    - python
    - 
author_profile: true

toc: true
toc_label: "My Talbe of Contents"
toc_icon: "cog"
toc_sticky: true

last_modified_at:
comments: true
---




## python 
 
- pyenv 란? 

파이썬 버전별로 관리를 해준다. 버전끼리 호환이 안되는 경우가 많기 때문에 그 버전들을 관리해준다. 버전에 대한 가상환경을 통해 독립적으로 프로그램을 설치하게 해준다. 보통은 프로젝트별로 가상환경을 하나씩 만들어서 clean한 공간에 새로 프로그램 또는 라이브러리를 설치하여 작업을 진행하게 해준다. 


- mac에서 pyenv 설치 


```
$ brew install pyenv
$ brew install pyenv-virtualenv
```
설치 후 

- ~/.zshrc 에 아래 내용 추가

```
# pyenv
export PYENV_ROOT=/usr/local/var/pyenv
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi
```

- 파이썬 설치 전 필요한 패키지 

```
$ brew install readline xz 
$ pyenv install [원하는 버전 ex:3.6.6]

$ pyenv versions
$ pyenv global [원하는 버전]
```
- pyenv가 관리하는 가상환경 ROOT

`(pyenv설치폴더) /envs/ <가상환경명>`

- 가상 환경 설치하기 

```
$ pyenv virtualenv [python 버전] [가상환경 폴더 이름]
$ pyenv local [가상환경 폴더 이름]
```

- 가상환경을 설정한 폴더 안에서 아래 명령어를 입력

```
$ pip list
$ pip install ipython
```

- python의 사용을 도와주는 노트북 설치 
<p>설치 후 설치한 폴더내에서 다음 명령어로 파일을 실행한다

```
$ pip install notebook
$ jupyter notebook
```
