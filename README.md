# gombessa

> This is the Glasslab compute resource that is hosted by [UAF RCS](https://www.gi.alaska.edu/services/research-computing-systems), located underneath Butrovich. This is our Linux-based server to configure to and use for our lab's bioinformatic and data analysis needs, and can be used instead or in addition to RCS's [chinook](https://uaf-rcs.gitbook.io/uaf-rcs-hpc-docs/hpc).

## Table of Contents

- [Overview](#overview)
- [Getting Started](#getting-started)
  - [VPN](#VPN)
  - [Logging In](#logging-in)
  - [Troubleshooting](#troubleshooting)
- [Configuration](#configuration)
  - [Module System](#module-system)
  - [Checking for software](#checking-for-software)
  - [Slurm Scheduler](#slurm-scheduler)
  - [Setting up your own environment](#setting-up-your-own-environment)
- [File Transfer](#file-transfer)

## Overview 

Gombessa consists of **1 node, 2 sockets/CPUs, 64 cores per socket (128 cores in total)**. Each core handles **2 threads**, meaning that we could run up to 256 parallel threads in the entire system at once. The total RAM for the entire system is **512G**. A breakdown of memory and storage is listed below in the drop-down portion.

The original quote with hardware specification can be found [here](https://github.com/GlassLabGenomics/gombessa_docs/blob/main/system/UAF_RCS_241014-4C_v2.pdf).

<details>
  <summary> see gombessa hardware specs </summary>

**CPU / Processor**

* 2 x AMD EPYC 7713
    * Each CPU:
        * 64 cores / 128 threads (SMT enabled)
        * Clock speed: 2.00 GHz baseS
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
 
</details>

## Filesystem

```
[yhsieh@gombessa home]$ df -h
Filesystem                      Size  Used Avail Use% Mounted on
devtmpfs                        4.0M     0  4.0M   0% /dev
tmpfs                           252G     0  252G   0% /dev/shm
tmpfs                           101G  955M  100G   1% /run
/dev/mapper/vg_server-root       49G  3.1G   46G   7% /
/dev/mapper/vg_server-home       49G   17G   33G  34% /home
/dev/mapper/vg_server-var        15G  1.1G   14G   8% /var
/dev/mapper/vg_server-tmp       9.7G  104M  9.6G   2% /tmp
/dev/sda2                       968M  308M  610M  34% /boot
/dev/sda4                       200M   12K  200M   1% /boot/efi
/dev/mapper/vg_server-software  100G   30G   71G  30% /usr/local
```

## Getting Started

To use gombessa, you should be familiar with the Unix-style command line interface. This is using your computer via the Terminal app on Mac or Linux computers, and a Linux-emulator on Windows. The recommended way for Windows users is to install an IDE such as VSCode, and interact with the Linux terminal there. Users of chinook will be familiar with this.

Many [command line tutorials](https://ubuntu.com/tutorials/command-line-for-beginners#3-opening-a-terminal) exist, so if you are not familiar with this way of interacting with your computer, look up a tutorial and go through it before using gombessa. 

### VPN

You can access gombessa only if you are on the [UA VPN](https://service.alaska.edu/TDClient/39/Portal/KB/ArticleDet?ID=975). This is a security feature enforced by RCS. Below are instructions for setting up VPN service and connecting to the UA VPN based on your OS.

**MAC and WINDOWS USERS**

Go to this [site](https://www.alaska.edu/oit/services/#id=162) and follow the instructions for downloading GlobalProtect for your computer. You need your UA credentials.

**LINUX USERS**

Take a deep breath. Then try out any of these alternatives.
<details>
  <summary> linux-specific-vpn-installation </summary>
  
1. Install the alternative [GlobalProtect-openconnect](https://github.com/yuezk/GlobalProtect-openconnect), and use the `gpclient launch-gui` to connect to `vpn.alaska.edu`.

With my Ubuntu 22.04 system, the CLI option, `gpclient connect vpn.alaska.edu` bugs out. If it works for you, great, because this is open source.
However, after some time, you might find that the GUI method is requesting you to get a license. If that's the case, uninstall this version and go for option 2.

2. NTS has a restricted access version of the GlobalProtect app for Linux. I have uploaded it to the lab drive [here](https://drive.google.com/file/d/1gzbg3OcgCah3RoxCxC7IkCNDLezM-6nF/view?usp=sharing). Unzip the file `tar -xvzf <>`, and choose which version you want to install. The GUI version is recommended, again: `GlobalProtect_UI_deb-5.3.1.0-36.deb`

No option of a license in this GUI, so this should be a stable installation. Fingers crossed.

</details>

### Logging In

We use ssh to log in. You will be prompted to enter your UA password. Remember to replace whatever is in these brackets `<>` with your own username.

**SSH:**  `ssh <username>@gombessa.rcs.alaska.edu`

### Troubleshooting

If you cannot access gombessa even after correctly setting up VPN, you can try getting onto the UAlaska Wifi network and trying again. If it still doesn't work, you may not have approved access to gombessa. Send system admins (Jessica, Yin, Laura) an email if you cannot access gombessa.

## Configuration

### Module System

Gombessa has the [lmod](https://lmod.readthedocs.io/en/latest/) environment module system, such that software needs to be loaded into the current working environment before using them. Software (libraries) is grouped into modules, with the grouping specific to the compiler that was used to make the library. Without getting too much into specifics, loading a module is done with `module load <modulename>`. If you haven't worked with this before, check out our examples scripts or refer to the lmod documentation. On top of that, you can create your own environments with [mamba](https://mamba.readthedocs.io/en/latest/index.html) or [anaconda](https://www.anaconda.com/). See below for more information on this.

Shown below are our current (5-6-25) core libraries on the system. Admins and RCS are working to expand the list. You are welcome to request software to be added, please add it to [this list](https://docs.google.com/document/d/1QPLZZZg3tKSpwjMUNc2cT1WWQoYBM_fBj2hTtbsaoTU/edit?usp=sharing) and ping us (Yin or Jessica, or both).
```
[yhsieh@gombessa ~]$ module --default avail

------------------------------------------------------------------------------------ /usr/local/modules/easybuild/Core ------------------------------------------------------------------------------------
   Bison/3.8.2              GCCcore/13.3.0                   M4/1.4.19           Perl/5.38.0            flex/2.6.4        gfbf/2024a     picard/3.3.0-Java-17
   FastQC/0.12.1-Java-11    Java/21.0.5              (21)    Nextflow/24.10.2    ant/1.10.12-Java-17    foss/2024a        gompi/2024a    pkgconf/1.8.0
   GCC/13.3.0               Julia/1.9.3-linux-x86_64         OpenSSL/3           binutils/2.42          gettext/0.22.5    ncurses/6.5    zlib/1.3.1
```

### Checking for software

To check if gombessa has your software installed, first load a compiler. 

For example, `module load GCCcore/13.3.0`. 

Then run `module avail`.

Below you can see a list of what is available that has been built with the GCC compiler. You would therefore need to load this compiler before you load your software.
```
[yhsieh@gombessa ~]$ module load GCC/13.3.0
[yhsieh@gombessa ~]$ module avail

---------------------------------------------------------------------------- /usr/local/modules/easybuild/Compiler/GCC/13.3.0 -----------------------------------------------------------------------------
   Arrow/17.0.0       BamTools/2.5.2    FFTW/3.3.10        GSL/2.8                        OpenBLAS/0.3.27    R/4.4.2          SAMtools/1.19.2        SciPy-bundle/2024.05    pybind11/2.12.0
   BEDTools/2.31.1    Boost/1.85.0      FlexiBLAS/3.4.4    HTSlib/1.21                    OpenMPI/5.0.3      SAMtools/1.17    SAMtools/1.21   (D)    matplotlib/3.9.2
   BLIS/1.0           DIAMOND/2.1.11    GEOS/3.12.2        MAFFT/7.526-with-extensions    QUAST/5.2.0        SAMtools/1.18    SOCI/4.0.3             networkx/3.4.2

-------------------------------------------------------------------------- /usr/local/modules/easybuild/Compiler/GCCcore/13.3.0 ---------------------------------------------------------------------------
   Abseil/20240722.0                   Kaleido/0.2.1                  PostgreSQL/16.4                     cppy/1.2.1                 libffi/3.4.5               nettle/3.10
   Autoconf/2.72                       LAME/3.100                     PyYAML/6.0.2                        cryptography/42.0.8        libgeotiff/1.7.3           nlohmann_json/3.11.3
   Automake/1.16.5                     LERC/4.0.0                     Python-bundle-PyPI/2024.06          expat/2.6.2                libgit2/1.8.1              nodejs/20.13.1
   Autotools/20231222                  LLVM/18.1.8-minimal            Python/3.12.3                       flex/2.6.4          (D)    libglvnd/1.7.0             numactl/2.0.18
   BWA/0.7.18                          LMDB/0.9.31                    Qhull/2020.2                        flit/3.9.0                 libiconv/1.17              patchelf/0.18.0
   Bison/3.8.2                  (D)    LibTIFF/4.6.0                  RE2/2024-07-02                      fontconfig/2.15.0          libjpeg-turbo/3.0.1        pixman/0.43.4
   Brotli/1.1.0                        LittleCMS/2.16                 RapidJSON/1.1.0-20240815            fonttools/4.53.1           libogg/1.3.5               pkgconf/2.2.0            (D)
   Brunsli/0.1                         M4/1.4.19               (D)    Rust/1.78.0                         freetype/2.13.2            libopus/1.5.2              plotly.py/5.24.1
   CFITSIO/4.4.1                       MPFR/4.2.1                     SQLite/3.45.3                       gettext/0.22.5      (D)    libpciaccess/0.18.1        poetry/1.8.3
   CMake/3.29.3                        Mako/1.3.5                     SWIG/4.2.1                          giflib/5.2.1               libpng/1.6.43              pydantic/2.9.1
   Catch2/2.13.10                      Mesa/24.1.3                    Szip/2.1.1                          giflib/5.2.2        (D)    libreadline/8.2            scikit-build/0.17.6
   Cython/3.0.10                       Meson/1.4.0                    Tcl/8.6.14                          googletest/1.15.2          libsndfile/1.2.2           setuptools-rust/1.9.0
   Doxygen/1.11.0                      NASM/2.16.03                   Tk/8.6.14                           gperf/3.1                  libtirpc/1.3.5             snappy/1.2.1
   Eigen/3.4.0                         NLopt/2.7.1                    Tkinter/3.12.3                      groff/1.23.0               libtool/2.4.7              tiktoken/0.9.0
   FLAC/1.4.3                          Ninja/1.12.1                   UCC/1.3.0                           hatchling/1.24.2           libunwind/1.8.1            tqdm/4.66.5
   FriBidi/1.0.15                      OpenEXR/3.2.4                  UCX/1.16.0                          help2man/1.49.3            libvorbis/1.3.7            typing-extensions/4.11.0
   GLPK/5.0                            OpenJPEG/2.5.2                 UDUNITS/2.2.28                      hwloc/2.10.0               libwebp/1.4.0              utf8proc/2.9.0
   GLib/2.80.4                         PCRE/8.45                      Wayland/1.23.0                      hypothesis/6.103.1         libxml2/2.12.7             util-linux/2.40
   GMP/6.3.0                           PCRE2/10.43                    X11/20240607                        intltool/0.51.0            libxslt/1.1.42             virtualenv/20.26.2
   GObject-Introspection/1.80.1        PMIx/5.0.2                     XZ/5.4.5                            jbigkit/2.1                libyaml/0.2.5              xorg-macros/1.20.1
   Ghostscript/10.03.1                 PROJ/9.4.1                     Xerces-C++/3.2.5                    json-c/0.17                lit/18.1.8                 yaml-cpp/0.8.0
   HDF/4.3.0                           PRRTE/3.0.5                    Xvfb/21.1.14                        libGLU/9.0.3               lz4/1.9.4                  zlib/1.3.1               (L,D)
   HarfBuzz/9.0.0                      Pango/1.54.0                   binutils/2.42              (L,D)    libarchive/3.7.4           make/4.4.1                 zstd/1.5.6
   ICU/75.1                            Perl-bundle-CPAN/5.38.2        bzip2/1.0.8                         libdeflate/1.20            makeinfo/7.1
   ImageMagick/7.1.1-38                Perl/5.38.2             (D)    cairo/1.18.0                        libdrm/2.4.122             maturin/1.6.0
   Imath/3.1.11                        Pillow-SIMD/10.4.0             cffi/1.16.0                         libevent/2.1.12            meson-python/0.16.0
   JasPer/4.2.4                        Pillow/10.4.0                  cpio/2.15                           libfabric/1.21.0           ncurses/6.5         (D)

```

Some more module commands to configure your environment are given below. The table is taken from [this useful tutorial](https://nesusws-tutorials-bd-dl.readthedocs.io/en/latest/).


| Command                        | Description                                                   |
|--------------------------------|---------------------------------------------------------------|
| `module avail`                 | Lists all the modules which are available to be loaded        |
| `module spider <pattern>`      | Search for <pattern> among available modules **(Lmod only)**  |
| `module load <mod1> [mod2...]` | Load a module                                                 |
| `module unload <module>`       | Unload a module                                               |
| `module list`                  | List loaded modules                                           |
| `module purge`                 | Unload all modules (purge)                                    |
| `module display <module>`      | Display what a module does                                    |
| `module use <path>`            | Prepend the directory to the MODULEPATH environment variable  |
| `module unuse <path>`          | Remove the directory from the MODULEPATH environment variable |


### Slurm Scheduler

RCS is currently working on installing slurm.

### Setting up your own environment

#### Automatically loading modules into the workspace

**Only do this if you are mainly working interactively (for example, through an IDE)**

<details>
  <summary> load modules upon login </summary>

If you primarily work with several types of packages loaded, you can consider configuring your login to automatically load modules. 

You do this by modifying your `.bashrc` file and adding this segment, putting module commands below the comment:

```
if [ -z "$BASHRC_READ" ]; then
   export BASHRC_READ=1
   # Place any module commands here
   module load <>
fi
```

For example, my `.bashrc` file has: 

```
if [ -z "$BASHRC_READ" ]; then
   export BASHRC_READ=1
   # Place any module commands here
   module load foss/2024a
   module load Python/3.12.3
fi
```
**NOTE: this doesn't affect any jobs you submit via slurm, you will still have to specify in your job script which modules you load, and always preface with `module purge`**
</details>


#### Installing your own software (recommended)

<details>
  <summary> creating and using virtual environments </summary>

</details>


#### Installing your own software with EasyBuild (more advanced, please make sure you know what you're doing)

<details>
  <summary> installing and using EasyBuild </summary>

**For general purpose software shared across users, installer has no root access**

According to its documentation, [EasyBuild](https://docs.easybuild.io/installation/) is a software build and installation framework that allows you to manage (scientific) software on High Performance Computing (HPC) systems in an efficient way. It basically does the heavy-lifting of software installing in the background to arrange dependencies and avoid versioning issues, and allows for sharing of installed software, co-existence of packages of different versions, and is generally very useful if you want to set up a server. 

We recommend you use it to install software that may be useful to everyone on the compute node, such as general purpose alignment software, or other types of well-known and well-used bioinformatics software. For all other personal-use software please install it within your own virtual environment with conda or mamba.

Below I detail the code used to install Mamba on gombessa. EasyBuild is Python-dependent so requires you have the Python module loaded.

```
## load the necessary libraries
module load foss/2024a 
module load Python/3.12.3

## install easybuild 
pip install easybuild
```

Successful installation of EasyBuild will put an easybuild folder in your .local directory that is structured as follows.
```
[yhsieh@gombessa ~]$ ls .local/easybuild/
build  easyconfigs  ebfiles_repo  modules  scripts  software  sources
```

EasyBuild installs use easyconfig files, that you can find with `eb --search <softwarename>`. Most software you will be installing with EasyBuild will be already included in their own repository of config files. 

Make sure you test with a dry run before installing your package.

```
## test out what files are downloaded if this is installed
eb Miniforge3-24.11.3-0 --dry-run

## actually install the package
eb Miniforge3-24.11.3-0.eb
```

Once installation is successful, make sure you add the path of your EasyBuild installed packages to lmod, using the `module use` command. The path to my EasyBuild-installed packages is `/home/yhsieh/.local/easybuild/modules/all/`, so I run `module use /home/yhsieh/.local/easybuild/modules/all`. I can also add this to my `.bashrc`.

If I check what modules are available to me now I can see:
```
[yhsieh@gombessa ~]$ module avail

----------------------------------------------------------------------------------- /home/yhsieh/.local/easybuild/modules/all ------------------------------------------------------------------------------------
   Miniforge3/24.11.3-0

```

I can run `module load Miniforge3/24.11.3-0` and I am ready to use `conda` and `mamba` as usual.

</details>

## File Transfer
