# Elastos privnet - API MISC Mainchain - elastos.org
# This is an official but unsupported docker image

# alpine3.8 - use image digest for security
FROM alpine@sha256:dad671370a148e9d9573e3e10a9f8cc26ce937bea78f3da80b570c2442364406

ENV SRC_DIR="/home/elauser"

WORKDIR $SRC_DIR

COPY ./misc ./misc
COPY ./sidechain/did/config.json ./config.json
COPY ./sidechain/did/deploy.sh ./deploy.sh
COPY ./sql-init ./sql-init

# This binary requires some glibc libraries that are not on alpine by default 
# so we need to install glibc to make it work
RUN apk update \
    && apk add --no-cache mysql-client \
    && apk add --no-cache curl \
    && apk add --no-cache ca-certificates wget \
    && wget -q -O /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub \
    && wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.28-r0/glibc-2.28-r0.apk \
    && apk add glibc-2.28-r0.apk \
    && adduser -h $SRC_DIR -D -g '' elauser \
    && chown -R elauser:elauser $SRC_DIR

USER elauser

EXPOSE 8093

CMD sh -c "sh ./deploy.sh && /bin/sh"