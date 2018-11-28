삽질 - log 


1. elasticbeanstalk

docker run을 실행 시 localhost:7000/admin에서 502 bad gate error(nginx) 발생.

localhost:7000은 정상 작동. 

원인은 static file. 

장고 외부에 .static을 빼주고 `./manage.py collectstatic`명령어를 안해줌 

collectstatic설정을 해주고 나서 해결


2 RDS 과금

RDS의 저장 용량을 항상 확인하자 

프리티어에서는 저장 용량을 최소로 잡아서 과금이 되지 않지만

이를 복구했을 때는 프리티어가 풀릴 수 있다. 

항상 확인할 것. 