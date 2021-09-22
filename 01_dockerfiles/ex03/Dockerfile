FROM ubuntu:18.04


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


ARG USER_URL

EXPOSE 22 80 8443

RUN mkdir -p /etc/gitlab/ssl && \
    chmod 755 /etc/gitlab/ssl && \
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/gitlab/ssl/${USER_URL}.key -out /etc/gitlab/ssl/${USER_URL}.crt -subj "/C=RU/ST=Nmustach/L=Nmustach/O=school21/OU=nmustach/CN=${USER_URL}" && \
    chmod 600 /etc/gitlab/ssl/* && \
    sed -i "s/external_url 'http:\/\/gitlab.example.com'/external_url 'https:\/\/${USER_URL}:8443'/" /etc/gitlab/gitlab.rb && \
    sed -i "s/# nginx\['redirect_http_to_https'\] = false/nginx\['redirect_http_to_https'\] = true/" /etc/gitlab/gitlab.rb && \
    sed -i "s/# grafana\['enable'\] = true/grafana['enable'] = false/g" /etc/gitlab/gitlab.rb && \
    sed -i "s/# prometheus\['enable'\] = true/prometheus['enable'] = false/g" /etc/gitlab/gitlab.rb


RUN mkdir -p /run/sshd

ENV GITLAB_ROOT_PASSWORD='ljVSTigHLFnHTcR6tIbIyySjzv0Z6pUSstLQrf'

ENTRYPOINT (/opt/gitlab/embedded/bin/runsvdir-start &)  && /usr/sbin/sshd &&  gitlab-ctl reconfigure && tail -f /dev/null

##docker build --build-arg USER_URL=localhost --tag gitlab .
##docker run -it -p 8443:8443 -p 8080:80 -p 22:22 --privileged gitlab
