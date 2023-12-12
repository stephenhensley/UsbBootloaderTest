# UsbBootlloaderTest

This is a simple project for testing the v6 Daisy Bootloader's various configurations in an isolated environment.

There are four variants of the bootloader available, and the project is cross compatible with any version.

The equivalent configuration to previous versions of the bootloader, is: `dsy_bootloader_v6_2-intdfu-2000ms.bin`

This is the version that will be the default when running `make program-boot` from a libDaisy project.

## Customization and Setting

There are two things that you can tweak in the main.cpp file:

The `blink_rate` variable can be used to check that changes are occurring as expected.

The method of jumping to the bootloader can be changed, or mapped to any other pin.
For simplicity's sake, this has been set to D27 (the pin connected to SW 1 on the Daisy Pod).

This default setting expects the pin to be floating by default, and connected through a switch to GND.
On a falling edge (button release), the application will reset to the bootloader.

## Bootloader Variants

The variants can be found in the `bootloader/` directory and the list of files is here, with details below:

* dsy_bootloader_v6_2-extdfu-10ms.bin - external DFU, 10ms boot timeout
* dsy_bootloader_v6_2-extdfu-2000ms.bin - external DFU, 2000ms boot timeout
* dsy_bootloader_v6_2-intdfu-10ms.bin - internal DFU, 10ms boot timeout
* dsy_bootloader_v6_2-intdfu-2000ms.bin - internal DFU, 2000ms boot timeout

**Note**: When using the EXTDFU variants, the internal DFU port becomes nonfunctional.
In the event that you can not update the Daisy application in the extdfu variant, you can update the bootloader to one of the intdfu variants without requiring any change to the application build.

### DFU port

Variants with the `-intdfu` suffix are configured the same as previous versions of the bootloader.
In this configuration, the internal USB port is initialized for DFU firmware updates, the external USB port is configured as a USB host, and checks for connected media drives, and the SDMMC is initilaized to firmware update when a .bin file is present on the USB drive.

Variants with the `-extdfu` suffix are configured with the external USB port initialized for USB DFU. The internal USB port is inaccessible in this configuration. The SDMMC works the same, and checks for update bin files.

### Timeout

To increase boot times on finished projects, and applications where the user manually enters an update mode, there is now a shorter boot time variant.

This is the additional timeout where the bootloader will wait for interaction prior to resetting into the application, not the exact time it takes to boot.
Things like the presence of media to scan, etc. will still increase boottime regardless of timeout setting.

Options are 10ms and 2000ms

