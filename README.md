# psgsdockerwindows
##8. Composing Applications with docker-compose
###2 Why docker-compose Exists
command
```
docker run --name db -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=pass -v db:/var/lib/mysql mysql
```
extract ip address
```
docker inspect db
```


web server
```
docker run --name web -d -p 8080:80 -e MY_DB_PORT=3306 -e MY_DB_HOST=? -v /app:/usr/share/nginx/html nginx
```


docker-compose.yml
```
version:'2'
services:
  db:
    image:mysql
    ports:
      - 3306:3306
    emvironment:
      - MYSQL_ROOT_PASSWORD=pass
    volumes:
      - db:/var/lib/mysql

web:
  image: nginx
  ports:
    - 8080:80
  environment:
    - MY_DB_PORT=3306
    - MY_DB_HOST=db
  volumes:
    - /app:/usr/share/nginx/html
```


###3 A TeamCity docker-compose.yml with 3 Containers
```
version:'2'
services:
  teamcity:
    image: sjoerdmuler/teamcity
    ports:
      - 8111:8111
  teamcity-agent:
    image: sjoerdmuler/teamcity-agent
    environment:
      - TEAMCITY_SERVER=http://teamcity:8111
  postgres:
    image: postgres
    environment:
      - POSTGRES_DB=teamcity
```


###4 Spinning Up Complex Apps with a Single Command: docker-compose up
```

```

###6 docker-compose Creates Isolated Container Networks
list commands
```
docker network ls
docker volume ls
```
inspect a network
```
docker network inspect teamcity_default
```

###7 Service Discovery via an Embedded DNS Server
```
docker exec -it teamcity_postgres_1 bash
```
is same as
```
docker-compose exec postgres bash
root@12345: /#   ping teamcity
exit
```
test another 
```
docker-compose exec teamcity bash
root@12345: /#   ping postgres
exit
```

###8 Connecting Another Container to Your User Defined Network
```
docker run --name alpine --rm --net teamcity_default alpine sh
ping teamcity
ip a s
```
```
docker-compose exec teamcity bash
ping alpine
exit
```
###9 Restarting Containers with docker-compose start
