ARG BASE_IMAGE=centos:7

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
 && sed --in-place "s|^\(override_install_langs\s*=\s*.*\)$|\1:${LOCALE_LANGUAGE}_${LOCALE_COUNTRY}|" /etc/yum.conf \
 && yum -y reinstall \
        glibc-common \
 && sed --in-place "s|^LANG\s*=\s*\".*\"$|LANG=\"${LOCALE_LANGUAGE}_${LOCALE_COUNTRY}.${LOCALE_CHARMAP}\"|" /etc/locale.conf \
# 升级重要或者有安全漏洞的软件包
 && yum -y update \
        yum \
 && yum -y clean all \
 && rm -rf /var/cache/yum