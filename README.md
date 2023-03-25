# Maia SDR Docker images

This repository contains the Dockerfiles and related assets used in [Maia
SDR](https://maia-sdr.org). Pre-built images are available in the
[maiasdr dockerhub](https://hub.docker.com/u/maiasdr).

## maia-sdr-devel

[maiasdr/maia-sdr-devel](https://hub.docker.com/r/maiasdr/maia-sdr-devel)
contains all the development tools used in Maia SDR. These are:

* [Amaranth](https://github.com/amaranth-lang/amaranth).
* The [OSS CAD suite](https://github.com/YosysHQ/oss-cad-suite-build),
  which includes [Yosys](https://yosyshq.net/yosys/).
* [Icarus Verilog](http://iverilog.icarus.com/).
* [cocotb](https://www.cocotb.org/).
* The [Rust](https://www.rust-lang.org/) toolchain for
  `armv7-unknown-linux-gnueabihf` and `wasm32-unknown-unknown`.
* [wasm-pack](https://rustwasm.github.io/wasm-pack/).
* The gcc toolchain distributed with Vitis 2021.2. This is the
  toolchain used to build the Pluto SDR kernel and u-boot. Starting
  with the ADI Pluto firmware v0.36, the buildroot toolchain is used
  to build the buildroot rootfs.
* Miscellaneous tools used to build the Pluto SDR firmware image
  (buildroot requirements, etc.)

Additionally, the image has all the requirements to run
[Vivado](https://www.xilinx.com/products/design-tools/vivado.html) 2021.2, which
is needed to build the FPGA bitstream. Vivado needs to be installed manually on
a volume as described below.

The docker image can be run as follows:
```
docker run --rm --net host -e DISPLAY=$DISPLAY -e TERM \
	--name=maia-sdr-devel --hostname=maia-sdr-devel \
	-v vivado2021_2_volume:/opt/Xilinx -v maia_sdr_devel_home:/home \
        -v $HOME:/hdl \
	-it maiasdr/maia-sdr-devel
```

This assumes that Vivado has been installed to a Docker volume
`vivado2021_2_volume` and uses a volume to hold the home directory (in order to
have persistent bash history, etc.). It mounts the user home directory into
`/hdl`, so that the Maia SDR repositories working copies can be accessed
(different paths can be used here.).

The home of the docker container user should contain the following in `.bashrc`:
```
source /opt/Xilinx/Vivado/2021.2/settings64.sh
source /opt/rust/env
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin/:/usr/bin:/sbin:/bin:/opt/gcc-arm-linux-gnueabi/bin:$PATH:/opt/oss-cad-suite/bin
```

In order to install Vivado to the volume, the installer can be run as root. A
root bash session can be launched in the container by doing
```
docker exec -u 0 -it maia-sdr-devel /bin/bash
```
From this session, it is possible to run the Vivado installer and choose
`/opt/Xilinx` as the installation path.
