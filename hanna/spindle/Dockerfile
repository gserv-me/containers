from alpine:edge as builder

workdir /src

run apk add alpine-sdk git go
run git clone --depth=1 https://tangled.org/tangled.org/core .
run mkdir -p bin && go build -o bin/spindle ./cmd/spindle

from alpine:edge as runner

env SPINDLE_PIPELINES_LOG_DIR=/var/log/spindle
env SPINDLE_SERVER_DB_PATH=/var/lib/spindle/spindle.db

run apk add shadow docker
run mkdir -p /var/lib/spindle && mkdir -p /var/log/spindle

copy --from=builder /src/bin/spindle /usr/bin/spindle

run useradd -G docker -d /var/lib/spindle spindle && \
    chown -R spindle /var/log/spindle && \
    chown -R spindle /var/lib/spindle

user spindle
cmd ["spindle"]
