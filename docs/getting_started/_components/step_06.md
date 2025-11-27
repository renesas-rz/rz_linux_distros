## Step 6: Run Ubuntu / Debian on RZ EVK

### 1. Setup EVK's peripheral

Before you start Ubuntu / Debian on RZ EVK, you need to set up the EVK.

=== "RZ/G2L"

    ![](images/rzg2l_sd-boot.png)

=== "RZ/G3E"

    ![](images/rzg3e_sd-boot.png)

### 2. Setup EVK's DIP switch

=== "RZ/G2L"

    Set up DIP switch SW1 and SW11 as follows.

    * SW1

        !!! content-wrapper no-indent table-no-sort table-no-hover ""

            ![](images/smarc-rzg2l-board-SW1.png){ align=left .switch-icon }

            |      SW1-1     |      SW1-2     |
            |:--------------:|:--------------:|
            | ON {: .green } | OFF {: .red }  |

    * SW11

        !!! content-wrapper no-indent table-no-sort table-no-hover ""

            ![](images/smarc-carrier-board-SW11_QSPI.png){ align=left .switch-icon }

            |     SW11-1     |     SW11-2     |    SW11-3    |     SW11-4     |
            |:--------------:|:--------------:|:------------:|:--------------:|
            | OFF {: .red }  | OFF {: .red }  | OFF {: .red} | ON {: .green } |

=== "RZ/G3E"

    Set up DIP switch SW1 and SW11 as follows.

    * SW_MODE

        !!! content-wrapper no-indent table-no-sort table-no-hover ""

            ![](images/smarc-carrier2-board-SW_MODE_QSPI.jpg){ align=left .switch-icon }

            |     Pin1     |     Pin2     |    Pin3    |     Pin4     |
            |:--------------:|:--------------:|:------------:|:--------------:|
            | OFF {: .red }  | OFF {: .red }  | OFF {: .red} | ON {: .green } |


### 3. Setup u-boot

=== "RZ/G2L"
    Turn on the power of the EVK by pressing SW9, and press Enter key within a few seconds to interrupt the boot process.
    If it succeeds, you will see the following messages:

    ``` console
    NOTICE:  BL2: v2.9(release):v2.5/rzg2l-1.00-3892-gcc1869562
    NOTICE:  BL2: Built : 16:36:45, Jul  2 2025
    NOTICE:  BL2: Booting BL31
    NOTICE:  BL31: v2.9(release):v2.5/rzg2l-1.00-3892-gcc1869562
    NOTICE:  BL31: Built : 16:36:48, Jul  2 2025


    U-Boot 2021.10-gb105c304da (Jul 02 2025 - 16:35:42 +0700)

    CPU:   Renesas Electronics CPU rev 1.0
    Model: smarc-rzg2l
    DRAM:  1.9 GiB
    SF: Detected mt25qu512a with page size 256 Bytes, erase size 64 KiB, total 64 MiB
    WDT:   watchdog@0000000012800800
    WDT:   Started with servicing (60s timeout)
    MMC:   sd@11c00000: 0, sd@11c10000: 1
    Loading Environment from MMC... OK
    In:    serial@1004b800
    Out:   serial@1004b800
    Err:   serial@1004b800
    U-boot WDT started!
    Net:
    Error: ethernet@11c20000 address not set.

    Error: ethernet@11c30000 address not set.
    No ethernet found.

    Hit any key to stop autoboot:  0
    =>
    =>
    ```

    Configure U-Boot environment variables to boot from a microSD card as follows:

    === "Ubuntu"

        ```bash
        setenv bootcmd 'load mmc 1:1 0x48080000 efi/boot/bootaa64.efi; bootefi 0x48080000'
        saveenv
        ```
        {: .diamond2}

        Load and boot _"bootaa64.efi"_ from EFI partition 1 of the microSD card (CN10).
        If the EFI partition resides on a different partition, such as partition 2, please modify _"1:1"_ to _"1:2"_.

    === "Debian"

        ```bash
        setenv bootcmd 'load mmc 1:2 0x48080000 efi/boot/bootaa64.efi; bootefi 0x48080000'
        saveenv
        ```
        {: .diamond2}

        Load and boot _"bootaa64.efi"_ from EFI partition 2 of the microSD card (CN10).
        If the EFI partition resides on a different partition, such as partition 1, please modify _"1:2"_ to _"1:1"_.

    The RZ/G2L EVK does not have a MAC address, so you need to set appropriate MAC addresses.
    Set the Ethernet MAC addresses on U-boot as follows:

    ``` bash
    setenv ethaddr XX:XX:XX:XX:XX:XX
    setenv eth1addr XX:XX:XX:XX:XX:XX
    saveenv
    ```
    {: .diamond2 }

