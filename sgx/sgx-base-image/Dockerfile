FROM ubuntu:18.04

MAINTAINER "pengfei zhao"

RUN apt-get update && apt-get install  -y \
    build-essential \
    ocaml \
    ocamlbuild \
    automake \
    autoconf \
    libtool \
    wget \
    python \
    libssl-dev \
    git \
    cmake \
    perl \
    libcurl4-openssl-dev \ 
    protobuf-compiler \
    libprotobuf-dev \
    debhelper \
    reprepro \
    libcurl4 \
    libprotobuf10 \
    libssl1.1 \
    make \
    module-init-tools

WORKDIR /linux-sgx
COPY . .

RUN ./download_prebuilt.sh

RUN make sdk_install_pkg_no_mitigation

WORKDIR /opt/intel
RUN sh -c 'echo yes | /linux-sgx/linux/installer/bin/sgx_linux_x64_sdk_*.bin'

WORKDIR /linux-sgx
RUN make psw_install_pkg

WORKDIR /linux-sgx/linux/installer/bin
RUN ./sgx_linux_x64_psw*.bin --no-start-aesm

ENV LD_LIBRARY_PATH=/opt/intel/sgxpsw/aesm

WORKDIR /home
COPY docker-entrypoint.sh /home/docker-entrypoint.sh
RUN chmod +x docker-entrypoint.sh

WORKDIR /
RUN rm -rf linux-sgx

ENTRYPOINT ["sh", "/home/docker-entrypoint.sh"]

