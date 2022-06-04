FROM nginx:1.21

ARG ENV_TYPE

COPY docker/nginx/nginx_$ENV_TYPE.template /etc/nginx/conf.d/default.template
COPY docker/nginx/ssl/ /etc/nginx/ssl/
COPY docker/nginx/cors /etc/nginx/cors
COPY docker/nginx/run_nginx.sh /src/run_nginx.sh
