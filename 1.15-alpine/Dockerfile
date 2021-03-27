FROM golang:1.15-alpine
LABEL maintainer="Frank Wilkerson"

RUN apk add --no-cache gcc musl-dev git
RUN wget -O- -nv https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s v1.38.0
RUN GO111MODULE=on go get github.com/ory/go-acc
