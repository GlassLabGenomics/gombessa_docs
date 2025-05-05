# gombessa

> This is the Glasslab compute resource that is hosted by [UAF RCS](https://www.gi.alaska.edu/services/research-computing-systems), located underneath Butrovich. This is our Linux-based server to configure to and use for our lab's bioinformatic and data analysis needs, and can be used instead or in addition to RCS's [chinook](https://uaf-rcs.gitbook.io/uaf-rcs-hpc-docs/hpc).

## Table of Contents

- [Overview](#overview)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Configuration](#configuration)
  - [Basic Configuration](#basic-configuration)
  - [Advanced Configuration](#advanced-configuration)
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

To use gombessa (as is the case with chinook), you should be familiar with working with a Unix-style command line. This is the Terminal app on Mac or Linux computers, and a Linux-emulator on Windows. The recommended way for Windows users is to install an IDE such as VSCode, and interact with the Linux terminal there.

Many [command line tutorials](https://ubuntu.com/tutorials/command-line-for-beginners#3-opening-a-terminal) exist, so if you are not familiar with this way of interacting with your computer, look up a tutorial and go through it before using gombessa. 

### Logging In

You can access gombessa only if you are on the [UA VPN](https://service.alaska.edu/TDClient/39/Portal/KB/ArticleDet?ID=975). This is a security feature enforced by RCS. Then, you use ssh to log in, with your UA username and password.

**SSH**

`ssh username@gombessa.rcs.alaska.edu`

### Troubleshooting
