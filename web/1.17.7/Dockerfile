FROM nginx:1.17.7

ARG APP_ENV
ARG SET_REAL_IP_FROM
ARG SERVER_NAME
ENV APP_ENV=$APP_ENV
ENV SET_REAL_IP_FROM=$SET_REAL_IP_FROM
ENV SERVER_NAME=$SERVER_NAME

RUN apt-get update \
  && apt-get install -y vim curl procps less \
  && apt-get clean \
  && rm -Rf /var/lib/apt/lists/*

ENV LANG C.UTF-8
WORKDIR /var/www/html

RUN rm /etc/nginx/conf.d/default.conf
COPY .vimrc /root/.vimrc
COPY .bashrc /root/.bashrc
COPY nginx.conf /etc/nginx/nginx.conf
COPY conf.d/default.conf /etc/nginx/conf.d/

RUN if [ "$APP_ENV" != 'local' ]; then \
      sed -i "s%127.0.0.1%$SET_REAL_IP_FROM%" /etc/nginx/nginx.conf; \
  fi

RUN if [ -n "$SERVER_NAME" ]; then \
      sed -i "s%localhost%$SERVER_NAME%" /etc/nginx/conf.d/${APP_ENV}.conf; \
  fi

COPY docker-entrypoint.sh /bin/docker-entrypoint.sh
ENTRYPOINT ["/bin/docker-entrypoint.sh"]
