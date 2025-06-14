FROM alpine:latest
RUN apk add --no-cache bash coreutils curl

RUN curl -L -o /kaniko \
    https://github.com/GoogleContainerTools/kaniko/releases/latest/download/executor-linux-arm64 \
 && chmod +x /kaniko

ENTRYPOINT ["/kaniko"]
