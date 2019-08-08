OpenEmbedded/Yocto BSP layer for Google Coral Dev Board
=======================================================

**NOTE!** This layer is still an work in progress and this might not be the final location
nor are there any guarantees that I will not rebase the repository before I
consider it complete. If you are interested in this repository please open
a issue to get an status update.

This layer provides support for Coral Dev Board for use with OpenEmbedded
and/or Yocto.

This layer depends on:

    URI: git://git.openembedded.org/openembedded-core
    branch: warrior
    revision: HEAD

    URI: https://github.com/Freescale/meta-freescale.git
    branch: warrior
    revision: HEAD

Quick start
-----------

As this layer depends on the Freescale/NXP BSP we can utilize the base setup
from there.

Create directory where you want to store the environment and change the shell
to that location:

    mkdir coral && cd coral

Initialize repo manifest:

    repo init -u https://github.com/Freescale/fsl-community-bsp-platform -b warrior

Fetch layers in manifest:

    repo sync

Clone `meta-coral`:

    git clone https://github.com/mirzak/meta-coral.git sources/meta-coral

Setup the environment:

    MACHINE=coral-dev DISTRO=fslc-wayland source ./setup-environment build

Add the `meta-coral` layer to bblayers.conf:

    echo 'BBLAYERS += "${BSPDIR}/sources/meta-coral"' >> conf/bblayers.conf

Start baking:

    bitbake core-image-full-cmdline

then use bmaptool to write image to SD card, e.g.

    sudo bmaptool copy --nobmap tmp/deploy/images/coral-dev/core-image-full-cmdline-coral-dev-20190808100038.rootfs.wic.gz /dev/sdb

Insert SD card, set jumpers 3 and 4 to ON, and before you boot,
connect to serial console via picocom:

    picocom -b 115200 /dev/ttyUSB0

