FROM ghcr.io/cross-rs/armv7-unknown-linux-gnueabihf:0.2.5
ARG DEBIAN_FRONTEND=noninteractive

# Set up env variables to cross-compile openblas and install packages required
# to build pm-remez

ENV OPENBLAS_TARGET=armv7
ENV OPENBLAS_HOSTCC=gcc
ENV OPENBLAS_CC=arm-linux-gnueabihf-gcc
ENV OPENBLAS_FC=arm-linux-gnueabihf-gfortran

RUN apt-get update && \
    apt-get -y install pkg-config libssl-dev gfortran-arm-linux-gnueabihf && \
    rm -rf /var/lib/apt/lists/*
