FROM ubuntu:18.04
LABEL maintainer="<fedda@student.21-school.ru>"
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y wget \
        ca-certificates \
        openssh-server \
        postfix \ 
        tzdata && \
    apt-get -y clean
RUN wget https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh && \
    chmod 777 script.deb.sh && ./script.deb.sh && \
    apt-get install -y gitlab-ce && \
    apt-get -y clean
ARG USER_URL=$DOCKER_HOST_IP
EXPOSE 22 80 443
RUN mkdir -p /etc/gitlab/ssl && \
    chmod 755 /etc/gitlab/ssl && \
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/gitlab/ssl/${USER_URL}.key -out /etc/gitlab/ssl/${USER_URL}.crt -subj "/C=RU/ST=Fedda/L=Fedda/O=school21/OU=fedda/CN=${USER_URL}" && \
    chmod 600 /etc/gitlab/ssl/* && \
    sed -i "s/external_url 'http:\/\/gitlab.example.com'/external_url 'https:\/\/${USER_URL}:8443'/" /etc/gitlab/gitlab.rb && \
    sed -i "s/# nginx\['redirect_http_to_https'\] = false/nginx\['redirect_http_to_https'\] = true/" /etc/gitlab/gitlab.rb
RUN mkdir -p /run/sshd
ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &)  && /usr/sbin/sshd &&  gitlab-ctl reconfigure && tail -f /dev/null

