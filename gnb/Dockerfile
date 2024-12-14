#
# Based on OpenAirInterface gNodeB (gNB) image
#

FROM arawirelesshub/openairinterface5g:oai_gnb as oai_gnb

FROM ubuntu:jammy as ara_gnb

RUN apt update && apt upgrade -y

RUN apt install -y \
    software-properties-common \
    procps \
    libsctp1 \
    libboost-chrono1.74.0 \
    libboost-date-time1.74.0 \
    libboost-filesystem1.74.0 \
    libboost-regex1.74.0 \
    libboost-serialization1.74.0 \
    libboost-thread1.74.0 \
    libboost-system1.74.0 \
    libboost-program-options1.74.0 \
    tzdata \
    libblas3 \
    libconfig9 \
    openssl \
    net-tools \
    iperf \
    iperf3 \
    iproute2 \
    iputils-ping \
    gdb \
    python3 \
    python3-six \
    python3-requests \
    libusb-1.0-0 \
    libyaml-cpp-dev

COPY --from=oai_gnb /usr/lib/x86_64-linux-gnu/libasan.so.8.0.0 /usr/lib/x86_64-linux-gnu/

COPY --from=oai_gnb /opt/oai-gnb /opt/oai-gnb

COPY --from=oai_gnb \
    /usr/local/lib/liboai_eth_transpro.so \
    /usr/local/lib/librfsimulator.so \
    /usr/local/lib/liboai_usrpdevif.so \
    /usr/local/lib/libcoding.so \
    /usr/local/lib/libparams_libconfig.so \
    /usr/local/lib/libdfts.so \
    /usr/local/lib/libldpc*.so \
    /usr/local/lib/libtelnetsrv.so \
    /usr/local/lib/libtelnetsrv_ci.so \
    /usr/local/lib/libparams_yaml.so \
    /usr/local/lib/

COPY --from=oai_gnb /usr/local/bin/uhd_find_devices /usr/local/bin
COPY --from=oai_gnb /usr/local/lib/libuhd.so.4.4.0 /usr/local/lib
COPY --from=oai_gnb /usr/local/lib/flexric /usr/local/lib/flexric
COPY --from=oai_gnb /tini /tini

RUN /bin/bash -c "ln -s /usr/local/lib/liboai_usrpdevif.so /usr/local/lib/liboai_device.so" && \
    ldconfig && \
    echo "---- ldd on nr-softmodem ----" && \
    ldd /opt/oai-gnb/bin/nr-softmodem && \
    echo "---- ldd on shared libraries ----" && \
    ldd /usr/local/lib/liboai_eth_transpro.so \
        /usr/local/lib/librfsimulator.so \
        /usr/local/lib/liboai_usrpdevif.so \
        /usr/local/lib/libcoding.so \
        /usr/local/lib/libparams_libconfig.so \
        /usr/local/lib/libdfts.so \
        /usr/local/lib/libldpc*.so \
        /usr/local/lib/libtelnetsrv.so \
        /usr/local/lib/libtelnetsrv_ci.so \
        /usr/local/lib/libuhd.so.4.4.0

WORKDIR /opt/oad-gnb

ENTRYPOINT [ "/tini", "-v", "--", "/opt/oai-gnb/bin/entrypoint.sh" ]
CMD [ "/opt/oai-gnb/bin/nr-softmodem" ]
