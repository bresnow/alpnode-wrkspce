ARG docker_registry=docker.io/bresnow
ARG image_tag=base

FROM ${docker_registry}/alpnode-wrkspce:${image_tag}

COPY supervisord-codeserver.conf /etc/supervisord/
COPY code-server-run.sh /opt/codeserver/code-server-run.sh 

RUN apk  --no-cache add alpine-sdk bash libstdc++ libc6-compat libx11-dev libxkbfile-dev libsecret-dev \
    && mkdir -p /opt/codeserver \
    && cd /opt/codeserver && yarn global add code-server \
    && mkdir -p /home/project \
    && mkdir /opt/codeserver/data \
    && mkdir -p mkdir /opt/vscode/extensions \ 
    && mkdir -p /var/log/codeserver \
    && chmod +x /opt/codeserver/code-server-run.sh 

RUN npm config set init.author.name "Bresnow" \
    && npm config set init.author.email "bresnow@me.com" \
    && npm config set init.author.url "https://fltngmmth.com" \
    && npm config set init.license "UNLICENSED" \
    && git config --global user.name "Bresnow" \
    && git config --global user.email "bresnow@me.com" 