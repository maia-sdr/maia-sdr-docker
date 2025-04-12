# syntax=docker/dockerfile:1
FROM ubuntu:24.04

ARG OSS_CAD_SUITE_RELEASE=20250412
ARG AMARANTH_RELEASE=0.5.4

ARG DEBIAN_FRONTEND=noninteractive
ENV RUSTUP_HOME=/opt/rust
ARG CARGO_HOME=/opt/rust
SHELL ["/bin/bash", "-c"]

# libtinfo5 is not available in Ubuntu 24.04, so we fetch the package from
# Ubuntu 22.04, which can be installed without dependency issues
# libtinfo5 is required for Vivado

RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y make gcc g++ python3 python3-dev python3-pip \
           python3-pytest python3-numpy python3-scipy iverilog wget curl \
           default-jre libxrender-dev libxtst-dev file \
           libgtk2.0-0 locales git python3-build python3-venv twine \
           zip unzip dfu-util fakeroot u-boot-tools device-tree-compiler mtools \
           bison flex libncurses5-dev libssl-dev bc cpio rsync cmake \
           libgtk-3-0 xz-utils libgmp-dev libmpc-dev lz4 pkg-config \
           libclang-dev xvfb \
    && wget http://launchpadlibrarian.net/666971015/libtinfo5_6.3-2ubuntu0.1_amd64.deb \
    && dpkg -i libtinfo5_6.3-2ubuntu0.1_amd64.deb \
    && rm -f libtinfo5_6.3-2ubuntu0.1_amd64.deb \
    && curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
    && apt-get install -y nodejs \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && pip install --break-system-packages cocotb cocotb-bus \
    && cd /tmp \
    && wget https://github.com/YosysHQ/oss-cad-suite-build/releases/download/${OSS_CAD_SUITE_RELEASE:0:4}-${OSS_CAD_SUITE_RELEASE:4:2}-${OSS_CAD_SUITE_RELEASE:6:2}/oss-cad-suite-linux-x64-${OSS_CAD_SUITE_RELEASE}.tgz \
    && mkdir /opt/oss-cad-suite \
    && tar zxf oss-cad-suite-linux-x64-${OSS_CAD_SUITE_RELEASE}.tgz -C /opt \
    && rm -f oss-cad-suite-linux-x64-${OSS_CAD_SUITE_RELEASE}.tgz \
    && pip install --break-system-packages amaranth==${AMARANTH_RELEASE} \
    && curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --no-modify-path -t armv7-unknown-linux-gnueabihf,wasm32-unknown-unknown \
    && source /opt/rust/env \
    && curl https://rustwasm.github.io/wasm-pack/installer/init.sh -sSf | sh \
    && sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen \
    && dpkg-reconfigure --frontend=noninteractive locales
USER ubuntu
WORKDIR /home/ubuntu
