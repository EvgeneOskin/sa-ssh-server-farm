FROM ubuntu:18.04

RUN apt-get update -y && apt-get upgrade -y && \
    apt-get install -y openssh-server

RUN \
    apt-get install -y gnupg2 pcscd scdaemon curl \
    && gpg2 --list-keys
RUN echo '\
enable-ssh-support \n\
pinentry-program /usr/bin/pinentry-curses \n\
default-cache-ttl 60 \n\
max-cache-ttl 120'\
    > ~/.gnupg/gpg-agent.conf \
    && echo '\
auto-key-locate keyserver \n\
keyserver hkps://hkps.pool.sks-keyservers.net \n\
keyserver-options no-honor-keyserver-url \n\
keyserver-options ca-cert-file=/etc/sks-keyservers.netCA.pem \n\
personal-cipher-preferences AES256 AES192 AES CAST5 \n\
personal-digest-preferences SHA512 SHA384 SHA256 SHA224 \n\
default-preference-list SHA512 SHA384 SHA256 SHA224 AES256 AES192 AES CAST5 ZLIB BZIP2 ZIP Uncompressed \n\
cert-digest-algo SHA512 \n\
s2k-digest-algo SHA512 \n\
s2k-cipher-algo AES256 \n\
charset utf-8 \n\
fixed-list-mode \n\
no-comments \n\
no-emit-version \n\
keyid-format 0xlong \n\
list-options show-uid-validity \n\
verify-options show-uid-validity \n\
with-fingerprint \n\
require-cross-certification '\
    > ~/.gnupg/gpg.conf

RUN curl -s "https://sks-keyservers.net/sks-keyservers.netCA.pem" \
    -o /etc/sks-keyservers.netCA.pem

ADD entrypoint.sh /usr/bin/entrypoint.sh
ADD sshd_config /etc/ssh/sshd_config
ADD id_rsa.pub /tmp/
RUN chmod +x /usr/bin/entrypoint.sh
CMD /usr/bin/entrypoint.sh
