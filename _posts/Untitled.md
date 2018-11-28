docker run으로 실행되어있는 container에 docker exec ...를 실행하면 .log/error.log 내용을 보여주기

docker exec eb tail -f /srv/project/.log/error.log




```
./deploy.sh -> EC2환경에 배포 

eb ssh -> eb를 사용해서 EC2에 접속


docker run  -> docker에 접속

```

애플리케이션이 배포된 이후에 실행될 script의 내용이 /opt/elasticbeanstalk/hooks/appdeploy/post 에 들어있다. 
