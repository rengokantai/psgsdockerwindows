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

```
