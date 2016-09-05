FROM alpine
MAINTAINER Walker Lee <walkerlee.tw@gmail.com>

RUN apk -U --no-cache add python py-pip \
    && rm -rf /var/cache/apk/* \
    && find / -name '*.pyc' -o -name '*.pyo' | xargs -rn1 rm -f

CMD ["/usr/bin/python"]
