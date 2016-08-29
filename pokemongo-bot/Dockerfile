FROM alpine
MAINTAINER Walker Lee <walkerlee.tw@gmail.com>

ARG BUILD_REPO=PokemonGoF/PokemonGo-Bot
ARG BUILD_BRANCH=master
ARG TIMEZONE=Etc/UTC

LABEL build_repo=$BUILD_REPO build_branch=$BUILD_BRANCH

WORKDIR /usr/src/app
VOLUME ["/usr/src/app/configs", "/usr/src/app/web"]

ADD https://raw.githubusercontent.com/$BUILD_REPO/$BUILD_BRANCH/requirements.txt .
ADD http://pgoapi.com/pgoencrypt.tar.gz /dev/null
RUN apk -U --no-cache add python py-pip \
    && apk --no-cache add --virtual .build-dependencies python-dev gcc make musl-dev git tzdata \
    && cp -fa /usr/share/zoneinfo/$TIMEZONE /etc/localtime \
    && echo $TIMEZONE > /etc/timezone \
    && wget -q -O- http://pgoapi.com/pgoencrypt.tar.gz | tar zxf - -C /tmp \
    && make -C /tmp/pgoencrypt/src \
    && cp /tmp/pgoencrypt/src/libencrypt.so /usr/src/app/encrypt.so \
    && ln -s locale.h /usr/include/xlocale.h \
    && pip install --no-cache-dir -r requirements.txt \
    && apk del .build-dependencies \
    && rm -rf /var/cache/apk/* /tmp/pgoencrypt /usr/include/xlocale.h \
    && find / -name '*.pyc' -o -name '*.pyo' | xargs -rn1 rm -f

ADD https://api.github.com/repos/$BUILD_REPO/commits/$BUILD_BRANCH /dev/null
RUN apk -U --no-cache add --virtual .pgobot-dependencies wget ca-certificates tar jq \
    && wget -q https://github.com/$BUILD_REPO/archive/$BUILD_BRANCH.tar.gz -O- | tar zxf - --strip-components=1 -C /usr/src/app \
    && wget -q https://api.github.com/repos/$BUILD_REPO/commits/$BUILD_BRANCH -O /tmp/pgobot-version \
    && jq -r .sha /tmp/pgobot-version > /usr/src/app/version \
    && apk del .pgobot-dependencies \
    && rm -rf /var/cache/apk/* /tmp/pgobot-version

ENTRYPOINT ["python", "pokecli.py"]
