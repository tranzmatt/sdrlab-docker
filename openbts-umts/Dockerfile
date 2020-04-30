FROM uhd
MAINTAINER Ralf-Philipp Weinmann <ralf@comsecuris.com>
RUN apt-get update
WORKDIR /home/user/src

# install OpenBTS deps
RUN apt-get install -y libortp-dev libsqlite3-dev libreadline-dev libzmq3-dev libosip2-dev psmisc sqlite3 libtool-bin

RUN git clone https://github.com/RangeNetworks/liba53.git
RUN cd liba53 \
    && cp /usr/share/misc/config.guess . \
    && cp /usr/share/misc/config.sub . \
    && perl -i -pe 's|/usr/|/usr/local/|g' Makefile \
    && make install
RUN git clone https://github.com/RangeNetworks/OpenBTS-UMTS.git
RUN tar -xzf OpenBTS-UMTS/asn1c-0.9.23.tar.gz \
    && mv vlm-asn1c-0959ffb asn1c-0.9.23 \
    && cd asn1c-0.9.23 \
    && cp /usr/share/misc/config.guess . \
    && cp /usr/share/misc/config.sub . \    
    && ./configure \
    && make install
RUN cd OpenBTS-UMTS/ \
    && git submodule init \
    && git submodule update \
    && cp /usr/share/misc/config.guess . \
    && cp /usr/share/misc/config.sub . \    
    && sh autogen.sh \
    && ./configure --with-uhd \
    && make -j8 install
COPY nocoredump.patch /tmp/nocoredump.patch
RUN git clone https://github.com/RangeNetworks/smqueue
RUN cd smqueue \
    && git submodule init \
    && git submodule update \
    && cd CommonLibs && patch -p1 -i /tmp/nocoredump.patch && cd .. \
    && cp /usr/share/misc/config.guess . \
    && cp /usr/share/misc/config.sub . \   
    && sh autogen.sh \
    && ./configure \
    && make -j8 install
RUN git clone https://github.com/RangeNetworks/subscriberRegistry.git
RUN cd subscriberRegistry \
    && git submodule init \
    && git submodule update \
    && cd CommonLibs && patch -p1 -i /tmp/nocoredump.patch && cd .. \
    && cp /usr/share/misc/config.guess . \
    && cp /usr/share/misc/config.sub . \    
    && sh autogen.sh \
    && ./configure \
    && make -j8 install
WORKDIR /OpenBTS
