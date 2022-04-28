# meshdesk-firmware
Firmware for Meshdesk

## Instructions for Ubiquity device


### GRAB all the firmware

- wget https://downloads.openwrt.org/releases/21.02.0/targets/ath79/generic/packages/mtd_26_mips_24kc.ipk
- wget https://downloads.openwrt.org/releases/21.02.0/targets/ath79/generic/packages/libc_1.1.24-3_mips_24kc.ipk
- wget https://downloads.openwrt.org/releases/21.02.0/packages/mips_24kc/base/libubox20210516_2021-05-16-b14c4688-2_mips_24kc.ipk

### Download the Meshdesk image
- git clone https://github.com/iNethi/meshdesk-firmware.git


### Power up Ubiquity mesh AC lite
- Power up device
- Plug ethernet from you PC into LAN of Ubiquity POE


### Flashing nodes

- scp ./ubnt-ac-mesh/openwrt*squashfs-sysupgrade.bin ubnt@192.168.1.20:/tmp/
- scp *ipk ubnt@192.168.1.20:/tmp/


### Now SSH onto the device and extract the packages into /tmp/flash directory.
- ssh ubnt@192.168.1.20
- pass: ubnt
- mkdir /tmp/flash
- cd /tmp/flash
- tar -xzOf /tmp/libc_1.1.24-3_mips_24kc.ipk  ./data.tar.gz | tar -xvz
- tar -xzOf /tmp/mtd_26_mips_24kc.ipk ./data.tar.gz | tar -xvz
- tar -xzOf /tmp/libubox20210516_2021-05-16-b14c4688-2_mips_24kc.ipk ./data.tar.gz | tar -xvz


- LD_LIBRARY_PATH=${PWD}/lib lib/ld-musl-mips-sf.so.1 sbin/mtd write /tmp/openwrt-ath79-generic-ubnt_unifiac-mesh-squashfs-sysupgrade.bin kernel0

### Erase kernel 1
- LD_LIBRARY_PATH=${PWD}/lib lib/ld-musl-mips-sf.so.1 sbin/mtd erase kernel1

### Ensure boot loader set to kernel0
- dd if=/dev/zero bs=1 count=1 of=/dev/mtd4

### Power cycle
- Turn power off and on again and Meshdesk should be available
