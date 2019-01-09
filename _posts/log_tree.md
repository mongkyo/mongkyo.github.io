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



3. 로컬 환경에서 runserver시 css가 적용되지 않을 때,

static 설정들이 올바르게 되어있다고 전제되어있을 때

DEBUG True 설정이 dev에 setting이 되어있는지 확인한다. 

장고는 자체적으로 dev에 debug False옵션이 주어져 있으면

production환경으로 인식하기 때문에 dev모드에서 지원하는 admin옵션의 

css를 지원하지 않는다. (정확한 이유는 모름....)


4. 

import를 할 수 없을 때 

pakages 분리를 통해 꼬여있는 import를 풀어준다 



5.

S3에 올라간 사진을 읽어 올 수 없을 때 

```
This XML file does not appear to have any style information associated with it. The document tree is shown below.
<Error>
<Code>NoSuchKey</Code>
<Message>The specified key does not exist.</Message>
<Key>media/post/이북집찹쌀순대-1.jpg</Key>
<RequestId>D1B1A37C6359DD13</RequestId>
<HostId>
6YfSzFEyW3Fjwv6llAgYifuNxwsAuRe7XsDnebW1C1OiJyyMDQOYa680aLa96jYaygWNcBgst10=
</HostId>
</Error>
```

S3에서 사진을 올리고 퍼블릭 설정을 변경해준다. 


6. 

Pipenv를 사용하는 경우 Home directory에 Pipfile이 있는 경우 pipenv가 항상 ~에 위치하게 된다. 

따라서 home directory에 Pipfile이 있는지 확인해야한다.


7.

DB migrate, makemigration할 때 오류가 나는 경우 

```
django.db.migrations.exceptions.InconsistentMigrationHistory: Migration admin.0001_initial is applied before its dependency members.0001_initial on database 'default'.
```

다음의 절차대로 해결한다.

![에러 처리 절차](/assets/deploy/db_error_case.png)



