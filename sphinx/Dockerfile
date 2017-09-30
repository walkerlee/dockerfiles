FROM walkerlee/python

RUN apk add --no-cache nginx supervisor inotify-tools make bash && \
	pip install --no-cache-dir Sphinx sphinx-rtd-theme && \
	mkdir -p /run/nginx

ADD supervisord.conf /etc/
ADD nginx.conf /etc/nginx/conf.d/default.conf
ADD autoreload /usr/bin/

WORKDIR /doc

VOLUME ["/doc"]

EXPOSE 80

CMD ["supervisord"]
