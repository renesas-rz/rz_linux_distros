---
hide:
  - navigation
  - toc
---

# RZ/G2L Debian Bootable Tool

[Back to Getting Started >](../getting_started/index.md#step-4-create-a-bootable-microsd-card){ .md-button .btn-right }

## 1. Overview

The **RZ/G2L Debian Bootable Tool** is a Bash script designed to streamline the creation and customization of bootable ARM64 Linux disk images for Debian 13 on the RZ/G2L platform.

It offers a user-friendly, menu-based interface using `#!bash whiptail` and `#!bash zenity`, making it easy to create, manage disk images and apply custom configurations.

The tool also supports writing images directly to an SD card.

## 2. Objectives

- **Preparing The Image**:
    - Use an existing image or create a new 16GB image.
    - Download Debian 13 ARM64 ISO if it doesn't exist already.
    - Use QEMU to install Debian 13 from the ISO into the created image.

- **Install Software Packages**:
    - Install OSS (Open-source Software) packages including CIP kernel, GStreamer, MMNGR, VSPMIF.
    - Install HW Graphics packages (optional).
    - Install HW Codec packages (optional).

- **Write The Image**:
    - Select and write the image to an SD card.

## 3. Scripts Details

The bootable Linux image creation process is implemented as several small scripts for easy maintenance.
Here is the introduction and main functions.

- `#!bash make_bootable_tool.sh`: Contain the main menu selection.
- `#!bash install_qemu.sh`: Install QEMU version 8.2.9.
- `#!bash run_qemu.sh`: Start QEMU VM to install Debian 13 from the ISO into the image.
- `#!bash install_renesas_sw.sh`: Set up environment for chroot method to install software packages.
- `#!bash install_packages_debian.sh`: Install software packages inside a Debian chroot environment.
- `#!bash attach_sd.sh`: Write bootable image to SD card.
- `#!bash settings.txt`: Save previously used image names.

## 4. Usage

### 4.1 Prerequisites

- Ubuntu 22.04 LTS, and 24.04 LTS are supported.
- Internet connection is required.
- The `#!bash sudo` permission is required.
- 50GB+ available storage.
- 32GB+ microSD card.

### 4.2 How To Use Debian Installer

- Set the following environment:

    ```bash
    export DL_DIR=<A directory path where packages downloaded in step 3 are stored>
    export WORK_DIR=<A path to your working directory>
    ```
    {: .dollar }

- Create your working directory, and decompress packages:

    ```bash
    mkdir -p ${WORK_DIR}
    cd ${DL_DIR}
    unzip RTK0EF0045Z0031AZJ-v*.zip
    tar xf RTK0EF0045Z0031AZJ-v*/rz-debian-support-v*.tar.gz -C ${WORK_DIR}
    unzip RTK0EF0045Z0033AZJ-v*_EN.zip
    tar xf RTK0EF0045Z0033AZJ-v*_EN/rz-graphics-v*.tar.gz -C ${WORK_DIR}
    unzip RTK0EF0045Z0039AZJ-v*_EN.zip
    tar xf RTK0EF0045Z0039AZJ-v*_EN/rz-codecs-v*.tar.gz -C ${WORK_DIR}
    ```
    {: .dollar }


- Check your working directory:

    ```bash
    ls ${WORK_DIR}
    ```
    {: .dollar }

    You will see the following directories.

    ```bash
    bootloaders  installer  oss  rz-codecs  rz-graphics
    ```

- Execute `#!bash ./make_bootable_tool.sh` to open the GUI:

    ```bash
    cd ${WORK_DIR}/installer
    ./make_bootable_tool.sh
    ```
    {: .dollar }

- The main menu provides 3 options:
    - [**Select Image**](#421-select-image): Prepare an image with Debian 13 pre-installed.
    - [**Install Software**](#422-install-software): Install software packages on the image.
    - [**Write Image**](#423-write-image): Write the image to the SD card.

    ![Main menu](images_debian/main_menu.png)

#### 4.2.1 Select Image

- Press ++enter++ on the **Select Image:** and select one of the two options below:

    - **1 Browse**: Select an image with Debian 13 pre-installed.
    - **2 Create New**: Create a new 16GiB image and install Debian 13 on it.

    ![Prepare image](images_debian/browse_or_create_new.png)

!!! note

    When you perform this process for the first time, you need to create a new image (**2 Create New**).
    You can use the created image next time (**1 Browse**).

- For the **1 Browse** option, use the file dialog to select the image (e.g., `#!bash Debian_13.img`). Next, press **OK** to confirm and skip to [Section 4.2.2](#422-install-software).

    ![Choose image](images_debian/choose_image.png)

- For the **2 Create New** option, use the file dialog to name the image (e.g., `#!bash Debian_13.img`) and select its location. Next, press **OK** to confirm.

    ![Image name](images_debian/create_image.png)

- Enter the administrator password. The tool will then download, build, and install QEMU version 8.2.9. If it's already installed, this step will be skipped.

    ![Download QEMU](images_debian/download_QEMU.png)

- The tool will allocate and format `#!bash Debian_13.img`.

    ![Create raw image](images_debian/create_raw_image.png)

- The tool will download **Debian 13 ISO**. If it already exists in the `#!bash downloads` folder, this step will be skipped.

    ![Download ISO file](images_debian/download_iso_file.png)

- The tool will use QEMU VM to install Debian 13 on `#!bash Debian_13.img`. Press "Ok" to continue.

    ![Start QEMU](images_debian/start_QEMU.png)

- Follow installation instructions in [Section 4.3](#43-how-to-install-debian-13).

- If successful, the **Completed** dialog box will appear. Press **OK** to continue.

    ![Complete install Debian 13](images_debian/complete_install_debian_13.png)

#### 4.2.2 Install Software

- Press Enter on the **Install Software:**.

    ![Select "Install Software" option](images_debian/option_install_software.png)

- Enter the administrator password. The tool will then install all .deb packages into `Debian_13.img`.

    ![Install software packages](images_debian/install_renesas_sw.png)

- Wait for the **Success** dialog box. Press **OK** to continue.

    ![Finish install software packages](images_debian/install_renesas_sw_successful.png)

#### 4.2.3 Write Image

- Plug in the microSD card to the Host PC.

- Press Enter on **Write Image:** to write `#!bash Debian_13.img` to the microSD card.

    ![Select "Write Image" option](images_debian/option_write_image.png)

- Select the microSD card (e.g., `#!bash sdb Transcend (59,5G)`). Press **OK** to continue.

    ![Select microSD card](images_debian/select_device_to_write.png)

- Press **Proceed** to confirm.

    ![Confirm action](images_debian/confirm_write_image.png)

- The tool will write `#!bash Debian_13.img` to `#!bash sdb Transcend (59,5G)`. Wait until 16GiB of data is written.

    ![Write image](images_debian/write_image.png)

- If successful, the following dialog box will appear. Press **OK** to continue.

    ![Write image_successful](images_debian/write_image_successful.png)

    ![Attach_sd_successful](images_debian/attach_sd_successful.png)

- Press **Save and Exit** to save the settings. Then press **OK** to exit.

    ![Return main menu](images_debian/option_write_image.png)

    ![Save setting](images_debian/save_setting.png)

    ![Exit tool](images_debian/exit_tool.png)

- Execute `#!bash sudo eject /dev/sdb` to safely remove the microSD card from the Host PC.

### 4.3 How To Install Debian 13

- Press ++enter++ on **Install**.

    ![Install Debian 13](images_debian/install_debian_13.png)

- Press ++enter++ on **English**.

    ![Select language](images_debian/select_language.png)

- Select your country, territory, or area (e.g., **United States**).

    ![Select location](images_debian/select_location.png)

- Select your keyboard configuration (e.g., **American English**).

    ![Configure keyboard](images_debian/configure_keyboard.png)

- Edit the hostname (e.g., **debian**), then press **Continue**.

    ![Host name](images_debian/enter_hostname.png)

- Enter the domain name, then press **Continue** (leave blank if not applicable).

    ![Domain name](images_debian/domain_name.png)

- Set and verify the root password.

    ![Root password](images_debian/password_for_root.png)

    ![Verify root password](images_debian/verify_root_password.png)

- Create a user account (e.g., **user**).

    ![User name](images_debian/new_user.png)

    ![Name account](images_debian/new_user_2.png)

    ![User password](images_debian/password_for_user.png)

    ![Verify user password](images_debian/verify_user_password.png)

- Select your time zone (e.g., **Eastern**).

    ![Configure clock](images_debian/configure_clock.png)

- Press ++enter++ on **Guided - use entire disk**.

    ![Use entire disk](images_debian/use_entire_disk.png)

- Press ++enter++ on **Virtual disk 1 (vda) - 17.2 GB Virtio Block Device**.

    ![Select Disk](images_debian/select_disk.png)

- Press ++enter++ on **All files in one partition (recommended for new users)**.

    ![One partition](images_debian/configure_one_partition.png)

- Press ++enter++ on **Finish partitioning and write changes to disk**, then confirm with **Yes**.

    ![Finish partitioning](images_debian/finish_partitioning.png)

    ![Write to disk](images_debian/write_to_disk.png)

- Press **Yes** to update the software using a network mirror.

    ![Network mirror](images_debian/network_mirror.png)

- Select archive mirror country (e.g., **United States**).

    ![Mirror country](images_debian/mirror_country.png)

- Press ++enter++ on **deb.debian.org**.

    ![Archive mirror](images_debian/debian_archive_mirror.png)

- Edit HTTP proxy information (leave blank if not applicable), then press **Continue**.

    ![HTTP proxy](images_debian/http_proxy.png)

- Press **NO** to skip the package usage survey.

    ![Usage survey](images_debian/usage_survey.png)

- Do not select any desktop environment. Press **Continue** to proceed.

    ![Software selection](images_debian/software_selection.png)

- If successful, the **Installation complete** dialog box will appear. Click **Continue** to exit the installer.

    ![Reboot](images_debian/reboot.png)

- At GRUB interface, press ++ctrl+a+x++ to exit the VM.

    ![Grub menu](images_debian/enter_debian.png)

[Back to Getting Started >](../getting_started/index.md#step-4-create-a-bootable-microsd-card){ .md-button .btn-right }
