java -jar atlassian-agent.jar -d -m admin@admin.com -n BAT -p conf -o http://localhost -s B9KK-YK2J-BTN0-WWD4

 docker run -d --netowrk confluence -p 8090:8090 --name confluence  atlassian/confluence-server

echo 'export CATALINA_OPTS="-javaagent:/atlassian-agent.jar ${CATALINA_OPTS}"' >> /opt/atlassian/confluence/bin/setenv.sh

### docker postgress
启动容器
docker run -d --network confluence -p 5432:5432 -e POSTGRES_PASSWORD=postgres --name postgres  postgres

进入docker postgress 容器
docker exec -it postgres /bin/bash

进入数据库
psql -U postgres(这是默认账号) -W [密码]

创建数据库
CREATE DATABASE confluence;