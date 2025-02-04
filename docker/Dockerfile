FROM node:lts-alpine
LABEL maintainer="blacktitty-y"
ARG QL_URL=https://github.com/blacktitty-y/ql-2.2.git
ARG QL_BRANCH=master
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
    LANG=zh_CN.UTF-8 \
    SHELL=/bin/bash \
    PS1="\u@\h:\w \$ " \
    QL_DIR=/ql
WORKDIR ${QL_DIR}
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories \
    && apk update -f \
    && apk upgrade \
    && apk --no-cache add -f bash \
                             coreutils \
                             moreutils \
                             git \
                             curl \
                             wget \
                             tzdata \
                             perl \
                             openssl \
                             nginx \
                             python3 \
                             jq \
                             openssh \
    && rm -rf /var/cache/apk/* \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone \
    && touch ~/.bashrc \
    && mkdir /run/nginx \
    && git clone -b ${QL_BRANCH} https://github.com/blacktitty-y/ql-static-2.2.git /static \
    && git clone -b ${QL_BRANCH} ${QL_URL} ${QL_DIR} \
    && git config --global user.email "52312781+blacktitty-y@users.noreply.github.com" \
    && git config --global user.name "Blacktitty" \
    && git config --global pull.rebase true \
    && cd ${QL_DIR} \
    && cp -rf /static/* ./ \
    && rm -rf /static \
    && cp -f .env.example .env \
    && chmod 777 ${QL_DIR}/shell/*.sh \
    && chmod 777 ${QL_DIR}/docker/*.sh \
    && npm install -g pm2 \
    && npm install -g pnpm \
    && rm -rf /root/.npm \
    && pnpm install --prod \
    && rm -rf /root/.pnpm-store
ENTRYPOINT ["./docker/docker-entrypoint.sh"]
