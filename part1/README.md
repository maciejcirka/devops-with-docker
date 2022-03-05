# Part 1

## 1.1: Getting started
- Commands:
```
docker run -d nginx
docker run -d nginx
docker run -d nginx
docker stop b896
docker stop 183d
```

- Result:
```
maciejcirka~ docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                      PORTS     NAMES
f4a599b81f31   nginx     "/docker-entrypoint.…"   40 seconds ago   Up 39 seconds               80/tcp    xenodochial_hamilton
183d3a2d5d57   nginx     "/docker-entrypoint.…"   42 seconds ago   Exited (0) 7 seconds ago              jovial_dirac
b896077b5a2c   nginx     "/docker-entrypoint.…"   44 seconds ago   Exited (0) 16 seconds ago             brave_kare
```

## 1.2: Cleanup

- Commands:
```
docker stop $(docker ps -q -a)
docker rm $(docker ps -q -a)
docker image prune -a
```
- Result:

```
maciejcirka~ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
maciejcirka~ docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```


## 1.3: Secret message
- Commands:
```
docker run -it -d devopsdockeruh/simple-web-service:ubuntu
docker exec -it c6e16 bash
tail -f ./text.log
```
- Result:
```
Secret message is: 'You can find the source code here: https://github.com/docker-hy'
```

## 1.4: Missing dependencies

- Commands:
```
docker run -it --name missing-dependencies ubuntu sh -c 'apt update; apt install -y curl; echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'
helsinki.fi
```

## 1.5: Sizes of images
- Images comparison:
```
maciejcirka~ docker images
REPOSITORY                          TAG       IMAGE ID       CREATED         SIZE
devopsdockeruh/simple-web-service   ubuntu    4e3362e907d5   11 months ago   83MB
devopsdockeruh/simple-web-service   alpine    fd312adc88e0   11 months ago   15.7MB
```
- Commands:
```
docker run -d -it devopsdockeruh/simple-web-service:alpine
docker exec -it dc24 sh
tail -f ./text.log
```


## 1.6: Hello Docker Hub
- Commands:
```
docker run -it devopsdockeruh/pull_exercise
```

- Result:
```
Give me the password: basics
You found the correct password. Secret message is:
"This is the secret message"
```


## 1.7: Two line Dockerfile

- Dockerfile:
```
FROM devopsdockeruh/simple-web-service:alpine

CMD server
```
- Commands:
```
docker build . -t web-server
docker run web-server
```

## 1.8: Image for script

- Dockerfile:
```
FROM ubuntu:18.04

RUN apt-get update; apt-get install -y curl;

CMD echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;
```

- Commands:
```    
docker build . -t curler
docker run -it curler
```

## 1.9: Volumes
- Commands:
```
touch text.log
docker run -v /Users/maciejcirka/text.log:/usr/src/app/text.log devopsdockeruh/simple-web-service
```

## 1.10: Ports open

- Commands:
```
docker run -d -p 8080:8080 devopsdockeruh/simple-web-service sh -c 'server'
```


## 1.11: Spring

- Dockerfile:
```
FROM openjdk:8

WORKDIR /usr/src

COPY . .

RUN ./mvnw package

CMD ["java", "-jar", "./target/docker-example-1.1.3.jar"]

EXPOSE 8080
```

- Commands:
```
git clone https://github.com/docker-hy/material-applications.git
cp Dockerfile material-applications/spring-example-project/
cd material-applications/spring-example-project/
docker build . -t spring-project
docker run -p 8080:8080 spring-project
```

## 1.12: Hello, frontend!

- Dockerfile:
```
FROM node:14

WORKDIR /usr/src 

COPY . .

RUN npm install
RUN npm run build
RUN npm install -g serve


CMD ["serve","-s","-l","5000","build"]
```
- Commands:
```
git clone https://github.com/docker-hy/material-applications.git
cp Dockerfile material-applications/example-frontend/
cd material-applications/example-frontend/
docker build . -t frontend-project
docker run -p 5000:5000 frontend-project
```

## 1.13: Hello, backend!

- Dockerfile:
```
FROM golang:1.16

WORKDIR /usr/src

EXPOSE 8080

COPY . .

RUN go build

RUN go test ./...

CMD ./server
```
- Commands:
```
git clone https://github.com/docker-hy/material-applications.git
cp Dockerfile material-applications/example-backend/
cd material-applications/example-backend/
docker build . -t backend-project
docker run -p 8080:8080 backend-project
```

## 1.14: Environment

- Dockerfile (frontend-project):
```
FROM node:16

WORKDIR /usr/src/app

COPY . .

ENV REACT_APP_BACKEND_URL http://localhost:8080

EXPOSE 5000

RUN npm install && npm run build && npm install -g serve

CMD ["serve", "-s", "-l", "5000", "build"]
```
- Dockerfile (backend-project):
```
FROM golang:1.16

WORKDIR /usr/src/app

EXPOSE 8080

ENV PORT 8080

ENV REQUEST_ORIGIN http://localhost:5000

COPY . .

RUN go build && go test ./...

CMD ["./server"]
```
- Commands (each in the repos from exercise 1.12 and 1.13):
```

docker build . -t hello-backend
docker run -d -p 8080:8080 hello-backend

docker build . -t hello-frontend
docker run -d -p 5000:5000 hello-frontend
```

## 1.15: Homework

- Commands:
```
docker pull maciejcirka/youtube-dl
docker run maciejcirka/youtube-dl <youtube_url_here>
docker cp "<container_id>://mydir" .
```


## 1.16: Heroku

- Link:
```
https://git.heroku.com/maciejcirka-app.git
```