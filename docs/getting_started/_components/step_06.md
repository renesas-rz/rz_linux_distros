## Step 6: Run Ubuntu / Debian on RZ EVK

### 1. Setup EVK's peripheral

Before you start Ubuntu / Debian on RZ EVK, you need to set up the EVK.

=== "RZ/G2L"

    ![](images/rzg2l_sd-boot.png)

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


### 3. Setup u-boot

Turn on the power of the EVK by pressing SW9, and press Enter key within a few seconds to interrupt the boot process.
If it succeeds, you will see the following messages:

``` console
NOTICE:  BL2: v2.9(release):v2.5/rzg2l-1.00-3892-gcc1869562
NOTICE:  BL2: Built : 20:49:37, Jun 22 2025
NOTICE:  BL2: Booting BL31
NOTICE:  BL31: v2.9(release):v2.5/rzg2l-1.00-3892-gcc1869562
NOTICE:  BL31: Built : 20:49:38, Jun 22 2025


U-Boot 2021.10-g5141064c15 (Jun 22 2025 - 20:48:27 +0700)

CPU:   Renesas Electronics CPU rev 1.0
Model: smarc-rzg2l
DRAM:  1.9 GiB
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

    U-Boot will load _"efi/boot/bootaa64.efi"_ from EFI partition **(default: partition 1)** of the microSD card.
    If the EFI partition resides on a different partition, such as partition 2, please modify _"1:1"_ to _"1:2"_.

=== "Debian"

    ```bash
    setenv bootcmd 'load mmc 1:2 0x48080000 efi/boot/bootaa64.efi; bootefi 0x48080000'
    saveenv
    ```
    {: .diamond2}

    U-Boot will load _"efi/boot/bootaa64.efi"_ from EFI partition **(default: partition 2)** of the microSD card.
    If the EFI partition resides on a different partition, such as partition 1, please modify _"1:2"_ to _"1:1"_.

!!! note

    The RZ/G2L EVK does not have a MAC address, so you need to set an appropriate MAC address when you use ethernet.
    You can set the address on U-boot as follows:

    * For eth0,
    ``` bash
    setenv ethaddr XX:XX:XX:XX:XX:XX
    saveenv
    ```
    {: .diamond2 }

    * For eth1,
    ``` bash
    setenv eth1addr XX:XX:XX:XX:XX:XX
    saveenv
    ```
    {: .diamond2 }

    To restore default environment variables in u-boot, run the following command:

    ``` bash
    env default -a
    ```
    {: .diamond2 }


Press the RESET button (SW10) to start Ubuntu / Debian.

### 3. Configure network

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

### 4. Run Ubuntu / Debian on EVK

Turn on the power of the EVK or press the RESET button.
Select **Linux-renesas-6.1** in the **GNU GRUB** menu, and press ++enter++ to run Ubuntu / Debian on the EVK.
