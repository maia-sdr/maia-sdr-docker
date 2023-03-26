FROM ghcr.io/cross-rs/armv7-unknown-linux-gnueabihf:main

RUN apt-get update && \
    DEBIAN_FRONTENT=noninteractive apt-get -y install git wget cpio unzip rsync bc patch locales && \
    sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    dpkg-reconfigure --frontend=noninteractive locales && \
    git clone https://github.com/maia-sdr/plutosdr-fw && \
    cd plutosdr-fw && \
    git submodule init buildroot && \
    git submodule update buildroot && \
    cd buildroot && \
    make zynq_pluto_defconfig && \
    make toolchain && \
    mkdir /opt/buildroot-toolchain && \
    cp -a output/host/* /opt/buildroot-toolchain && \
    cd / && \
    rm -rf /plutosdr-fw

ENV PATH="/opt/buildroot-toolchain/bin:${PATH}"
ENV CROSS_TOOLCHAIN_PREFIX=arm-buildroot-linux-uclibcgnueabihf-
ENV CROSS_SYSROOT=/opt/buildroot-toolchain/arm-buildroot-linux-uclibcgnueabihf/sysroot
ENV CARGO_TARGET_ARMV7_UNKNOWN_LINUX_GNUEABIHF_LINKER="$CROSS_TOOLCHAIN_PREFIX"gcc \
    CARGO_TARGET_ARMV7_UNKNOWN_LINUX_GNUEABIHF_RUNNER="$CROSS_TARGET_RUNNER" \
    AR_armv7_unknown_linux_gnueabihf="$CROSS_TOOLCHAIN_PREFIX"ar \
    CC_armv7_unknown_linux_gnueabihf="$CROSS_TOOLCHAIN_PREFIX"gcc \
    CXX_armv7_unknown_linux_gnueabihf="$CROSS_TOOLCHAIN_PREFIX"g++ \
    BINDGEN_EXTRA_CLANG_ARGS_armv7_unknown_linux_gnueabihf="--sysroot=$CROSS_SYSROOT" \
    QEMU_LD_PREFIX="$CROSS_SYSROOT" \
    PKG_CONFIG_PATH="/opt/buildroot-toolchain/lib/pkgconfig/:${PKG_CONFIG_PATH}" \
