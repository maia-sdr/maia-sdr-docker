# syntax=docker/dockerfile:1
FROM ubuntu:22.04

ARG OSS_CAD_SUITE_RELEASE=20231119
ARG AMARANTH_HASH=f9da3c0d166dd2be189945dca5a94e781e74afeb

ARG DEBIAN_FRONTEND=noninteractive
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
           libgtk-3-0 xz-utils libgmp-dev libmpc-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && pip install -U pip \
    && pip install cocotb cocotb-bus \
    && cd /tmp \
    && wget https://github.com/YosysHQ/oss-cad-suite-build/releases/download/${OSS_CAD_SUITE_RELEASE:0:4}-${OSS_CAD_SUITE_RELEASE:4:2}-${OSS_CAD_SUITE_RELEASE:6:2}/oss-cad-suite-linux-x64-${OSS_CAD_SUITE_RELEASE}.tgz \
    && mkdir /opt/oss-cad-suite \
    && tar zxf oss-cad-suite-linux-x64-${OSS_CAD_SUITE_RELEASE}.tgz -C /opt \
    && rm -f oss-cad-suite-linux-x64-${OSS_CAD_SUITE_RELEASE}.tgz \
    && git clone https://github.com/amaranth-lang/amaranth \
    && cd amaranth \
    && git checkout $AMARANTH_HASH \
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
