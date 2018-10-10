FROM nginx:1.15.0-alpine
LABEL MAINTAINER='Maksim Kostromin maksim.kostromin@trololo.com'
ARG PORT_ARG='80'
ARG HOST_ARG='127.0.0.1'
ENV PORT=${PORT_ARG} \
    HOST=${HOST_ARG}
RUN apk add --update --upgrade --no-cache libintl curl \
 && apk add --no-cache --virtual build_deps gettext \
 && cp /usr/bin/envsubst /usr/local/bin/envsubst \
 && echo '#!/bin/sh' >> /usr/local/bin/entrypoint.sh \
 && echo 'PORT=${PORT:=80}' >> /usr/local/bin/entrypoint.sh \
 && echo 'HOST=${HOST:=localhost}' >> /usr/local/bin/entrypoint.sh \
 && echo '#/bin/sh -c "envsubst < /etc/nginx/default.conf.tpl > /etc/nginx/conf.d/default.conf && exec nginx -g \"daemon off;\""' >> /usr/local/bin/entrypoint.sh \
 && echo '/bin/sh -c "envsubst < /etc/nginx/default.conf.tpl > /etc/nginx/conf.d/default.conf && ( cat /etc/nginx/conf.d/default.conf | grep listen ) && ( cat /etc/nginx/conf.d/default.conf | grep server_name ) && exec nginx -g \"daemon off;\""' >> /usr/local/bin/entrypoint.sh \
 && chmod +x /usr/local/bin/entrypoint.sh \
 && apk del build_deps curl
ENTRYPOINT entrypoint.sh
CMD /bin/bash
COPY ./docker/default.conf.tpl /etc/nginx/default.conf.tpl
COPY ./target/generated-docs   /usr/share/nginx/html
