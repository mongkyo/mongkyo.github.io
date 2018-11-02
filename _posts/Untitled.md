uwsgi 설정

uwsgi

`# 8000번 포트에 연결`
--http :8000 \ 



config에서 settings.py파일을 나눈다 

export DJANGO_SETTINGS_MODULE=config.settings.dev

django환경변수에 config.settings.dev를 추가한다


settings 모듈을 Import할때는 

from django.conf import settings

에서 해준다 



STATICFILES_DIRS = [
	'정적파일을 찾을 경로1',
	'정적파일을 찾을 경로2',
	'정적파일을 찾을 경로3',
	'정적파일을 찾을 경로4',
	'정적파일을 찾을 경로5',

]
app/
	app1
		static/
	app2
		static/
	app3
		static/
	app4
		static/


django.contrib.staticfiles
-> 9개 static폴더의 내용을 하나의 폴더에 합침

Nginx(Web Server)
-> STATIC_URL을 기준으로 온 내용은 위에서 합친 하나의 폴더안에서 검색
-> MEDIA_URL

{% static 'pby.jpg' %}
라는 파일을 찾을때 위의 모든 경로(9개)에서 찾는 로직을 갖는다. 이렇게 되면 시간이 너무 오래 걸리기 때문에 하나의 파일 안에서 검색하는 방법을 생각하게 됨. 그것이 Nginx의 역할


위의 합치는 명령어: ./manage.py collectstatic
명령어를 실행하기 전에 settings.base.py에 
STATIC_ROOT설정을 해준다 


ec2-deploy바로 아래에(app폴더와 같은 위치) .config파일을 만들고 그 안에 app.nginx파일과 uwsgi.ini파일을 만들어준다.


Dockerfile과 app.nginx, uwsgi.ini 두 파일에 각각 명령어 및 코드 작성 후 두개의 터미널에서 docker 연결한뒤 각 터미널에 순서대로 입력. 

uwsgi --ini /srv/project/.config/uwsgi.ini
