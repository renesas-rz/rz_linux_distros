## Step 5: Write flash writer and bootloaders

The following tools are used in the instructions below. Please install them to your PC in advance.

=== "Windows PC"

    [Tera Term (terminal software)](https://github.com/TeraTermProject/teraterm/releases){: target=_blank }

    [FTDI VCP driver](https://www.ftdichip.com/Drivers/VCP.htm){: target=_blank }

=== "Linux PC"

    [minicom](https://help.ubuntu.com/community/Minicom){: target=_blank }


### 1. Setup EVK's peripheral

To write flash writer and bootloaders, you need to connect your PC to the EVK with a USB serial cable.

=== "RZ/G2L"

    ![](images/rzg2l_update-firmware.png)

=== "RZ/G3E"

    ![](images/rzg3e_update-firmware.png)

### 2. Set boot mode to SCIF download mode

=== "RZ/G2L"

    Turn off the power of EVK, then set the SW11 as follows.

    !!! content-wrapper no-indent table-no-sort table-no-hover ""
        ![](images/smarc-carrier-board-SW11_SCIF.png){ align=left .switch-icon }

        |     SW11-1     |     SW11-2     |    SW11-3    |     SW11-4     |   
        |:--------------:|:--------------:|:------------:|:--------------:|
        | OFF {: .red }  | ON {: .green } | OFF {: .red} | ON {: .green } |


=== "RZ/G3E"

    Turn off the power of EVK, then set the SW_MODE as follows.

    !!! content-wrapper no-indent table-no-sort table-no-hover ""
        ![](images/smarc-carrier2-board-SW_MODE_SCIF.jpg){ align=left .switch-icon }

        |      Pin1      |      Pin2      |     Pin3     |      Pin4      |   
        |:--------------:|:--------------:|:------------:|:--------------:|
        | OFF {: .red }  | ON {: .green } | OFF {: .red} | ON {: .green } |


### 3. Configure terminal software

Configure the setting of serial communication on terminal software as follows:

!!! content-wrapper no-indent table-no-sort table-no-hover ""

    |   Variable   |        Value          |
    |:------------:|:----------------------|
    | Baud rate    | `#!bash 115200` bps   |
    | Data         |`#!bash 8 bits`        |
    | Parity       | `#!bash none`         |
    | Stop         | `#!bash 1 bit`        |
    | Flow control | `#!bash none`         |

### 4. Prepare flash writer and bootloaders

The flash writer and the bootloaders are stored in the package.
Unzip the package and pick up flash write and bootloaders.

=== "RZ/G2L"
    * Flash writer
        * `#!bash Flash_Writer_SCIF_RZG2L_SMARC_PMIC_DDR4_2GB_1PCS.mot`

    * Bootloaders
        * `#!bash bl2_bp_spi-smarc-rzg2l_pmic.srec`
        * `#!bash fip-smarc-rzg2l_pmic.srec`

=== "RZ/G3E"
    * Flash writer
        * `#!bash Flash_Writer_SCIF_RZG3E_EVK_LPDDR4X.mot`

    * Bootloaders
        * `#!bash bl2_bp_spi-smarc-rzg3e.srec`
        * `#!bash fip-smarc-rzg3e.srec`


### 5. Download flash writer to RAM

=== "RZ/G2L"

    Turn on the power of the EVK by pressing SW9. The messages below are shown on the terminal.
    ``` console
    SCIF Download mode
    (C) Renesas Electronics Corp.
    -- Load Program to System RAM ---------------
    please send !
    ```

=== "RZ/G3E"

    Turn on the power of the EVK by pressing red button (POWER). The messages below are shown on the terminal.
    ``` console
    SCI Download mode (Normal SCI boot)

    -- Load Program to SRAM  ---------------

    ```

Send an image of `#!bash Flash Writer` using terminal software after the message **please send !** is shown.

=== "Windows PC (Tera Term)"

    Open the **File** menu, then select **Send file...** to open **Send file** dialog.

    Select the `#!bash Flash Writer` image in the dialog, and press **Open** button.

=== "Linux PC (minicom)"

    Press ++ctrl+a+s++, then select **ascii** for upload mode.

    Select the `#!bash Flash Writer` image file.

    Press any key when upload is completed.

After successfully downloading the image, the `#!bash Flash Writer` starts automatically and shows a message like below on the terminal.

=== "RZ/G2L"

    ``` console
    Flash writer for RZ/G2 Series V1.06 Aug.10,2022                                                    
    Product Code : RZ/G2L
    >
    ```

=== "RZ/G3E"
    ``` console
    -- Start Boot Program on SRAM ---------

    Flash writer for RZ/G3E Series V0.90 Aug.26,2024
    Product Code : RZ/G3E
    >
    ```


### 6. Write the bootloaders
=== "RZ/G2L"

    #### 6-1. Change baud rate of serial port

    Before writing the loader files, change the flash writer transfer rate from default (`#!bash 115200` bps) to high speed (`#!bash 921600` bps) with `#!bash SUP` command.

    ```bash
    SUP
    ```
    {: .diamond}

    After the `#!bash SUP` command, change the serial communication protocol speed from `#!bash 115200` bps to `#!bash 921600` bps on the terminal software.

    #### 6-2. Write bl2 file

    Execute `#!bash XLS2` command to write boot loader binary files.

    ```bash
    XLS2
    ```
    {: .diamond}

    This command receives binary data from the serial port and writes the data to a specified address of the Flash ROM with information where the data should be loaded on the address of the main memory.
    Set the following addresses respectively.

    * Address to load to RAM: `#!bash H'11E00`
    * Address to save to ROM: `#!bash H'00000`

    For example:

    ``` console
    >XLS2                                                                                              
    ===== Qspi writing of RZ/G2 Board Command =============                                            
    Load Program to Spiflash                                                                           
    Writes to any of SPI address.                                                                      
    Dialog : AT25QL128A                                                                               
    Program Top Address & Qspi Save Address                                                            
    ===== Please Input Program Top Address ============                                                
    Please Input : H'11E00                                                                           

    ===== Please Input Qspi Save Address ===                                                           
    Please Input : H'00000                                                                           
    Work RAM(H'50000000-H'53FFFFFF) Clear....                                                          
    please send ! ('.' & CR stop load)
    ```

    Send the data of `#!bash bl2_bp-spi-smarc-rzg2*.srec` from terminal software in the same way as the `#!bash Flash Writer` after the message **please send !** is shown.

    !!! note

        In the case that the following message is shown, enter ++y++.

        ``` console
        SPI Data Clear(H'FF) Check :H'00000000-0000FFFF,Clear OK?(y/n)
        ```

    After writing the data is completed, the following messages like below are shown on the terminal.

    ``` console
    SPI Data Clear(H'FF) Check :H'00000000-0000FFFF Erasing..Erase Completed
    SAVE SPI-FLASH.......
    ======= Qspi  Save Information  =================
    SpiFlashMemory Stat Address : H'00000000
    SpiFlashMemory End Address  : H'0000CA38
    ===========================================================

    >
    ```

    #### 6-3. Write fip file

    Execute `#!bash XLS2` command to write fip file.

    ```bash
    XLS2
    ```
    {: .diamond}

    Set the following addresses respectively for the fip file.

    * Address to load to RAM: `#!bash H'00000`
    * Address to save to ROM: `#!bash H'1D200`

    For example:

    ``` console
    >XLS2
    ===== Qspi writing of RZ/G2 Board Command =============
    Load Program to Spiflash
    Writes to any of SPI address.
    Dialog : AT25QL128A
    Program Top Address & Qspi Save Address
    ===== Please Input Program Top Address ============
    Please Input : H'00000

    ===== Please Input Qspi Save Address ===
    Please Input : H'1D200
    Work RAM(H'50000000-H'53FFFFFF) Clear....
    please send ! ('.' & CR stop load)
    ```

    Send the data of `#!bash fip-smarc-rzg2*.srec` from terminal software in the same way as the `#!bash Flash Writer` after the message **please send !** is shown.

    !!! note

        In the case that the following message is shown, enter ++y++.

        ``` console
        SPI Data Clear(H'FF) Check :H'00010000-000EFFFF,Clear OK?(y/n)
        ```

    After writing the data is completed, the following messages like below are shown on the terminal.

    ``` console
    SPI Data Clear(H'FF) Check :H'00010000-000EFFFF Erasing..............Erase Completed
    SAVE SPI-FLASH.......
    ======= Qspi  Save Information  =================
    SpiFlashMemory Stat Address : H'0001D200
    SpiFlashMemory End Address  : H'000DD9EF
    ===========================================================

    >
    ```

    #### 6-4. Restore baud rate of serial port

    After writing two loader files is completed, restore the serial communication protocol speed to `#!bash 115200` bps on the terminal software.

    Then, turn off the power of the board by pressing the SW9.

=== "RZ/G3E"
    #### 6-1. Change baud rate of serial port

    Before writing the loader files, change the flash writer transfer rate from default (`#!bash 115200` bps) to high speed (`#!bash 921600` bps) with `#!bash SUP` command.

    ```bash
    SUP
    ```
    {: .diamond}

    After the `#!bash SUP` command, change the serial communication protocol speed from `#!bash 115200` bps to `#!bash 921600` bps on the terminal software.

    #### 6-2. Write bl2 file

    Execute `#!bash XLS2` command to write boot loader binary files.

    ```bash
    XLS2
    ```
    {: .diamond}

    This command receives binary data from the serial port and writes the data to a specified address of the Flash ROM with information where the data should be loaded on the address of the main memory.
    Set the following addresses respectively.

    * Address to load to RAM: `#!bash H'8003600`
    * Address to save to ROM: `#!bash H'0000000`

    For example:
    ``` console
    >XLS2
    ===== Qspi writing of RZ/G3E Board Command =============
    Load Program to Spiflash
    Writes to any of SPI address.
     Dialog : AT25QL128A
    Program Top Address & Qspi Save Address
    ===== Please Input Program Top Address ============
      Please Input : H'8003600

    ===== Please Input Qspi Save Address ===
      Please Input : H'0
    please send ! ('.' & CR stop load)
    ```

    Send the data of `#!bash bl2_bp-spi-smarc-rzg3e.srec` from terminal software in the same way as the `#!bash Flash Writer` after the message **please send !** is shown.

    !!! note

        In the case that the following message is shown, enter ++y++.

        ``` console
        SPI Data Clear(H'FF) Check :H'00000000-0000FFFF,Clear OK?(y/n)
        ```

    After writing the data is completed, the following messages like below are shown on the terminal.
    ``` console
        Erase SPI Flash memory...
        Erase Completed
        Write to SPI Flash memory.
        ======= Qspi  Save Information  =================
        SpiFlashMemory Stat Address : H'00000000
        SpiFlashMemory End Address  : H'00021170
        ===========================================================
        >
    ```

    #### 6-3. Write fip file

    Execute `#!bash XLS2` command to write fip file.

    ```bash
    XLS2
    ```
    {: .diamond}

    Set the following addresses respectively for the fip file.

    * Address to load to RAM: `#!bash H'00000`
    * Address to save to ROM: `#!bash H'60000`

    For example:

    ``` console
        >XLS2
        ===== Qspi writing of RZ/G3E Board Command =============
        Load Program to Spiflash
        Writes to any of SPI address.
        Dialog : AT25QL128A
        Program Top Address & Qspi Save Address
        ===== Please Input Program Top Address ============
            Please Input : H'0

        ===== Please Input Qspi Save Address ===
            Please Input : H'60000
        please send ! ('.' & CR stop load)
    ```

    Send the data of `#!bash fip-smarc-rzg3e.srec` from terminal software in the same way as the `#!bash Flash Writer` after the message **please send !** is shown.

    !!! note

        In the case that the following message is shown, enter ++y++.

        ``` console
        SPI Data Clear(H'FF) Check :H'00010000-000EFFFF,Clear OK?(y/n)
        ```

    After writing the data is completed, the following messages like below are shown on the terminal.

    ``` console
        Erase SPI Flash memory...
        Erase Completed
        Write to SPI Flash memory.
        ======= Qspi  Save Information  =================
        SpiFlashMemory Stat Address : H'00060000
        SpiFlashMemory End Address  : H'0014867E
        ===========================================================
    ```

    #### 6-4. Restore baud rate of serial port

    After writing two loader files is completed, restore the serial communication protocol speed to `#!bash 115200` bps on the terminal software.

    Then, turn off the power of the board by pressing the blue button (RESET).
