FROM alpine
MAINTAINER Walker Lee <walkerlee.tw@gmail.com>

ARG BUILD_REPO=andyet/SimpleWebRTC
ARG BUILD_BRANCH=master

WORKDIR /usr/src/app

RUN apk add --no-cache nodejs

ADD https://raw.githubusercontent.com/$BUILD_REPO/$BUILD_BRANCH/package.json .
RUN npm install

ADD https://api.github.com/repos/$BUILD_REPO/commits/$BUILD_BRANCH /tmp/app-version
RUN apk add --no-cache --virtual .src-dependencies wget ca-certificates tar jq \
    && wget -q -O- https://github.com/$BUILD_REPO/archive/$BUILD_BRANCH.tar.gz | tar zxf - --strip-components=1 -C /usr/src/app \
    && jq -r .sha /tmp/app-version > /usr/src/app/version \
    && apk del .src-dependencies \
    && rm -rf /var/cache/apk/* /tmp/app-version

EXPOSE 8000 8001

CMD [ "node", "server" ]
