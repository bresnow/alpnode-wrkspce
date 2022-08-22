
ARG NODE_VERSION=16
FROM mhart/alpine-node:${NODE_VERSION}
ENV TTYD_PORT=8026
ENV DIRECTORY=main
# lazygit version
ENV LGV=0.35
# filebrowser version
ENV FBV=2.22.4
# web terminal version
ENV TV=1.7.1

COPY unified-supervisord.conf supervisord.conf /etc/supervisord/
COPY zsh-in-docker.sh /tmp/zsh-in-docker.sh
COPY filebrowser.json /opt/filebrowser/database.json
COPY database.db /opt/filebrowser/database.db
COPY branding /branding
VOLUME [ "/branding","/opt/filebrowser" ]

RUN apk upgrade --no-cache -U && \
    apk add --no-cache curl zip gzip tar py-pip python3 supervisor openrc vim git-flow git mc grep dmidecode htop 
RUN  echo "------------------------------------------------------ GIT" \
    && wget -P /tmp https://github.com/jesseduffield/lazygit/releases/download/v${LGV}/lazygit_${LGV}_Linux_x86_64.tar.gz \
    && mkdir /tmp/lazygit  && tar -xzf /tmp/lazygit_${LGV}_Linux_x86_64.tar.gz --directory /tmp/lazygit \
    && chmod +x /tmp/lazygit/lazygit \
    && mv /tmp/lazygit/lazygit /usr/bin/ \
    && echo "------------------------------------------------------ Supervisor" \
    && pip3 install supervisor==4.2.2 \
    && chmod -R 777 /etc/supervisord/ \
    && mkdir -p /var/log/supervisord/ \
    && echo "------------------------------------------------------ File browsers: Xplr" \
    && wget -P /tmp https://github.com/sayanarijit/xplr/releases/download/v0.17.6/xplr-linux.tar.gz \
    && mkdir /tmp/xplr  && tar -xzf /tmp/xplr-linux.tar.gz --directory /tmp/xplr \
    && chmod +x /tmp/xplr/xplr \
    && mv /tmp/xplr/xplr /usr/bin/ \
    && echo "------------------------------------------------------ Filebrowser" \
    && cd /tmp && wget https://github.com/filebrowser/filebrowser/releases/download/v${FBV}/linux-amd64-filebrowser.tar.gz \
    && mkdir -p /tmp/filebrowser \
    && tar xvf /tmp/linux-amd64-filebrowser.tar.gz -C /tmp/filebrowser \
    && mv /tmp/filebrowser/filebrowser /opt/filebrowser \
    && chmod +x /opt/filebrowser/filebrowser \
    && mkdir -p /var/log/filebrowser/ \
    && mkdir -p /home/store \
    && echo "------------------------------------------------------ Web-based terminal" \
    && cd /tmp && wget https://github.com/tsl0922/ttyd/releases/download/${TV}/ttyd.x86_64 \
    && mv ttyd.x86_64 /usr/bin/ttyd \
    && chmod +x /usr/bin/ttyd \
    && echo "------------------------------------------------------ ZSH" \
    && chmod +x /tmp/zsh-in-docker.sh \
    && mkdir -p /home/${DIRECTORY} \
    && HOME=/home/${DIRECTORY} \
    && /tmp/zsh-in-docker.sh \
    -t https://github.com/pascaldevink/spaceship-zsh-theme \
    -a 'DISABLE_UPDATE_PROMPT="true"' \
    -a 'SPACESHIP_PROMPT_ADD_NEWLINE="false"' \
    -a 'SPACESHIP_PROMPT_SEPARATE_LINE="false"' \
    -a 'export LS_COLORS="$LS_COLORS:ow=1;34:tw=1;34:"' \
    -a 'SPACESHIP_USER_SHOW="false"' \
    -a 'SPACESHIP_TIME_SHOW="false"' \
    -a 'SPACESHIP_DIRECTORY_COLOR="cyan"' \
    -a 'SPACESHIP_GIT_SYMBOL="【 ɢɪᴛ 】"' \
    -a 'SPACESHIP_BATTERY_SHOW="false"' \
    -a 'if [[ $(pwd) != /home/${DIRECTORY}  ]]; then cd /home/${DIRECTORY}; fi  # Set starting dir to /home/${DIRECTORY} ' \
    -a 'hash -d p=/home/${DIRECTORY}' \
    -p git \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-completions \
    -p https://github.com/zsh-users/zsh-history-substring-search \
    -p https://github.com/zsh-users/zsh-syntax-highlighting \
    -p 'history-substring-search' \
    -p https://github.com/bobthecow/git-flow-completion \
    -a 'bindkey "\$terminfo[kcuu1]" history-substring-search-up' \
    -a 'bindkey "\$terminfo[kcud1]" history-substring-search-down' \
    && rm /tmp/zsh-in-docker.sh 

ENV HOME=/home/${DIRECTORY}
ENV PATH="${HOME}/.local/bin:/opt/filebrowser:${PATH}"

RUN rm -rf /var/cache/apk/*  /sbin/halt /sbin/poweroff /sbin/reboot &&  \
    { rm -rf /root/.gnupg || true; }

ENTRYPOINT supervisord -c "/etc/supervisord/unified-supervisord.conf"