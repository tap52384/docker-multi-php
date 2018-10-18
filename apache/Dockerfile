# Apache configured to work with PHP
ARG APACHE_VERSION=""
FROM httpd:${APACHE_VERSION:+${APACHE_VERSION}-}alpine

LABEL maintainer="Z. Patrick Lewis <tap52384@gmail.com>"

RUN apk update; \
    apk upgrade;

# Copy apache vhost file to proxy php requests to php-fpm container
# TODO: learn exactly how this works
COPY demo.apache.conf /usr/local/apache2/conf/demo.apache.conf
RUN echo "Include /usr/local/apache2/conf/demo.apache.conf" \
    >> /usr/local/apache2/conf/httpd.conf