FROM alpine:latest

LABEL maintainer "Tuan Anh Tran <me@tuananh.org>"

# install dependencies
RUN apk add --update g++ libtool make automake curl autoconf supervisor

# copy files and create necessary folders
RUN mkdir -p /var/log/supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# compile twemproxy
RUN curl -qL https://github.com/twitter/twemproxy/archive/v0.4.1.tar.gz | tar xzf -
RUN cd  twemproxy-0.4.1 && autoreconf -fvi && ./configure --enable-debug=log && make && make install

RUN apk del g++ libtool make automake curl autoconf

EXPOSE 22121

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]