FROM nginx:1.18

RUN touch /var/run/nginx.pid
RUN mkdir /sock

RUN apt-get update && apt-get install -y openssl curl && apt-get install -y vim
RUN mkdir /etc/nginx/certs \
  && echo -e "\n\n\n\n\n\n\n" | openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/certs/nginx.key -out /etc/nginx/certs/nginx.crt

ARG TARGETARCH

RUN cd /usr/local/bin/ \
  && curl -L https://github.com/FiloSottile/mkcert/releases/download/v1.4.3/mkcert-v1.4.3-linux-$TARGETARCH -o mkcert \
  && chmod +x mkcert


RUN mkdir -p /etc/nginx/html /var/www/html \
  && chown -R www-data:www-data /etc/nginx /var/www /var/cache/nginx /var/log/nginx /var/run/nginx.pid /sock \
  && chmod -R u+X /var/log/nginx

EXPOSE 80

USER www-data:www-data

VOLUME /var/www

WORKDIR /var/www/html/server
