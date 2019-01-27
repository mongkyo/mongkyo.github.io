---
title: "Jupyter notebook font 변경"
categories: 
  - Dev
tags: 
    - macOS
    - jupter notebook
author_profile: true

toc: true
toc_label: "My Talbe of Contents"
toc_icon: "cog"
toc_sticky: true

last_modified_at:
comments: true
---

# jupyter notebook font 변경


jupyter notebook font를 변경하는 방법은 아주 간단하다. 

일단 먼저 jupyter notebook을 먼저 설치해준다.

```
$ pip install jypter
```

pip를 사용하여 jypter notebook을 설치하고 나면 홈 폴더(`cd ~`)에 보면 `.jupyter`이라는 폴더가 생성된 것을 확인할 수 있다. 

이 폴더 안에 들어가면 `migrated`라는 파일 하나만 존재하는 것을 확인할 수 있다. 

여기에 `custom`파일을 만들고 `custom`파일로 이동한다. 

```
$ mkdir custom
$ cd custom
```

그리고 이 파일 안에 `custom.css`라는 파일을 만들어준다. 이 `custom.css`파일에 우리가 변경할 설정들을 할 것이다.

```
$ vim custom.css
```
파일을 만들어주고 그 아래 코드를 적어준다. 아래의 예시 코드는 Hack이라는 font를 10pt로 사용한다는 것이다.

```css
.CodeMirror pre {font-family: Hack; font-size: 10pt; line-height: 140%;}
.container { width:100% !important; }
div.output pre{
    font-family: Hack;
    font-size: 10pt;
}
```

> Hack폰트는 [https://github.com/source-foundry/Hack](https://github.com/source-foundry/Hack)에서 다운 받을 수 있다. 

위의 설정을 마치고 jupyter notebook을 터미널에서 다시 실행하면 변경 사항이 적용된 것을 확인할 수 있다.

