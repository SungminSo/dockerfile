FROM golang:1.14-alpine AS builder

ARG build_token
ARG organization
ARG repo_name
ARG port

RUN apk --update add ca-certificates git gcc musl-dev

ADD . /builder

WORKDIR /builder

RUN git config --global url."https://$build_token:x-oauth-basic@github.com/".insteadOf "https://github.com/"
RUN export GOPRIVATE="github.com/$organization" && go get $repo_name

RUN go mod tidy

RUN go test ./...

RUN GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -ldflags="-w -s" -o service cmd/main.go


FROM scratch AS production

COPY --from=builder /builder/service /
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt

ENV GIN_MODE=release

EXPOSE $port

ENTRYPOINT ["/service"]
