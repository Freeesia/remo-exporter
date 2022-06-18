ARG BASE_IMAGE=alpine:3.9

FROM golang:1.11.5-alpine AS build-env
WORKDIR /go/src/github.com/kenfdev/remo-exporter
COPY . .
RUN sh ./scripts/build-bin.sh

FROM ${BASE_IMAGE}

ARG EXPORTER_BINARY="remo-exporter-linux-amd64"
COPY --from=build-env /go/src/github.com/kenfdev/remo-exporter/dist/${EXPORTER_BINARY} ./remo-exporter

RUN apk --no-cache add ca-certificates \
     && addgroup exporter \
     && adduser -S -G exporter exporter
USER exporter

ENV LISTEN_PORT=9352
EXPOSE 9352
ENTRYPOINT [ "./remo-exporter" ]
