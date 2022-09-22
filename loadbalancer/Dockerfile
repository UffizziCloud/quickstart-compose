FROM nginx:1

# add curl for troubleshooting
# add wait-for-it to delay start
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    curl \
    wait-for-it \
    && rm -rf /var/lib/apt/lists/*

COPY ./vote.conf.template /etc/nginx/templates/vote.conf.template

COPY ./19-wait-for-backing-services.sh /docker-entrypoint.d/19-wait-for-backing-services.sh

ENV PORT=8080 \
    VOTE_HOST=localhost \
    VOTE_PORT=8088 \
    RESULT_HOST=localhost \
    RESULT_PORT=8888
