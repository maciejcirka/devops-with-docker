## 3.6
- Change in size:
```
REPOSITORY     TAG       IMAGE ID       CREATED             SIZE
frontend_new   latest    ac37aca2850d   9 minutes ago       6.73MB
backend_new    latest    22b34b631bc8   23 minutes ago      23.6MB
frontend       latest    cc5af8c37537   49 minutes ago      435MB
backend        latest    82133036cf7d   57 minutes ago      447MB
```


- Change on backend Dockerfile:
```
FROM golang:1.16-alpine as build-env
WORKDIR /usr/src/app
COPY . .
RUN go build -o goapp

FROM alpine
WORKDIR /usr/src/app
COPY --from=build-env /usr/src/app/goapp /usr/src/app
EXPOSE 8080
ENV PORT 8080
ENV REQUEST_ORIGIN http://localhost:5000
RUN adduser -D appuser && \
    chown appuser /usr/src/app
USER appuser
CMD ["./server"]
```
- Change on frontend Dockerfile:
```
FROM node:16-alpine as build-env
WORKDIR /usr/src/app
COPY . .
RUN npm install && \
    npm install  -g server && \
    npm run build

FROM alpine
WORKDIR /usr/src/app
COPY --from=build-env /usr/src/app/build /usr/src/app/build
ENV REACT_APP_BACKEND_URL http://localhost:8080
EXPOSE 5000
RUN adduser -D appuser && \
    chown appuser /usr/src/app
USER appuser
CMD ["serve", "-s", "-l", "5000", "build"]
```