* Use multi-stage builds with all compiled languages- > Go, Java (bytecode), Rust, C++, C, Erlang (bytecode), Haskell, Scala (bytecode), Swift

```
FROM golang:alpine AS build-env
WORKDIR /app
ADD . /app
RUN cd /app && go build -o goapp

FROM alpine
RUN apk update && apk add ca-certificates && rm -rf /var/cache/apk/*
WORKDIR /app
COPY --from=build-env /app/goapp /app

EXPOSE 8080
ENTRYPOINT ./goapp
```
