#Todo con Docker
#@utor Facundo Rauber <dis.jacku@gmail.com>
#
FROM ubuntu

MAINTAINER FacundoRauber "dis.jacku@gmail.com"

ENV NGINX_VERSION 1.18.0

RUN apt-get update && apt-get install -y ca-certificates build-essential wget libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev


RUN wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz \
  && tar -xzvf nginx-${NGINX_VERSION}.tar.gz

RUN cd nginx-${NGINX_VERSION} \
  && ./configure \
    --prefix=/usr/local/nginx \
    --sbin-path=/usr/sbin/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --pid-path=/var/run/nginx.pid \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
#    --with-http_ssl_module \
    --with-http_v2_module \
#    --with-openssl=/usr \
    --with-http_realip_module \
    --with-http_stub_status_module \
    --with-threads \
    --with-ipv6 \
  && make \
  && make install

RUN apt-get autoremove -y

RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

VOLUME ["/var/cache/nginx"]

RUN rm -v /etc/nginx/nginx.conf

ADD nginx.conf /etc/nginx/
ADD tuning.conf /etc/nginx/

RUN echo "daemon off;" >> /etc/nginx/nginx.conf


EXPOSE 80

CMD nginx
