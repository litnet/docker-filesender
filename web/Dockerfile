FROM nginx:1.12.0-alpine

ENV FILESENDER_V=1.6.1 SSP_V=1.14.12

RUN apk add --no-cache curl mariadb-client openssl
    
RUN mkdir opt && cd /opt && \
    curl -kL https://downloads.filesender.org/filesender-${FILESENDER_V}.tar.gz | tar xz && \
    curl -L https://github.com/simplesamlphp/simplesamlphp/releases/download/v${SSP_V}/simplesamlphp-${SSP_V}.tar.gz | tar xz

RUN apk del curl && \
    rm -r /etc/nginx/conf.d/default.conf 


ADD docker-entrypoint.sh /usr/local/bin/entrypoint.sh
ADD conf/ /opt/conf.d

RUN mkdir /opt/conf.d/cert && \
    cd /opt/conf.d/cert && \
    openssl req -subj '/CN=Test/O=Test/C=LT' -newkey rsa:2048 -new -x509 -days 3652 -nodes -out saml.crt -keyout saml.pem

VOLUME ["/opt/filesender", "/opt/simplesamlphp"]

ENTRYPOINT ["entrypoint.sh"]

EXPOSE 80 443
