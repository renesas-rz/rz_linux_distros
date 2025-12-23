# Ubuntu Bootable Tool

## 1. Overview

The **Ubuntu Bootable Tool** is a Bash script designed to streamline the creation and customization of bootable ARM64 Linux disk images for Ubuntu 24.04 on the RZ/G platforms. It offers a user-friendly, menu-based interface using `whiptail` and `zenity`, making it easy to create, and apply custom configurations. The tool also supports writing images directly to an external media (e.g., SD card).

## 2. Objectives

- **Prepare The Image**:
  - Use an existing image or create a new image.
  - Download Ubuntu 24.04 ARM64 ISO if it doesn't exist already.
  - Use QEMU to install Ubuntu 24.04 to the image.

- **Install Software Packages**:
  - Install OSS (Open Source Software) packages including: CIP kernel, GStreamer, MMNGR, VSPMIF.
  - Install HW Graphics packages (optional).
  - Install HW Codecs packages (optional).

- **Write The Image**:
  - Select and write the image to an external media (e.g., SD card).

## 3. Scripts Details

Split into smaller scripts for easier maintenance. The following introduces the scripts and their main functions:

- `make_bootable_image.sh`: Contain the main menu selection.
- `install_qemu.sh`: Install QEMU version 8.2.9.
- `run_qemu.sh`: Start QEMU VM to install Ubuntu 24.04 to the image.
- `install_renesas_sw.sh`: Set up chroot environment to install software packages.
- `install_packages_ubuntu.sh`: Install .deb packages.
- `write_image.sh`: Write the image to an external media (e.g., SD card).
- `settings.txt`: Contain user settings.

## 4. Usage

### 4.1 Prerequisites

- Supported OS: Ubuntu 22.04 LTS or 24.04 LTS.
  - Internet connection is required.
  - The sudo permission is required.
- Tested hardware:
  - Processor: Intel Core i5-7400 (4 cores, 4 threads).
  - RAM: 8 GB.
  - Storage: Minimum 50 GB available.
- External media: At least 32 GB (e.g., microSD card).

### 4.2 How To Use

- Run the main script:

  ```bash
  ./make_bootable_image.sh
  ```

