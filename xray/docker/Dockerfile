#
# Builder
#
FROM golang:alpine AS builder
ARG version
WORKDIR /root
RUN apk add --no-cache git gcc musl-dev ca-certificates && \
    git clone --branch $version --depth 1 https://github.com/XTLS/Xray-core.git && \
    cd Xray-core && \
    go build -o xray -trimpath -ldflags "-s -w -buildid=" ./main

#
# Xray image
#
FROM alpine:latest
ARG timezone
COPY --from=builder /root/Xray-core/xray /usr/bin/xray
RUN apk add --no-cache tzdata ca-certificates && \
    cp /usr/share/zoneinfo/$timezone /etc/localtime && \
    apk del tzdata
WORKDIR /root
CMD ["xray", "-config", "/etc/xray/config.json"]
