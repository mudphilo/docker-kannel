FROM alpine:3.5

# install dependancies
RUN apk add --no-cache ca-certificates bash subversion \
  libxml2 pcre musl hiredis openssl \
  libxml2-dev pcre-dev build-base libtool musl-dev bison \
  hiredis-dev openssl-dev supervisor

# checkout kannel
RUN svn checkout --trust-server-cert --non-interactive -r 5187 https://svn.kannel.org/gateway/trunk \
  kannel-svn-trunk

WORKDIR /kannel-svn-trunk

# install kannel
RUN ln -sf /usr/include/poll.h /usr/include/sys/poll.h \
  && ln -s /usr/include/unistd.h /usr/include/sys/unistd.h \
  && ln -s /usr/share/libtool/build-aux/ltmain.sh /kannel-svn-trunk/ltmain.sh
RUN ./configure \
  --with-redis --enable-docs=no --enable-start-stop-daemon=no \
  --without-sdb --without-oracle --without-sqlite2
RUN make \
  && make install \
  && cp /kannel-svn-trunk/test/fakesmsc /usr/local/bin/
RUN apk del libxml2-dev pcre-dev build-base libtool musl-dev bison \
  hiredis-dev openssl-dev subversion \
  && rm -rf /kannel-svn-trunk && rm -rf /tmp/*

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
COPY default-kannel.conf /etc/kannel/kannel.conf

EXPOSE 13000
EXPOSE 13001
EXPOSE 12345

VOLUME ["/var/spool/kannel", "/etc/kannel", "/var/log/kannel"]

CMD ["/usr/bin/supervisord","-n","-c","/etc/supervisor/conf.d/supervisord.conf"]