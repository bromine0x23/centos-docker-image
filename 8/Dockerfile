ARG BASE_IMAGE=centos:8

# hadolint ignore=DL3006
FROM $BASE_IMAGE

ARG IS_SLIM=

ARG NONROOT_GROUP=nonroot
ARG NONROOT_USER=nonroot
ARG NONROOT_GID=10010
ARG NONROOT_UID=10010
ARG NONROOT_HOME=/opt/nonroot

ENV NONROOT_HOME ${NONROOT_HOME}

ARG LOCALE_LANGUAGE=zh
ARG LOCALE_COUNTRY=CN
ARG LOCALE_CHARMAP=UTF-8
ARG LOCALE_TIMEZONE=Asia/Shanghai

ENV TZ   ${LOCALE_TIMEZONE}
ENV LANG ${LOCALE_LANGUAGE}_${LOCALE_COUNTRY}.${LOCALE_CHARMAP}

RUN set -ex \
# 设置时区
 && ln -sf /usr/share/zoneinfo/${LOCALE_TIMEZONE} /etc/localtime \
# 设置语言环境
 && sed --in-place "s|^\(%_install_langs\s*.*\)$|\1:${LOCALE_LANGUAGE}_${LOCALE_COUNTRY}|" /etc/rpm/macros.image-language-conf \
 && dnf -y install --nodocs --setopt='tsflags=nodocs' --setopt='install_weak_deps=False' \
        langpacks-zh_CN \
        glibc-langpack-zh \
 && sed --in-place "s|^LANG\s*=\s*\".*\"$|LANG=\"${LOCALE_LANGUAGE}_${LOCALE_COUNTRY}.${LOCALE_CHARMAP}\"|" /etc/locale.conf \
# 安装翻译
 && if [ "${IS_SLIM}" == '' ]; then \
       dnf -y reinstall --nodocs --setopt='tsflags=nodocs' \
           bash \
           binutils \
           coreutils \
           dnf \
           findutils \
           grep \
           sed \
           shadow-utils \
           tar \
           xz \
       ; \
    fi \
# 升级重要的或者有安全漏洞的软件包
 && dnf -y update \
        dnf \
# 安装常用工具
 && if [ "${IS_SLIM}" == '' ]; then \
       dnf -y install --nodocs --setopt='tsflags=nodocs' --setopt='install_weak_deps=False' \
           epel-release \
       ; \
       dnf -y install --nodocs --setopt='tsflags=nodocs' --setopt='install_weak_deps=False' \
           bash-completion \
           bzip2 \
           bind-utils \
           curl \
           dnf-plugins-core \
           gettext \
           htop \
           less \
           nano \
           openssh-clients \
           sudo \
           tzdata \
           unzip \
           vim \
           wget \
           zlib \
       ; \
    fi \
 && dnf -y clean all \
 && rm -rf /var/cache/dnf \
 && groupadd --gid ${NONROOT_GID}                                                                                    ${NONROOT_GROUP} \
 && useradd  --gid ${NONROOT_GID} --uid ${NONROOT_UID} --create-home --home-dir ${NONROOT_HOME} --comment "Non-root" ${NONROOT_USER}