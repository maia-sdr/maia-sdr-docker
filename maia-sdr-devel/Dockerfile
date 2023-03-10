# syntax=docker/dockerfile:1
FROM ubuntu:22.04

ARG DEBIAN_FRONTEND=noninteractive
ADD vivado2019_1_gcc-arm-linux-gnueabi.tar.xz /opt/gcc-arm-linux-gnueabi
ENV RUSTUP_HOME=/opt/rust
ARG CARGO_HOME=/opt/rust
SHELL ["/bin/bash", "-c"]
RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y make gcc g++ python3 python3-dev python3-pip \
           python3-pytest python3-numpy python3-scipy iverilog wget curl \
           default-jre libxrender-dev libxtst-dev libtinfo5 \
           libgtk2.0-0 locales git \
           zip unzip dfu-util fakeroot u-boot-tools device-tree-compiler mtools \
           bison flex libncurses5-dev libssl-dev bc cpio rsync cmake \
           libgtk-3-0 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && pip install -U pip \
    && pip install cocotb cocotb-bus \
    && cd /tmp \
    && wget https://github.com/YosysHQ/oss-cad-suite-build/releases/download/2023-03-12/oss-cad-suite-linux-x64-20230312.tgz \
    && mkdir /opt/oss-cad-suite \
    && tar zxf oss-cad-suite-linux-x64-20230312.tgz -C /opt \
    && rm -f oss-cad-suite-linux-x64-20230312.tgz \
    && git clone https://github.com/amaranth-lang/amaranth \
    && cd amaranth \
    && pip install . \
    && cd .. \
    && rm -rf amaranth \
    && curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --no-modify-path -t armv7-unknown-linux-gnueabihf,wasm32-unknown-unknown \
    && source /opt/rust/env \
    && curl https://rustwasm.github.io/wasm-pack/installer/init.sh -sSf | sh \
    && sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
    && dpkg-reconfigure --frontend=noninteractive locales \
    && useradd -ms /bin/bash user
USER user
WORKDIR /home/user