- The main menu provides 4 options:

  - [**Create new Ubuntu image.**](#421-create-new-ubuntu-image)
  - [**Select available bootable image.**](#422-select-available-bootable-image)
  - [**Install Renesas kernel and proprietary software.**](#423-install-renesas-kernel-and-proprietary-software)
  - [**Write image to external media.**](#424-write-image-to-external-media)

  ![main_menu.png](images_ubuntu/main_menu.png)

#### 4.2.1 Create new Ubuntu image

- Press Enter on the "Create new Ubuntu image".

  ![Create new Ubuntu image](images_ubuntu/create_new_ubuntu_image.png)

- Use the file dialog to name the image (e.g., Linux.img) and select its location. Next, press "OK" to confirm.

  ![Image name](images_ubuntu/naming_new_image.png)

- Enter administrator password. The tool will then download, build, and install QEMU version 8.2.9. If it's already installed, this step will be skipped.

  ![Download QEMU](images_ubuntu/download_qemu.png)

- The tool will allocate and format `Linux.img`.

  ![Create raw image](images_ubuntu/QEMU_installed_and_create_image.png)

- The tool will download the Ubuntu 24.04 ISO. If it already exists in the `downloads` folder, this step will be skipped.

  ![Download ISO file](images_ubuntu/download_ubuntu_iso.png)

  ![Complete Download ISO file](images_ubuntu/complete_download_iso.png)

- The tool will use QEMU VM to install Ubuntu 24.04 to `Linux.img`. Press "Ok" to continue.

  ![Start QEMU](images_ubuntu/starting_qemu_vm.png)

- Follow installation instructions in [Section 4.3](#43-how-to-install-ubuntu-2404).

- If successful, the "Completed" dialog box will appear. Press "Ok" to continue.

  ![Complete install Ubuntu](images_ubuntu/complete_dialog_install_ubuntu.png)

- The new image will now appear as the "Selected image" in the main menu.

  ![Select image after created](images_ubuntu/select_image_after_created.png)

#### 4.2.2 Select available bootable image

- Press Enter on the "Select available bootable image".

  ![Option select image](images_ubuntu/select_image.png)

- Use the file dialog to select the image (e.g., Linux.img). Next, press "OK" to confirm.

  ![Choose image](images_ubuntu/choose_available_image.png)

- Once selected, the image will appear as the "Selected image" in the main menu.

  ![Select image after selected](images_ubuntu/select_image_after_selected.png)

#### 4.2.3 Install Renesas kernel and proprietary software

- Please [create](#421-create-new-ubuntu-image) or [select](#422-select-available-bootable-image) an image first.

- Press Enter on the "Install Renesas kernel and proprietary software".

  ![Select "Install Software" option](images_ubuntu/install_renesas_sw.png)

- The tool will install all .deb packages into `Linux.img`.

  ![Install software packages](images_ubuntu/start_install_renesas_sw.png)

- Wait for the dialog box below to appear, then click 'Ok' to continue.

  ![Finish install software packages](images_ubuntu/install_renesas_sw_success.png)

#### 4.2.4 Write image to external media

- **Note:** In this guide, we use a microSD card to illustrate the steps.

- Please [create](#421-create-new-ubuntu-image) or [select](#422-select-available-bootable-image) an image first.

- Plug in the microSD card to the Host PC.

- In the main menu, press Enter on "Write image to external media" to write `Linux.img` to the microSD card.

  ![Select "Write Image" option](images_ubuntu/write_image_to_SD.png)

- Select the microSD card (e.g., "sdb Transcend (59,5G)"). Press "Ok" to continue.

  ![Select storage device](images_ubuntu/select_SD_to_write.png)

- Press "Proceed" to confirm.

  ![Confirm action](images_ubuntu/confirm_write_data.png)

- The tool will write `Linux.img` to `sdb Transcend (59,5G)`.

  ![Write image_with_bmap](images_ubuntu/write_image_with_bmap.png)

- If successful, the following dialog box will appear. Press "Ok" to continue.

  ![Write image_successful](images_ubuntu/success_write_sd.png)

- Execute "sudo eject /dev/sdb" to safely remove the microSD card from the Host PC.

#### 4.2.5 Edit settings.txt (optional)

##### 4.2.5.1 ISO_URLS

- The `ISO_URLS` specifies the download links for the ISO file.

- By default, it points to the `Ubuntu 24.04.3 LTS arm64 ISO` hosted on the official release site.

##### 4.2.5.2 IMAGE_SIZE

- The `IMAGE_SIZE` specifies the size of the image to be created.

- By default, it will be 16 GB (16000000000 bytes).

- **Important:** The image size must be at least 10 GB.

### 4.3 How To Install Ubuntu 24.04

- Press Enter on "Try or Install Ubuntu Server".

  ![Install Ubuntu 24.04](images_ubuntu/menu_grub_install_qemu.png)

- Press Enter on "Continue in rich mode".

  ![Choose mode installer](images_ubuntu/QEMU1_continue_in_rich_mode.png)

- Press Enter on "English".

  ![Select language](images_ubuntu/QEMU2_select_language.png)

- Select your keyboard configuration (e.g., "English (US)") and press "Done" to continue.

  ![Choose keyboard](images_ubuntu/QEMU3_keyboard_config.png)

- Use the default settings and press "Done" to continue.

  ![Select server](images_ubuntu/QEMU4_ubuntu_server_select.png)

- Use the default settings and press "Done" to continue.

  ![Network setting](images_ubuntu/QEMU5_network_config.png)

- Use the default settings and press "Done" to continue.

  ![Proxy setting](images_ubuntu/QEMU6_proxy_config.png)

- Wait for the mirror location to be tested (see below), then press "Done" to continue.

  ![Mirror setting](images_ubuntu/QEMU7_mirror_config.png)

- If the warning appears, press "Continue".

  ![Mirror warning](images_ubuntu/QEMU8_mirror_warning.png)

- Deselect "Set up this disk as an LVM group" and press "Done" to continue.

  ![Storage setting](images_ubuntu/QEMU9_storage_config.png)

- Press "Done" to confirm the partitions of the "/dev/vdb" disk.

  ![Storage2 setting](images_ubuntu/QEMU10_storage_config_2.png)

- Select "Continue" to finish partitioning and write changes to the "/dev/vdb" disk.

  ![Storage warn](images_ubuntu/QEMU11_warning.png)

- Enter username (e.g., rvc) and password for the administrator account, then press "Done".

  ![Profile setting](images_ubuntu/QEMU12_profile_config.png)

- Use the default settings and press "Continue".

  ![Skip ubuntu pro setting](images_ubuntu/QEMU13_skip_ubuntu_pro.png)

- Use the default settings and press "Done" to continue.

  ![Skip openssh setting](images_ubuntu/QEMU14_skip_install_OpenSSH_Server.png)

- Do not select any snap package. Press "Done" to continue.

  ![Skip snap setting](images_ubuntu/QEMU15_skip_feature_server_snaps.png)

- Wait for the installation to finish.

  ![Finish setting](images_ubuntu/QEMU16_waiting_install_ubuntu.png)

- Press "Reboot Now" to exit the installer.

  ![Network setting](images_ubuntu/QEMU17_install_ubuntu_sucsess.png)

- After logging in, press Ctrl-A X to exit the VM.

  ![Login success](images_ubuntu/QEMU18_login.png)

## 5. Common issues

### 5.1 System hangs while preparing Ubuntu install

- If the installation stays on a screen like the one below for more than 10 minutes after selecting `Try or Install Ubuntu Server`, the system may hang.

  ![Hang issue](images_ubuntu/hang_issue.png)

- For workaround, please increase the value of the `-smp` option in the `run_qemu.sh` script (default is `-smp 2`). Just make sure the new value does not exceed the number of CPU cores on your machine.

### 5.2 Unable to download ISO file

- If the installer fails to download the ISO file, it may show an error message:

  ![URL not exist](images_ubuntu/url_not_exist.png)

- For quick fix, find a valid URL for `ubuntu-24.04.3-live-server-arm64.iso` from the [Ubuntu old releases](https://old-releases.ubuntu.com/releases/noble/) or other trusted sources, then [update the value of ISO_URLS accordingly](#4251-iso_urls).

### 5.2 Missing bmap file causes slow write speed

- The installer uses .bmap file to accelerate the write speed when writing the image to external media. Without the .bmap file, it will fall back to nobmap mode, resulting in significantly slower write performance because the entire image must be written block by block.

  ![Write image with no bmap](images_ubuntu/write_image_with_no_bmap.png)

- When the image is compressed or transferred (e.g., uploaded to a server or shared with others), [its sparse regions (holes)](https://github.com/yoctoproject/bmaptool?tab=readme-ov-file#sparse-files) are filled with zeros. As a result, generating a new .bmap file afterward becomes meaningless and will no longer accelerate the write speed. To ensure fast writing and correct handling on other systems, always keep and distribute the image together with its corresponding .bmap file.
