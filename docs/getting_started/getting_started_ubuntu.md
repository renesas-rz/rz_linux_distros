---
hide:
  - navigation
  - toc
---

# RZ/G2L Ubuntu Bootable Tool

[Back to Getting Started >](../getting_started/index.md#step-4-create-a-bootable-microsd-card){ .md-button .btn-right }

## 1. Overview

The **RZ/G2L Ubuntu Bootable Tool** is a Bash script designed to streamline the creation and customization of bootable ARM64 Linux disk images for Ubuntu 24.04 on the RZ/G2L platform.

It offers a user-friendly, menu-based interface using `#!bash whiptail` and `#!bash zenity`, making it easy to create, manage disk images and apply custom configurations.

The tool also supports writing images directly to an SD card.

## 2. Objectives

- **Preparing The Image**:
    - Use an existing image or create a new 16GB image.
    - Download Ubuntu 24.04 ARM64 ISO if it doesn't exist already.
    - Use QEMU to install Ubuntu 24.04 from the ISO into the created image.

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
- `#!bash run_qemu.sh`: Start QEMU VM to install Ubuntu 24.04 from the ISO into the image.
- `#!bash install_renesas_sw.sh`: Set up environment for chroot method to install software packages.
- `#!bash install_packages_ubuntu.sh`: Install software packages inside an Ubuntu chroot environment.
- `#!bash attach_sd.sh`: Write bootable image to SD card.
- `#!bash settings.txt`: Save previously used image names.

## 4. Usage

### 4.1 Prerequisites

- Ubuntu 22.04 LTS, and 24.04 LTS are supported.
- Internet connection is required.
- The `#!bash sudo` permission is required.
- 50GB+ available storage.
- 32GB+ microSD card.

### 4.2 How To Use

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
    unzip RTK0EF0045Z0030AZJ-v*.zip
    tar xf RTK0EF0045Z0030AZJ-v*/rz-ubuntu-support-v*.tar.gz -C ${WORK_DIR}
    unzip RTK0EF0045Z0032AZJ-v*_EN.zip
    tar xf RTK0EF0045Z0032AZJ-v*_EN/rz-graphics-v*.tar.gz -C ${WORK_DIR}
    unzip RTK0EF0045Z0038AZJ-v*_EN.zip
    tar xf RTK0EF0045Z0038AZJ-v*_EN/rz-codecs-v*.tar.gz -C ${WORK_DIR}
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

- Run the main script:

    ```bash
    cd ${WORK_DIR}/installer
    ./make_bootable_tool.sh
    ```
    {: .dollar }

- The main menu provides 3 options:
    - [**Select Image**](#421-select-image): Prepare an image with Ubuntu 24.04 pre-installed.
    - [**Install Software**](#422-install-software): Install software packages to the image.
    - [**Write Image**](#423-write-image): Write the image to the SD card.

    ![main_menu.png](images_ubuntu/main_menu.png)

#### 4.2.1 Select Image

- Press ++enter++ on the **Select Image:** and select one of the two options below:

    - **1 Browse**: Select an image with Ubuntu 24.04 pre-installed.
    - **2 Create New**: Create a new 16GiB image and install Ubuntu 24.04 on it.

    ![Select image](images_ubuntu/select_image.png)

!!! note

    When you perform this process for the first time, you need to create a new image (**2 Create New**).
    You can use the created image next time (**1 Browse**).

- For the **1 Browse** option, use the file dialog to select the image (e.g., `#!bash Linux.img`). Next, press **OK** to confirm and skip to [Section 4.2.2](#422-install-software).

    ![Choose image](images_ubuntu/select_available_image.png)

- For the **2 Create New** option, use the file dialog to name the image (e.g., `#!bash Linux.img`) and select its location. Next, press **OK** to confirm.

    ![Image name](images_ubuntu/naming_new_image.png)

- Enter the administrator password. The tool will then download, build, and install QEMU version 8.2.9. If it's already installed, this step will be skipped.

    ![Download QEMU](images_ubuntu/download_qemu.png)

- The tool will allocate and format `#!bash Linux.img`.

    ![Create raw image](images_ubuntu/QEMU_installed_and_create_image.png)

- The tool will download the Ubuntu 24.04 ISO. If it already exists in the `#!bash downloads` folder, this step will be skipped.

    ![Download ISO file](images_ubuntu/download_ubuntu_iso.png)
    ![Compelte Download ISO file](images_ubuntu/complete_download_iso.png)

!!! note

    Downloading the Ubuntu ISO image may take a long time depending on your environment.
    You can also download it from a [**mirror**](https://www.renesas.com/document/sws/ubuntu-24042-live-server-arm64-iso).
    Please choose whichever is more convenient for you. <br>
    When you download the image from the mirror, set up your environment as follows.

    1. If you have never used `#!bash 7z` on your PC, install `#!bash 7z` first.
    ``` bash
    sudo apt install p7zip-full
    ```
    {: .dollar }

    2. Create a directory.
    ``` bash
    mkdir -p ${WORK_DIR}/installer/downloads/
    ```
    {: .dollar }

    3. Decompress the downloaded file from the mirror, and move the ISO image to your working directory.
    ``` bash
    7z x ubuntu-24.04.2-live-server-arm64-iso.7z
    mv ubuntu-24.04.2-live-server-arm64-iso/ubuntu-24.04.2-live-server-arm64.iso ${WORK_DIR}/installer/downloads/
    ```
    {: .dollar }

    4. Then, execute `#!bash ./make_bootable_tool.sh`.

- The tool will use QEMU VM to install Ubuntu 24.04 to `#!bash Linux.img`. Press **OK** to continue.

    ![Start QEMU](images_ubuntu/starting_qemu_vm.png)

- Follow installation instructions in [Section 4.2.4](#424-how-to-install-ubuntu-2404).

- If successful, the **Completed** dialog box will appear. Press **OK** to continue.

    ![Complete install Ubuntu](images_ubuntu/compelete_dialog_install_ubuntu.png)

#### 4.2.2 Install Software

- Press ++enter++ on the **Install Software:**.

    ![Select "Intall Software" option](images_ubuntu/install_renesas_sw.png)

- The tool will install all .deb packages into `#!bash Linux.img`.

    ![Install sofware packages](images_ubuntu/start_install_renesas_sw.png)

- Wait for the **Success** dialog box. Press **OK** to continue.

    ![Finish install software packages](images_ubuntu/install_renesas_sw_sucsess.png)

#### 4.2.3 Write Image

- Plug in the microSD card to the Host PC.

- In the main menu, press ++enter++ on **Write Image:** to write `#!bash Linux.img` to the microSD card.

    ![Select "Write Image" option](images_ubuntu/write_image_to_SD.png)

- Select the microSD card (e.g., `#!bash sdb Transcend (59,5G)`). Press **OK** to continue.

    ![Select storage device](images_ubuntu/select_SD_to_write.png)

- Press **Proceed** to confirm.

    ![Confirm action](images_ubuntu/confirm_write_data.png)

- The tool will write `#!bash Linux.img` to `#!bash sdb Transcend (59,5G)`.

    ![Write image](images_ubuntu/process_write_image.png)

- If successful, the following dialog box will appear. Press **OK** to continue.

    ![Write image_successful](images_ubuntu/success_write_sd.png)

- Execute `#!bash sudo eject /dev/sdb` to safely remove the microSD card from the Host PC.

#### 4.2.4 How to install Ubuntu 24.04

- Press ++enter++ on **Try or Install Ubuntu Server**.

    ![Install Ubuntu 24.04](images_ubuntu/menu_grub_install_qemu.png)

- Press ++enter++ on **Continue in rich mode**.

    ![Choose mode installer](images_ubuntu/QEMU1_continue_in_rich_mode.png)

- Press ++enter++ on **English**.

    ![Select language](images_ubuntu/QEMU2_select_language.png)

- Select your keyboard configuration (e.g., **English (US)**).

    ![Choose keyboard](images_ubuntu/QEMU3_keyboard_config.png)

- Use the default settings and press ++enter++ to continue.

    ![Select server](images_ubuntu/QEMU4_ubuntu_server_select.png)

- Use the default settings and press ++enter++ to continue.

    ![Network setting](images_ubuntu/QEMU5_network_config.png)

- Use the default settings and press ++enter++ to continue.

    ![Proxy setting](images_ubuntu/QEMU6_proxy_config.png)

- Wait for the mirror location to be tested (see below), then press **Done** to continue.

    ![Mirror setting](images_ubuntu/mirror_wait.png)

- Deselect **Set up this disk as an LVM group** and press **Done** to continue.

    ![Storage setting](images_ubuntu/QEMU9_storage_config.png)

- Press ++enter++ to confirm the partitions of the **/dev/vdb** disk.

    ![Storage2 setting](images_ubuntu/QEMU10_storage_config_2.png)

- Select **Continue** to finish partitioning and write changes to the **/dev/vdb** disk.

    ![Storage warn](images_ubuntu/QEMU11_warning.png)

- Enter username (e.g., **user**) and password for the administrator account, then press **Done**.

    ![Profile setting](images_ubuntu/QEMU12_profile_config.png)

- Use the default settings and press ++enter++ to continue.

    ![Skip ubuntu pro setting](images_ubuntu/QEMU13_skip_ubuntu_pro.png)

- Use the default settings and press **Done** to continue.

    ![Skip openssh setting](images_ubuntu/QEMU14_skip_install_OpenSSH_Server.png)

- Do not select any snap package. Press **Done** to continue.

    ![Skip snap setting](images_ubuntu/QEMU15_skip_feature_server_snaps.png)

- Wait for the installation to finish.

    ![Finish setting](images_ubuntu/QEMU16_waiting_install_ubuntu.png)

- Press **Reboot Now** to exit the installer.

    ![Network setting](images_ubuntu/QEMU17_install_ubuntu_sucsess.png)

- Press ++ctrl+a+x++ to exit the VM.

[Back to Getting Started >](../getting_started/index.md#step-4-create-a-bootable-microsd-card){ .md-button .btn-right }
