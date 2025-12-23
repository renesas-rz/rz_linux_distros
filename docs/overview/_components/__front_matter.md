## Overview

![](overview/images/ubuntu-debian-block-diagram_overview.svg){ align=right width=400 style=max-width:40% }

This solution provides documents (this website), Renesas software, and script that help you to download Ubuntu and Debian ISO image from their web site, then create Ubuntu and Debian environments running on RZ EVK.
The scripts merge Ubuntu/Debian root filesystem and Renesas software (Linux kernel, graphics library, codec library, and so on) to create the Ubuntu/Debian environment.
You can install additional software packages by using _'apt'_ command.
Renesas support covers Renesas deliverables only, i.e., Renesas software, documents, and scripts.
Ubuntu image is maintained by Canonical. Support is available through Canonical.

<br>
This solution supports the following distros and evaluation boards.

!!! content-wrapper no-indent table-no-sort table-no-hover ""

    +-------------------------------------------+-----------------------------------------------+-------------------+-------------------+-------------------------------+-------------------------------+
    | Linux Distro                              | Target Board                                  | Package Version   | kernel Version    | Limitations                   | Known Issues                  |
    +===========================================+===============================================+===================+===================+===============================+===============================+
    | Ubuntu 24.04.3                            | RZ/G2L EVK (RTK9744L23S01000BE)               | v1.0.2            | 6.1               | -                             | -                             |
    +-------------------------------------------+-----------------------------------------------+-------------------+-------------------+-------------------------------+-------------------------------+
    | Ubuntu 24.04.3                            | RZ/G3E EVK (RTK9947E57S01000BE)               | v1.0.0            | 6.1               | [L#1](./#limitations)         | -                             |
    +-------------------------------------------+-----------------------------------------------+-------------------+-------------------+-------------------------------+-------------------------------+
    | Debian 13.2                               | RZ/G2L EVK (RTK9744L23S01000BE)               | v1.0.2            | 6.1               | -                             | [K#1](./#known-issues)        |
    +-------------------------------------------+-----------------------------------------------+-------------------+-------------------+-------------------------------+-------------------------------+
    | Debian 13.1                               | RZ/G3E EVK (RTK9947E57S01000BE)               | v1.0.0            | 6.1               | [L#1](./#limitations)         | [K#1](./#known-issues)        |
    +-------------------------------------------+-----------------------------------------------+-------------------+-------------------+-------------------------------+-------------------------------+


The [Getting Started](getting_started/index.md) section provides a complete guide to running Ubuntu and Debian on the RZ MPU EVK.
It covers environment preparation, board setup, and steps to run Ubuntu and Debian.
**Get your board and get started!**

### Limitations

Limitations in Renesas deliverables are shown below:

!!! content-wrapper no-indent table-no-sort table-no-hover ""

	| ID            | Description                                   | Planned Fix           |
	| ------------- | --------------------------------------------- | --------------------- |
	| **L#1**       | On the Renesas Linux for RZ/G3E EVK, slight striping artifacts might occur in the video stream captured with a USB camera. | T.B.D. |

### Known Issues

Known issues in Ubuntu / Debian root file system are shown below:

!!! content-wrapper no-indent table-no-sort table-no-hover ""

	| ID            | Description                                   |
	| ------------- | --------------------------------------------- |
	| **K#1**       | On Debian 13 RC1 and Debian 13, `#!bash aplay` in [alsa-utils](https://github.com/alsa-project/alsa-utils) does not correctly handle 24-bit WAV audio files. |