=== "RZ/G3E"
    Turn on the power of the EVK by pressing red button (POWER), and press Enter key within a few seconds to interrupt the boot process.
    If it succeeds, you will see the following messages:

    ``` console
    NOTICE:  BL2: v2.10.5(release):2.10.5/rzg3e_1.2.0
    NOTICE:  BL2: Built : 11:09:15, Sep 22 2025
    NOTICE:  BL2: SYS_LSI_MODE: 0x13e06
    NOTICE:  BL2: SYS_LSI_DEVID: 0x8679447
    NOTICE:  BL2: SYS_LSI_PRR: 0x0
    NOTICE:  BL2: Booting BL31
    NOTICE:  BL31: v2.10.5(release):2.10.5/rzg3e_1.2.0
    NOTICE:  BL31: Built : 11:09:19, Sep 22 2025


    U-Boot 2024.07-gdeef73d62a (Sep 22 2025 - 11:06:23 +0700)

    CPU:   Renesas Electronics CPU rev 1.0
    Model: Renesas SMARC EVK based on r9a09g047e57
    DRAM:  3.9 GiB
    Core:  45 devices, 18 uclasses, devicetree: separate
    MMC:   mmc@15c00000: 0, mmc@15c10000: 1, mmc@15c20000: 2
    Loading Environment from MMC... Reading from MMC(0)... OK
    In:    serial@11c01400
    Out:   serial@11c01400
    Err:   serial@11c01400
    Net:
    Error: ethernet@15C40000 No valid MAC address found.

    Error: ethernet@15C30000 No valid MAC address found.

    Error: ethernet@15C30000 No valid MAC address found.

    Error: ethernet@15C40000 No valid MAC address found.
    No ethernet found.

    Hit any key to stop autoboot:  0
    =>
    =>
    ```

    Configure U-Boot environment variables to boot from a microSD card as follows:

    === "Ubuntu"

        ```bash
        setenv bootcmd 'load mmc 1:1 0x48080000 efi/boot/bootaa64.efi; bootefi 0x48080000'
        saveenv
        ```
        {: .diamond2}

        Load and boot _"bootaa64.efi"_ from EFI partition 1 of the microSD card (CN10).
        If the EFI partition resides on a different partition, such as partition 2, please modify _"1:1"_ to _"1:2"_.

    === "Debian"

        ```bash
        setenv bootcmd 'load mmc 1:2 0x48080000 efi/boot/bootaa64.efi; bootefi 0x48080000'
        saveenv
        ```
        {: .diamond2}

        Load and boot _"bootaa64.efi"_ from EFI partition 2 of the microSD card (CN10).
        If the EFI partition resides on a different partition, such as partition 1, please modify _"1:2"_ to _"1:1"_.

!!! success "Tip"

    If you would like to restore default environment variables in u-boot, run the following command:

    ``` bash
    env default -a
    saveenv
    ```
    {: .diamond2 }


Press the RESET button (SW10) to start Ubuntu / Debian.

### 4. Configure network

When you turn on the power of the EVK or press the RESET button, you will see **GNU GRUB** menu.
Select **Linux-renesas-6.1** in the menu, and press ++enter++.

When you run Ubuntu / Debian for the first time on the environment, configure network so that you can install packages with `#!bash apt`.

=== "Ubuntu"

    Edit "/etc/netplan/50-cloud-init.yaml" as follows:

    ```bash
    network:
      version: 2
      renderer: networkd
      ethernets:
        end0:
          dhcp4: true
    ```

    Then, execute the following command to apply the settings.
    ```bash
    sudo netplan apply
    ```
    {: .dollar }

=== "Debian"

    Edit "/etc/network/interfaces" as follows:

    ```bash
    allow-hotplug end0
    iface end0 inet dhcp
    ```

    Then, execute the following command as root to apply the settings.
    ```bash
    systemctl restart networking
    ```
    {: .hash }

### 5. Run Ubuntu / Debian on EVK

Turn on the power of the EVK or press the RESET button.
Select **Linux-renesas-6.1** in the **GNU GRUB** menu, and press ++enter++ to run Ubuntu / Debian on the EVK.
