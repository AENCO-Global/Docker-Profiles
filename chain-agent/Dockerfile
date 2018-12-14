FROM golang:1
LABEL MAINTAINER="Simon Ball<simon.ball@aencoin.com>"

WORKDIR /go/src/app
COPY . .

RUN go get \
    github.com/tools/godep

RUN godep go build
RUN go build -o aen.agent && cp aen.agent $GOPATH/bin/aen.agent

CMD ["aen.agent"]
