FROM node:12.2.0-alpine AS client-builder

ADD . /builder
WORKDIR /builder

RUN npm install && npm run build


FROM golang:1.14-alpine AS server-builder

RUN apk --update add ca-certificates

WORKDIR /builder
ADD ./server ./server

RUN cd ./server && go mod tidy && GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build -ldflags="-w -s" -o service ./main.go


FROM scratch AS production

COPY --from=client-builder /builder/build /client
COPY --from=server-builder /builder/server/service /
COPY --from=server-builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt

ENV GIN_MODE=release

EXPOSE 3000

ENTRYPOINT [ "/service" ]
