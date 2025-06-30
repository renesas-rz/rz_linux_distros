## Overview

![](overview/images/ubuntu-debian-block-diagram_overview.svg){ align=right width=400 style=max-width:40% }

This solution provides documents (this website), Renesas software, and script that help you create Ubuntu and Debian environments running on RZ EVK.
The scripts merge Ubuntu/Debian root filesystem and Renesas software (Linux kernel, graphics library, codec library, and so on) to create the Ubuntu/Debian environment.
You can install additional software packages by using _'apt'_ command.
Renesas support covers Renesas deliverables only, i.e., Renesas software, documents, and scripts.

This solution supports the following distros and evaluation boards.

!!! content-wrapper no-indent table-no-sort table-no-hover ""

    +-----------------------------------+-------------------+-----------------------------------------------+
    | Linux Distro                      | kernel Version    | Target Board                                  |
    +===================================+===================+===============================================+
    | Ubuntu 24.04                      | 6.1               | RZ/G2L EVK (RTK9744L23S01000BE)               |
    +-----------------------------------+-------------------+-----------------------------------------------+
    | Debian 13 testing                 | 6.1               | RZ/G2L EVK (RTK9744L23S01000BE)               |
    | version^[1](#tf:1)^{: #tfref:1 }  |                   |                                               |
    +-----------------------------------+-------------------+-----------------------------------------------+

    1. This solution uses Debian 13 [weekly build](https://cdimage.debian.org/cdimage/weekly-builds/){: target=_blank } on May 26, 2025. <br>
    For information about the *testing version*, refer to [Debian Releases](https://www.debian.org/releases/){: target=_blank }. [↩](#tfref:1){: .tf-backref }
    {: #tf:1 }
