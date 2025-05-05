# gombessa

> This is the Glasslab compute resource that is hosted by [UAF RCS](https://www.gi.alaska.edu/services/research-computing-systems), located underneath Butrovich. This is our Linux-based server to configure to and use for our lab's bioinformatic and data analysis needs, and can be used instead or in addition to RCS's [chinook](https://uaf-rcs.gitbook.io/uaf-rcs-hpc-docs/hpc).

## Table of Contents

- [Overview](#overview)
- [Getting Started](#getting-started)
  - [Logging In](#logging-in)
  - [Troubleshooting](#troubleshooting)
- [Configuration](#configuration)
  - [Module System](#module-system)
  - [Slurm Scheduler](#slurm-scheduler)
- [Usage](#usage)
  - [Connecting to the Server](#connecting-to-the-server)
  - [Key Features and Commands](#key-features-and-commands)
- [Support](#support)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## Overview 

Gombessa consists of **1 node, 2 sockets/CPUs, 64 cores per socket (128 cores in total)**. Each core handles **2 threads**, meaning that we could run up to 256 parallel threads in the entire system at once. The total RAM for the entire system is **512G**. A breakdown of memory and storage is listed below. 

The original quote with hardware specification can be found [here](https://github.com/GlassLabGenomics/gombessa_docs/blob/main/system/UAF_RCS_241014-4C_v2.pdf).

**CPU / Processor**

* 2 x AMD EPYC 7713
    * Each CPU:
        * 64 cores / 128 threads (SMT enabled)
        * Clock speed: 2.00 GHz base
        * 256 MB L3 Cache
        * 225W TDP
    * Total: 128 cores / 256 threads

**Memory**

* 16 x 32GB DDR4 ECC RDIMM
    * Speed: 3200MHz
    * Type: PC4-25600
    * Total RAM: 512GB

**Storage**

* 8 x Solidigm D3-S4520 7.68TB SSDs
    * SATA 6Gb/s
    * Endurance: 2.6 DWPD
    * Total storage: ~61.44TB

## Getting Started

To use gombessa, you should be familiar with the Unix-style command line interface. This is using your computer via the Terminal app on Mac or Linux computers, and a Linux-emulator on Windows. The recommended way for Windows users is to install an IDE such as VSCode, and interact with the Linux terminal there. Users of chinook will be familiar with this.

Many [command line tutorials](https://ubuntu.com/tutorials/command-line-for-beginners#3-opening-a-terminal) exist, so if you are not familiar with this way of interacting with your computer, look up a tutorial and go through it before using gombessa. 

### Logging In

You can access gombessa only if you are on the [UA VPN](https://service.alaska.edu/TDClient/39/Portal/KB/ArticleDet?ID=975). This is a security feature enforced by RCS. 
If you are on a Linux system, install the alternative [GlobalProtect-openconnect](https://github.com/yuezk/GlobalProtect-openconnect), and use the `gpclient launch-gui` to connect to `vpn.alaska.edu`. With my Ubuntu 22.04 system, the CLI option, `gpclient connect vpn.alaska.edu` does not work.

We use ssh to log in. You will be prompted to enter your UA password.

**SSH:**  
`ssh username@gombessa.rcs.alaska.edu`

### Troubleshooting

If you cannot access gombessa even after correctly setting up VPN. You can try getting onto the UAlaska Wifi network and trying again. If it still doesn't work, you may not have approved access to gombessa. Send system admins (Jessica, Yin, Laura) an email if you cannot access gombessa.

## Configuration

### Module System

### Slurm Scheduler
