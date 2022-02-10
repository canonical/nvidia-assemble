# NVIDIA Assemble Snap

This repository contains source code for an auxiliary snap that
assists in assembling and loading NVIDIA drivers.

Installing this snap implies consent with [NVIDIA
license](https://www.nvidia.com/en-gb/drivers/geforce-license/).

When installed together with a compatible kernel snap, an attempt will
be made to assemble, load and setup NVIDIA graphics drivers on an
Ubuntu Core system.

## Quick Start

    $ sudo snap refresh pc-kernel --channel 22/edge
    $ reboot
    $ sudo snap install --devmode nvidia-assemble --channel 22/edge

## Quick Checks

    $ sudo ls -latr /dev/nvidia*
    $ sudo lsmod | grep nvidia

Expectation is that `/dev/nvidia` devices are available on the system,
and that nvidia drivers are loaded into the kernel.

## Quick Demo

Install content provider with userspace libraries, install sample demo
apps, and connect the content provider.

    $ sudo snap install nvidia-core22
    $ sudo snap install --devmode graphics-core22-samples
    $ sudo snap connect graphics-core22-samples:graphics-core22 nvidia-core22:graphics-core22

After above setup is done one can query EGL card information, query
CUDA device information, and execute a sample CUDA application:

    $ sudo graphics-core22-samples.eglinfo
    $ sudo graphics-core22-samples.deviceQuery
    $ sudo graphics-core22-samples.bandwidthTest

## How to use drivers from your own snap

Once the drivers are available on the system, one can use
[nvidia-core22](https://github.com/xnox/nvidia-core22) graphics-core22
content interface provider in your snap to provide userspace libraries
to access NVIDIA graphics card functionality. This content interface
can be used if stock userspace libraries are sufficient for a given
snap, or if one wants to share userspace libraries across multiple
snaps. Alternatively, one can simply stage one's own userspace
dependencies as required.

Samples of GL & CUDA CLI applications are available from
[graphics-core22-samples](https://github.com/xnox/graphics-core22-samples). This
is an example snap that vendors existing GL binaries, and complies a
CUDA binary from source. The snapcraft.yaml for this sample removes
staged libraries from the snap, such that at runtime all libraries are
used from the nvidia-core22 graphics-core22 content provider.

## Troubleshooting

First is to check that kernel contains nvidia driver components:

    $ sudo ls /lib/modules/*/kernel | grep nvidia

To troubleshoot further collect dmesg output and output from snap
installations:

    $ sudo journalctl -b -k
    $ sudo journalctl -b -u snap.nvidia-assemble.service

## Caveats

Some of the above snaps currently are not fully confined and thus are
installed in devmode. There is ongoing work to allow usage of these
snaps with full confinment.

The above snaps have been tested on GeForce MX350 and GeForce RTX 3070
cards.
