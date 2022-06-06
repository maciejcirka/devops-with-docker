## 3.4/3.5
- Change in size:
```
REPOSITORY     TAG       IMAGE ID       CREATED             SIZE
frontend       latest    cc5af8c37537   58 seconds ago      435MB
backend        latest    82133036cf7d   8 minutes ago       447MB
backend_old    latest    6c2058c39082   16 minutes ago      1.07GB
frontend_old   latest    b4749b46b3f4   About an hour ago   1.23GB
```


- Change on backend Dockerfile:
```
FROM golang:1.16-alpine

WORKDIR /usr/src/app

COPY . .

RUN go build && \
    test ./... && \
    adduser -D appuser && \
    chown appuser /usr/src/app

USER appuser

CMD ["./server"]
```
- Change on frontend Dockerfile:
```
FROM node:16-alpine

WORKDIR /usr/src/app

COPY . .

RUN npm install && \
    npm run build && \
    npm install -g serve && \
    adduser -D appuser && \
    chown appuser /usr/src/app

USER appuser

CMD ["serve", "-s", "-l", "5000", "build"]
```