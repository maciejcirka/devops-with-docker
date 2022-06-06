## 3.3
- Change on backend Dockerfile:
```
FROM golang:1.16

WORKDIR /usr/src/app

EXPOSE 8080

ENV PORT 8080

ENV REQUEST_ORIGIN http://localhost:5000

COPY . .

RUN go build && go test ./...

RUN useradd -m appuser && chown appuser /usr/src/app

USER appuser

CMD ["./server"]
```
- Change on frontend Dockerfile:
```
FROM node:16

WORKDIR /usr/src/app

COPY . .

ENV REACT_APP_BACKEND_URL http://localhost:8080

EXPOSE 5000

RUN npm install && npm run build && npm install -g serve

RUN useradd -m appuser && chown appuser /usr/src/app

USER appuser

CMD ["serve", "-s", "-l", "5000", "build"]
```