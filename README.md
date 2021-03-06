# rtl8821CU

Drivers for rtl8811CU and rtl8821CU Wi-Fi chipsets forked from [whitebatman2/rtl8821CU](https://github.com/whitebatman2/rtl8821CU). 


This repository is based on soruce code found on a CD shipped with a rtl8811CU based card. It's updated to build on newer kernel versions. A simple line was added to allow this driver to work with the latest hardware revision of the DWA-171 C1 wifi dongle!

## Mind the architecture: i386 or ARM?
You might need to set correct the platform in the Makefile.

For Intel, change it to:
```
CONFIG_PLATFORM_I386_PC = y
CONFIG_PLATFORM_ARM_RPI = n
CONFIG_PLATFORM_ARM_RPI3 = n
```
For ARMv7:
```
CONFIG_PLATFORM_I386_PC = n
CONFIG_PLATFORM_ARM_RPI = y
CONFIG_PLATFORM_ARM_RPI3 = n
```
For ARMv8:
```
CONFIGx_PLATFORM_I386_PC = n
CONFIG_PLATFORM_ARM_RPI = n
CONFIG_PLATFORM_ARM_RPI3 = y
```

## Build and install without DKMS
Use following commands in source directory:
```
make
sudo make install
sudo modprobe 8821cu
```
## Build and install with DKMS

DKMS is a system which will automatically recompile and install a kernel module when a new kernel gets installed or updated. To make use of DKMS, install the dkms package, which on Debian (based) systems is done like this:

    apt-get install dkms

To make use of the DKMS feature with this project, do the following:

    DRV_NAME=rtl8821CU
    DRV_VERSION=5.2.5.3
    sudo mkdir /usr/src/${DRV_NAME}-${DRV_VERSION}
    git archive master | sudo tar -x -C /usr/src/${DRV_NAME}-${DRV_VERSION}
    sudo dkms add -m ${DRV_NAME} -v ${DRV_VERSION}
    sudo dkms build -m ${DRV_NAME} -v ${DRV_VERSION}
    sudo dkms install -m ${DRV_NAME} -v ${DRV_VERSION}

If you later on want to remove it again, do the following:

    DRV_NAME=rtl8821CU
    DRV_VERSION=5.2.5.3
    sudo dkms remove ${DRV_NAME}/${DRV_VERSION} --all
    
## Final step: add this script!!!
The USB Wifi Dongle has a small storage partition with the Windows drivers inside that gets mounted before the wifi adapter itself. Follow these steps to ensure the Wi-Fi adapter is mounted with the stick is inserted!

```
chmod +x automountDWA171.sh
sudo ./automountDWA171.sh
```
You're done, you can plug the dongle!

