FROM alpine:3.10

LABEL description "PostfixAdmin is a web based interface used to manage mailboxes" \
      maintainer="Hardware <contact@meshup.net>"

ARG VERSION=3.3.10

# https://pgp.mit.edu/pks/lookup?search=0xC6A682EA63C82F1C&fingerprint=on&op=index
# pub  4096R/63C82F1C 2005-10-06 Christian Boltz (www.cboltz.de) <gpg@cboltz.de>
ARG GPG_SHORTID="0xC6A682EA63C82F1C"
ARG GPG_FINGERPRINT="70CA A060 DE04 2AAE B1B1  5196 C6A6 82EA 63C8 2F1C"
ARG SHA256_HASH="866d4c0ca870b2cac184e5837a4d201af8fcefecef09bc2c887a6e017a00cefe"

RUN echo "@community https://nl.alpinelinux.org/alpine/v3.10/community" >> /etc/apk/repositories \
 && apk -U upgrade \
 && apk add -t build-dependencies \
    ca-certificates \
    gnupg \
 && apk add \
    su-exec \
    dovecot \
    tini@community \
    php7@community \
    php7-phar \
    php7-fpm@community \
    php7-imap@community \
    php7-pgsql@community \
    php7-mysqli@community \
    php7-session@community \
    php7-mbstring@community \
 && cd /tmp \
 && PFA_TARBALL="postfixadmin-${VERSION}.tar.gz" \
 && wget -q https://github.com/postfixadmin/postfixadmin/archive/refs/tags/postfixadmin-${VERSION}.tar.gz\
 && mkdir /postfixadmin && tar xzf ${PFA_TARBALL} -C /postfixadmin && mv /postfixadmin/postfixadmin-postfixadmin-$VERSION/* /postfixadmin \
 && apk del build-dependencies \
 && rm -rf /var/cache/apk/* /tmp/* /root/.gnupg /postfixadmin/postfixadmin-$VERSION*

COPY bin /usr/local/bin
RUN chmod 755 /usr/local/bin/*
EXPOSE 8888
CMD ["tini", "--", "run.sh"]
