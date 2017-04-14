FROM ubuntu:16.04

MAINTAINER Tuan Anh Tran "me@tuananh.org"

# install dependencies
RUN apt-get update
RUN apt-get install -y libtool make automake curl supervisor

# copy files and create necessary folders
RUN mkdir -p /var/log/supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# compile twemproxy
RUN curl -qL https://github.com/twitter/twemproxy/archive/v0.4.1.tar.gz | tar xzf -
RUN cd  twemproxy-0.4.1 && autoreconf -fvi && ./configure --enable-debug=log && make && make install

RUN apt-get remove make automake curl

EXPOSE 22121

CMD ["/usr/bin/supervisord"]