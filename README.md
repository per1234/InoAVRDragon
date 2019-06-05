InoAVRDragon
==========

A **workaround** for [Arduino/#650](https://github.com/arduino/Arduino/issues/650) as it occurs for the AVR Dragon ISP programmer. Some people are getting the error message: `avrdude: failed to sync with the AVR Dragon in ISP mode` when trying to **Burn Bootloader** using the **AVR Dragon** programmer using the [Arduino](http://arduino.cc) IDE. The problem is that the Arduino IDE's standard **Burn Bootloader** process is done in two avrdude commands, which are sent without enough delay between them.

This **workaround** adds **AVR Dragon (workaround)**, **AVR Dragon HVSP mode (workaround)**, and **AVR Dragon PP mode (workaround)** programmers to the **Tools > Programmer** menu. This programmer definition causes the **erase** and **bootloader** steps to be done in the first avrdude command so burning the bootloader is accomplished successfully even though the second avrdude command fails as usual.


## Comparison With Alternative **Workaround**
Another **workaround** has been presented in the Arduino Forum by dmjlambert: http://forum.arduino.cc/index.php?topic=345838 here is a comparison of InoAVRDragon vs the dmjlambert **workaround**:
### Pros
- Doesn't require modification of any Arduino or other hardware core files.
- Doesn't need to be redone every time the Arduino IDE or any hardware core with a platform.txt is updated.
- Doesn't affect any programmer except for AVR Dragon.

### Cons
- **Burn Bootloader** appears to fail even when successful.
- **Burn Bootloader** process does not follow ideal command order and does not set the lock bits.


## Burn Bootloader Process
Unfortunately, this workaround causes the **Burn Bootloader** process to work differently than normal.
### Normal Burn Bootloader
1. Perform chip erase
1. Set lock bits according to `{bootloader.unlock_bits}`
1. Set efuse
1. Set hfuse
1. Set lfuse
1. Flash bootloader .hex file
1. Set lock bits according to `{bootloader.lock_bits}`

### InoAVRDragon Burn Bootloader
1. Perform chip erase
1. Flash bootloader .hex file
1. Set lock bits according to `{bootloader.unlock_bits}`
1. Set efuse
1. Set hfuse
1. Set lfuse


## Installation
There are two options for installing InoAVRDragon:
### Boards Manager
This installation method requires Arduino IDE version 1.6.4 or newer.
1. Start the Arduino IDE.
1. **File > Preferences**
1. Enter the following URL in **Additional Boards Manager URLs**: https://per1234.github.io/InoAVRDragon/package_per1234_InoAVRDragon_index.json
    - If you already have a URL in that field, separate the URLs with a comma.
1. Click "OK".
1. **Tools > Board > Boards Manager...**
1. Wait for the downloads to finish.
1. Scroll down until you see **InoAVRDragon by per1234**. Click on it.
1. Click **Install**.
1. Wait for installation to finish.
1. Click **Close**.

Notes:
- If you are using Arduino IDE 1.6.6 then you may need to close **Boards Manager** and then reopen it before the **AVR Dragon (workaround)** entry will appear.
- If using Arduino IDE older that v1.6.6 you may need to restart the IDE to make the **AVR Dragon (workaround)** programmer appear in the **Tools > Programmer** menu.

### Manual Installation
1. Download InoAVRDragon from one of the **Source code** links of the newest release at: https://github.com/per1234/InoAVRDragon/releases.
1. Extract the downloaded file.
1. Copy the extracted folder inside your sketchbook/hardware folder. You can find the location of the sketchbook folder in the Arduino IDE at **File > Preferences > Sketchbook Location**
1. If the Arduino IDE is running, then restart it.


## Usage
### Burn Bootloader
1. Connect the AVR Dragon to your Arduino.
1. Select **Tools > Programmer > AVR Dragon (workaround)**.
1. Select the correct board from **Tools > Board**.
1. Click **Tools > Burn Bootloader**.
1. The bootloader will be burned to your Arduino but then the avrdude output will fail with the error message `avrdude: usbdev_open(): did not find any USB device "usb"`. This is because the second avrdude command failed, however if you enable **File > Preferences > Show verbose output during: upload** (check) and examine the avrdude output fully, you will see that the first command was successful and the bootloader was burned in that command.

### Upload Using Programmer
The **Upload Using Programmer** process should work as normal when using the InoAVRDragon programmers and complete without errors.


## Contributing
Pull requests or issue reports are welcome! Please see the [contribution rules](https://github.com/per1234/InoAVRDragon/blob/master/.github/CONTRIBUTING.md) for instructions.
