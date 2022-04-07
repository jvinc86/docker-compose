FROM rockylinux:8.5
MAINTAINER idnomic
# do not use ENV since it stays in the container
# ARG is only available at build time
ARG USER_ID=${USER_ID:-1000}
ARG USER_NAME=jenkins
ARG PROXY_HOST=${PROXY_HOST:-proxy.dev.idnomic.com}
ARG PROXY_PORT=${PROXY_PORT:-3128}

COPY Rocky-BaseOS.repo /etc/yum.repos.d/
COPY Rocky-AppStream.repo /etc/yum.repos.d/
COPY Rocky-Extras.repo /etc/yum.repos.d/
COPY repo.dev.repo /etc/yum.repos.d/
COPY local.repo /etc/yum.repos.d/
RUN true \
  && echo "proxy=http://$PROXY_HOST:$PROXY_PORT" >> /etc/yum.conf \
  && echo "export http_proxy=http://$PROXY_HOST:$PROXY_PORT" >> /etc/profile.d/proxy.sh \
  && echo "export https_proxy=http://$PROXY_HOST:$PROXY_PORT" >> /etc/profile.d/proxy.sh \
  && /bin/rm -f /etc/yum.repos.d/Rocky-Sources.repo \
  && yum clean all \
  && yum --disablerepo=local -y install createrepo \
  && mkdir -pv /rpms/el8 \
  && createrepo /rpms/el8 \
  && true