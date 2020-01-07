ARG BASE_IMAGE=centos:8

# hadolint ignore=DL3006
FROM $BASE_IMAGE

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
 && dnf -y install \
        langpacks-zh_CN \
        glibc-langpack-zh \
 && sed --in-place "s|^LANG\s*=\s*\".*\"$|LANG=\"${LOCALE_LANGUAGE}_${LOCALE_COUNTRY}.${LOCALE_CHARMAP}\"|" /etc/locale.conf \
# 安装翻译
 && dnf -y reinstall \
        binutils \
        bash \
        coreutils \
        dnf \
        findutils \
        grep \
        sed \
        shadow-utils \
        tar \
        xz \
# 升级重要的或者有安全漏洞的软件包
 && dnf -y update \
        bash \
# 安装常用工具
 && dnf -y install --setopt='tsflags=nodocs' \
        epel-release \
 && dnf -y install --setopt='tsflags=nodocs' \
        bash-completion \
        curl \
        gettext \
        htop \
        less \
        nano \
        openssh-clients \
        sudo \
        telnet \
        unzip \
        vim \
        wget \
 && dnf -y clean all \
 && rm -rf /var/cache/dnf