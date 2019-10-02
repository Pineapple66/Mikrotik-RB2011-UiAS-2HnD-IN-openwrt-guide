# Mikrotik-RB2011-UiAS-2HnD-IN-OpenWRT-Installation-guide
This is a guide that helps you to install OpenWRT into a Mikrotik device
We are mainly focus on this guide  https://openwrt.org/toh/mikrotik/rb2011

We can also use this guide as reference
```
https://openwrt.org/toh/mikrotik/rb2011uias         this guide is a lot harder, might be older? Not sure
```

**1. Setup DHCP Server with TFTP Boot.**
Set your PC to static IP of 192.168.1.1/24. Note that the initramfs file should be in the directory indicated by the command.

```
   sudo dnsmasq -i eth0 --dhcp-range=192.168.1.100,192.168.1.200 \
   --dhcp-boot=openwrt-ar71xx-mikrotik-vmlinux-initramfs.elf \
   --enable-tftp --tftp-root=~/directory/where/file/is/ \
   -d -u $USER -p0 -K --log-dhcp --bootp-dynamic
```
Where eth0 is your NIC card name, in my case was eno1. Once you have download the file from this repo, you need to rename the file to openwrt-ar71xx-mikrotik-vmlinux-initramfs.elf. 
If your tftp-root folder is in /, use /tftpboot/ 

This is a long one line code, please input them at once. 

**2. Connect PC to ethernet port 1.**

**3. Boot from DHCP.**
Power off router, and hold reset while powering on. Wait 20 seconds until you see PC send file. Release reset, and remain connected to ethernet port until you see Router report Openwrt hostname.
On the dhcp long command window, you will see something like this 
```
orange@orange-Latitude-E6540:~/Desktop$ sudo dnsmasq -i eno1 --dhcp-range=192.168.1.100,192.168.1.200 --dhcp-boot=openwrt-ar71xx-mikrotik-vmlinux-initramfs.elf --enable-tftp --tftp-root=/tftpboot/ -d -u $USER -p0 -K --log-dhcp --bootp-dynamic
dnsmasq: started, version 2.80 DNS disabled
dnsmasq: compile time options: IPv6 GNU-getopt DBus i18n IDN DHCP DHCPv6 no-Lua TFTP conntrack ipset auth DNSSEC loop-detect inotify dumpfile
dnsmasq: warning: interface eno1 does not currently exist
dnsmasq-dhcp: DHCP, IP range 192.168.1.100 -- 192.168.1.200, lease time 1h
dnsmasq-tftp: TFTP root is /tftpboot/ 
dnsmasq-dhcp: 1522824632 available DHCP range: 192.168.1.100 -- 192.168.1.200
dnsmasq-dhcp: 1522824632 vendor class: Mips_boot
dnsmasq-dhcp: 1522824632 DHCPDISCOVER(eno1) 4c:5e:0c:c7:10:60 
dnsmasq-dhcp: 1522824632 tags: eno1
dnsmasq-dhcp: 1522824632 DHCPOFFER(eno1) 192.168.1.132 4c:5e:0c:c7:10:60 
dnsmasq-dhcp: 1522824632 requested options: 1:netmask, 3:router
dnsmasq-dhcp: 1522824632 bootfile name: openwrt-ar71xx-mikrotik-vmlinux-initramfs.elf
dnsmasq-dhcp: 1522824632 next server: 192.168.1.1
dnsmasq-dhcp: 1522824632 sent size:  1 option: 53 message-type  2
dnsmasq-dhcp: 1522824632 sent size:  4 option: 54 server-identifier  192.168.1.1
dnsmasq-dhcp: 1522824632 sent size:  4 option: 51 lease-time  1h
dnsmasq-dhcp: 1522824632 sent size:  4 option: 58 T1  30m
dnsmasq-dhcp: 1522824632 sent size:  4 option: 59 T2  52m30s
dnsmasq-dhcp: 1522824632 sent size:  4 option:  1 netmask  255.255.255.0
dnsmasq-dhcp: 1522824632 sent size:  4 option: 28 broadcast  192.168.1.255
dnsmasq-dhcp: 1522824632 sent size:  4 option:  3 router  192.168.1.1
dnsmasq-dhcp: 2966300407 available DHCP range: 192.168.1.100 -- 192.168.1.200
dnsmasq-dhcp: 2966300407 vendor class: Mips_boot
dnsmasq-dhcp: 2966300407 DHCPDISCOVER(eno1) 4c:5e:0c:c7:10:60 
dnsmasq-dhcp: 2966300407 tags: eno1
dnsmasq-dhcp: 2966300407 DHCPOFFER(eno1) 192.168.1.132 4c:5e:0c:c7:10:60 
dnsmasq-dhcp: 2966300407 requested options: 1:netmask, 3:router
dnsmasq-dhcp: 2966300407 bootfile name: openwrt-ar71xx-mikrotik-vmlinux-initramfs.elf
dnsmasq-dhcp: 2966300407 next server: 192.168.1.1
dnsmasq-dhcp: 2966300407 sent size:  1 option: 53 message-type  2
dnsmasq-dhcp: 2966300407 sent size:  4 option: 54 server-identifier  192.168.1.1
```
On the serial port command window, you will see something like this 

```
RouterBOOT booter 3.18

RouterBoard 2011UiAS-2HnD

CPU frequency: 750 MHz
 Memory speed: 200 MHz
  Memory size: 128 MiB
    NAND size: 128 MiB

Press any key within 2 seconds to enter setuptrying dhcp protocol..... OK
resolved mac address EC:F4:BB:08:00:78
Gateway: 192.168.1.20
transfer started ............................ transfer ok, time=1.78s
setting up elf image... OK
jumping to kernel code


OpenWrt kernel loader for AR7XXX/AR9XXX
Copyright (C) 2011 Gabor Juhos <juhosg@openwrt.org>
Decompressing kernel... done!
Starting kernel at 80060000...

[    0.000000] Linux version 4.9.184 (buildbot@c2c312384f99) (gcc version 7.3.0 (OpenWrt GCC 7.3.0 r7808-ef686b7292) ) #0 Thu Jun 27 12:18:52 2019
[    0.000000] bootconsole [early0] enabled
[    0.000000] CPU0 revision is: 0001974c (MIPS 74Kc)
[    0.000000] SoC: Atheros AR9344 rev 3
[    0.000000] Determined physical RAM map:
[    0.000000]  memory: 08000000 @ 00000000 (usable)
[    0.000000] User-defined physical RAM map:
[    0.000000]  memory: 08000000 @ 00000000 (usable)
[    0.000000] Initrd not found or empty - disabling initrd
[    0.000000] Primary instruction cache 64kB, VIPT, 4-way, linesize 32 bytes.
[    0.000000] Primary data cache 32kB, 4-way, VIPT, cache aliases, linesize 32 bytes
[    0.000000] Zone ranges:
[    0.000000]   Normal   [mem 0x0000000000000000-0x0000000007ffffff]
[    0.000000] Movable zone start for each node
[    0.000000] Early memory node ranges
[    0.000000]   node   0: [mem 0x0000000000000000-0x0000000007ffffff]
[    0.000000] Initmem setup node 0 [mem 0x0000000000000000-0x0000000007ffffff]
[    0.000000] Built 1 zonelists in Zone order, mobility grouping on.  Total pages: 32512
[    0.000000] Kernel command line: parts=1 boot_part_size=4194304 gpio=249387 HZ=375000000 mem=128M kmac=4C:5E:0C:C7:10:60 board=2011r5 ver=3.18 boot=1 mlc=6 console=ttyS0,115200 rootfstype=squashfs noinitrd
[    0.000000] PID hash table entries: 512 (order: -1, 2048 bytes)
[    0.000000] Dentry cache hash table entries: 16384 (order: 4, 65536 bytes)
[    0.000000] Inode-cache hash table entries: 8192 (order: 3, 32768 bytes)
[    0.000000] Writing ErrCtl register=00000000
[    0.000000] Readback ErrCtl register=00000000
[    0.000000] Memory: 122700K/131072K available (3608K kernel code, 163K rwdata, 456K rodata, 2356K init, 209K bss, 8372K reserved, 0K cma-reserved)
[    0.000000] SLUB: HWalign=32, Order=0-3, MinObjects=0, CPUs=1, Nodes=1
[    0.000000] NR_IRQS:51
[    0.000000] Clocks: CPU:750.000MHz, DDR:400.000MHz, AHB:400.000MHz, Ref:25.000MHz
[    0.000000] clocksource: MIPS: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 5096694524 ns
[    0.000007] sched_clock: 32 bits at 375MHz, resolution 2ns, wraps every 5726622718ns
[    0.008858] Calibrating delay loop... 373.55 BogoMIPS (lpj=1867776)
[    0.071926] pid_max: default: 32768 minimum: 301
[    0.077258] Mount-cache hash table entries: 1024 (order: 0, 4096 bytes)
[    0.084772] Mountpoint-cache hash table entries: 1024 (order: 0, 4096 bytes)
[    0.095370] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 19112604462750000 ns
[    0.106554] futex hash table entries: 256 (order: -1, 3072 bytes)
[    0.114583] NET: Registered protocol family 16
[    0.124158] MIPS: machine is Mikrotik RouterBOARD 2011UiAS-2HnD
[    0.628493] clocksource: Switched to clocksource MIPS
[    0.635210] NET: Registered protocol family 2
[    0.641001] TCP established hash table entries: 1024 (order: 0, 4096 bytes)
[    0.648962] TCP bind hash table entries: 1024 (order: 0, 4096 bytes)
[    0.656180] TCP: Hash tables configured (established 1024 bind 1024)
[    0.663483] UDP hash table entries: 256 (order: 0, 4096 bytes)
[    0.670149] UDP-Lite hash table entries: 256 (order: 0, 4096 bytes)
[    0.677483] NET: Registered protocol family 1
[    2.568505] random: fast init done
[    2.851652] Crashlog allocated RAM at address 0x3f00000
[    2.858992] workingset: timestamp_bits=30 max_order=15 bucket_order=0
[    2.872084] squashfs: version 4.0 (2009/01/31) Phillip Lougher
[    2.878738] jffs2: version 2.2 (NAND) (SUMMARY) (LZMA) (RTIME) (CMODE_PRIORITY) (c) 2001-2006 Red Hat, Inc.
[    2.968431] io scheduler noop registered
[    2.972921] io scheduler deadline registered (default)
[    2.979118] Serial: 8250/16550 driver, 16 ports, IRQ sharing enabled
[    2.988668] console [ttyS0] disabled
[    3.012799] serial8250.0: ttyS0 at MMIO 0x18020000 (irq = 11, base_baud = 1562500) is a 16550A
[    3.022603] console [ttyS0] enabled
[    3.022603] console [ttyS0] enabled
[    3.030160] bootconsole [early0] disabled
[    3.030160] bootconsole [early0] disabled
[    3.044300] m25p80 spi0.0: found is25cd512, expected m25p80
[    3.050121] m25p80 spi0.0: is25cd512 (64 Kbytes)
[    3.090503] Creating 4 MTD partitions on "spi0.0":
[    3.095467] 0x000000000000-0x00000000c000 : "routerboot"
[    3.120168] 0x00000000c000-0x00000000d000 : "hard_config"
[    3.144918] 0x00000000d000-0x00000000e000 : "bios"
[    3.169095] 0x00000000e000-0x00000000f000 : "soft_config"
[    3.194918] nand: device found, Manufacturer ID: 0x98, Chip ID: 0xd1
[    3.201527] nand: Toshiba TC58NVG0S3E 1G 3.3V 8-bit
[    3.206563] nand: 128 MiB, SLC, erase size: 128 KiB, page size: 2048, OOB size: 64
[    3.214426] Scanning device for bad blocks
[    3.218980] Bad eraseblock 2 at 0x000000040000
[    3.223617] Bad eraseblock 3 at 0x000000060000
[    3.228248] Bad eraseblock 4 at 0x000000080000
[    3.232908] Bad eraseblock 5 at 0x0000000a0000
[    3.237540] Bad eraseblock 6 at 0x0000000c0000
[    3.242191] Bad eraseblock 7 at 0x0000000e0000
[    3.246832] Bad eraseblock 8 at 0x000000100000
[    3.251482] Bad eraseblock 9 at 0x000000120000
[    3.256115] Bad eraseblock 10 at 0x000000140000
[    3.260854] Bad eraseblock 11 at 0x000000160000
[    3.265576] Bad eraseblock 12 at 0x000000180000
[    3.385998] Creating 3 MTD partitions on "ar934x-nfc":
[    3.391361] 0x000000000000-0x000000040000 : "booter"
[    3.415702] 0x000000040000-0x000000400000 : "kernel"
[    3.440106] 0x000000400000-0x000008000000 : "ubi"
[    3.467190] libphy: Fixed MDIO Bus: probed
[    3.570422] libphy: ag71xx_mdio: probed
[    3.600473] switch0: Atheros AR8327 rev. 4 switch registered on ag71xx-mdio.0
[    4.551387] libphy: ag71xx_mdio: probed
[    5.179939] ag71xx ag71xx.0: connected to PHY at ag71xx-mdio.0:00 [uid=004dd034, driver=Atheros AR8216/AR8236/AR8316]
[    5.191527] eth0: Atheros AG71xx at 0xb9000000, irq 4, mode:RGMII
[    5.820245] ag71xx-mdio.1: Found an AR934X built-in switch
[    5.872547] eth1: Atheros AG71xx at 0xba000000, irq 5, mode:GMII
[    5.880091] NET: Registered protocol family 10
[    5.887812] NET: Registered protocol family 17
[    5.892509] bridge: filtering via arp/ip/ip6tables is no longer available by default. Update your scripts to load br_netfilter if you need this.
[    5.905897] 8021q: 802.1Q VLAN Support v1.8
[    5.913632] UBI: auto-attach mtd6
[    5.917081] ubi0: attaching mtd6
[    7.221445] random: crng init done
[    7.426918] ubi0: scanning is finished
[    7.447207] ubi0: attached mtd6 (name "ubi", size 124 MiB)
[    7.452921] ubi0: PEB size: 131072 bytes (128 KiB), LEB size: 129024 bytes
[    7.460030] ubi0: min./max. I/O unit sizes: 2048/2048, sub-page size 512
[    7.466943] ubi0: VID header offset: 512 (aligned 512), data offset: 2048
[    7.473962] ubi0: good PEBs: 992, bad PEBs: 0, corrupted PEBs: 0
[    7.480172] ubi0: user volume: 3, internal volumes: 1, max. volumes count: 128
[    7.487625] ubi0: max/mean erase counter: 1/0, WL threshold: 4096, image sequence number: 1883244627
[    7.497062] ubi0: available PEBs: 0, total reserved PEBs: 992, PEBs reserved for bad PEB handling: 20
[    7.506634] ubi0: background thread "ubi_bgt0d" started, PID 368
[    7.514612] block ubiblock0_1: created from ubi0:1(rootfs)
[    7.520334] ubiblock: device ubiblock0_1 (rootfs) set to be root filesystem
[    7.539618] Freeing unused kernel memory: 2356K
[    7.544304] This architecture does not have kernel memory protection.
[    7.563901] init: Console is alive
[    7.567618] init: - watchdog -
[    7.589023] kmodloader: loading kernel modules from /etc/modules-boot.d/*
[    7.598138] kmodloader: done loading kernel modules from /etc/modules-boot.d/*
[    7.616583] init: - preinit -
[    7.768095] IPv6: ADDRCONF(NETDEV_UP): eth0: link is not ready
Press the [f] key and hit [enter] to enter failsafe mode
Press the [1], [2], [3] or [4] key and hit [enter] to select the debug level
[    8.820086] eth0: link up (1000Mbps/Full duplex)
[    8.825052] IPv6: ADDRCONF(NETDEV_CHANGE): eth0: link becomes ready
[   10.904121] eth0: link down
[   10.916809] procd: - early -
[   10.920502] procd: - watchdog -
[   11.490728] procd: - watchdog -
[   11.494205] procd: - ubus -
[   11.548700] procd: - init -
Please press Enter to activate this console.
[   11.708101] kmodloader: loading kernel modules from /etc/modules.d/*
[   11.718268] ip6_tables: (C) 2000-2006 Netfilter Core Team
[   11.732810] Loading modules backported from Linux version wt-2017-11-01-0-gfe248fc2c180
[   11.741124] Backport generated by backports.git v4.14-rc2-1-31-g86cf0e5d
[   11.750633] ip_tables: (C) 2000-2006 Netfilter Core Team
[   11.763558] nf_conntrack version 0.5.0 (2048 buckets, 8192 max)
[   11.813083] xt_time: kernel timezone is -0000
[   11.866901] PPP generic driver version 2.4.2
[   11.873554] NET: Registered protocol family 24
[   11.933466] ieee80211 phy0: Atheros AR9340 Rev:3 mem=0xb8100000, irq=47
[   11.978871] kmodloader: done loading kernel modules from /etc/modules.d/*
[   25.175299] IPv6: ADDRCONF(NETDEV_UP): eth0: link is not ready
[   25.225670] eth0: link up (1000Mbps/Full duplex)
[   25.233083] br-lan: port 1(eth0.1) entered blocking state
[   25.238716] br-lan: port 1(eth0.1) entered disabled state
[   25.244650] device eth0.1 entered promiscuous mode
[   25.249647] device eth0 entered promiscuous mode
[   25.318646] IPv6: ADDRCONF(NETDEV_CHANGE): eth0: link becomes ready
[   25.378599] br-lan: port 1(eth0.1) entered blocking state
[   25.384188] br-lan: port 1(eth0.1) entered forwarding state
[   25.390231] IPv6: ADDRCONF(NETDEV_UP): br-lan: link is not ready
[   25.642196] IPv6: ADDRCONF(NETDEV_UP): eth1: link is not ready
[   25.670950] br-lan: port 2(eth1.1) entered blocking state
[   25.676533] br-lan: port 2(eth1.1) entered disabled state
[   25.682503] device eth1.1 entered promiscuous mode
[   25.687461] device eth1 entered promiscuous mode
[   26.258676] IPv6: ADDRCONF(NETDEV_CHANGE): br-lan: link becomes ready



BusyBox v1.28.4 () built-in shell (ash)

  _______                     ________        __
 |       |.-----.-----.-----.|  |  |  |.----.|  |_
 |   -   ||  _  |  -__|     ||  |  |  ||   _||   _|
 |_______||   __|_____|__|__||________||__|  |____|
          |__| W I R E L E S S   F R E E D O M
 -----------------------------------------------------
 OpenWrt 18.06.4, r7808-ef686b7292
 -----------------------------------------------------
=== WARNING! =====================================
There is no root password defined on this device!
Use the "passwd" command to set up a new password
in order to prevent unauthorized SSH logins.
--------------------------------------------------
root@OpenWrt:/# 
```
At this point, you are running OpenWRT in the RAM. 

**4. Connect PC to a different ethernet port.**

**5. Logon to OpenWrt webGUI.**
Login to 192.168.1.1 and get ready to flash Sysupgrade Mikrotik nand-large sysupgrade.bin.
You may download the sysupgradefile from this repo and continue with the flash process. 

**6. Sysupgrade Mikrotik nand-large sysupgrade.bin.**
Simplely upload the image to router, and it will start to flash to nand. 
```
root@OpenWrt:/# Watchdog handover: fd=3
- watchdog -
killall: telnetd: no process killed
killall: dropbear: no process killed
Sending TERM to remaining processes ... odhcpd udhcpc odhcp6c ntpd dnsmasq sh ubus ubusd logd rpcd netifd 
Sending KILL to remaining processes ... 
Performing system upgrade...
Unlocking kernel ...
Erasing kernel ...

Skipping bad block at 0x0   
Skipping bad block at 0x20000   
Skipping bad block at 0x40000   
Skipping bad block at 0x60000   
Skipping bad block at 0x80000   
Skipping bad block at 0xa0000   
Skipping bad block at 0xc0000   
Skipping bad block at 0xe0000   
Skipping bad block at 0x100000   
Skipping bad block at 0x120000   
Skipping bad block at 0x140000   Writing data to block 0 at offset 0x0
Bad block at 0, 1 block(s) will be skipped
Writing data to block 1 at offset 0x20000
Bad block at 20000, 1 block(s) will be skipped
Writing data to block 2 at offset 0x40000
Bad block at 40000, 1 block(s) will be skipped
Writing data to block 3 at offset 0x60000
Bad block at 60000, 1 block(s) will be skipped
Writing data to block 4 at offset 0x80000
Bad block at 80000, 1 block(s) will be skipped
Writing data to block 5 at offset 0xa0000
Bad block at a0000, 1 block(s) will be skipped
Writing data to block 6 at offset 0xc0000
Bad block at c0000, 1 block(s) will be skipped
Writing data to block 7 at offset 0xe0000
Bad block at e0000, 1 block(s) will be skipped
Writing data to block 8 at offset 0x100000
Bad block at 100000, 1 block(s) will be skipped
Writing data to block 9 at offset 0x120000
Bad block at 120000, 1 block(s) will be skipped
Writing data to block 10 at offset 0x140000
Bad block at 140000, 1 block(s) will be skipped
Writing data to block 11 at offset 0x160000
Writing data to block 12 at offset 0x180000
Writing data to block 13 at offset 0x1a0000
Writing data to block 14 at offset 0x1c0000
Writing data to block 15 at offset 0x1e0000
Writing data to block 16 at offset 0x200000
Writing data to block 17 at offset 0x220000
Writing data to block 18 at offset 0x240000
Writing data to block 19 at offset 0x260000
Writing data to block 20 at offset 0x280000
Writing data to block 21 at offset 0x2a0000
Writing data to block 22 at offset 0x2c0000
removing ubiblock0_1
[  161.737871] block ubiblock0_1: released
Volume ID 0, size 12 LEBs (1548288 bytes, 1.4 MiB), LEB size 129024 bytes (126.0 KiB), dynamic, name "none", alignment 1
Volume ID 1, size 19 LEBs (2451456 bytes, 2.3 MiB), LEB size 129024 bytes (126.0 KiB), dynamic, name "rootfs", alignment 1
Set volume size to 120895488
Volume ID 2, size 937 LEBs (120895488 bytes, 115.2 MiB), LEB size 129024 bytes (126.0 KiB), dynamic, name "rootfs_data", alignment 1
sysupgrade successful
umount: can't unmount /dev: Resource busy
umount: can't unmount /tmp: Resource busy
[  163.504995] reboot: Restarting systemt
�

RouterBOOT booter 3.18

RouterBoard 2011UiAS-2HnD

CPU frequency: 750 MHz
 Memory speed: 200 MHz
  Memory size: 128 MiB
    NAND size: 128 MiB

Press any key within 2 seconds to enter setup..

loading kernel from nand... OK
setting up elf image... OK
jumping to kernel code


OpenWrt kernel loader for AR7XXX/AR9XXX
Copyright (C) 2011 Gabor Juhos <juhosg@openwrt.org>
Decompressing kernel... done!
Starting kernel at 80060000...

[    0.000000] Linux version 4.4.50 (buildbot@builds-02.infra.lede-project.org) (gcc version 5.4.0 (LEDE GCC 5.4.0 r3103-1b51a49) ) #0 Mon Feb 20 17:13:44 2017
[    0.000000] bootconsole [early0] enabled
[    0.000000] CPU0 revision is: 0001974c (MIPS 74Kc)
[    0.000000] SoC: Atheros AR9344 rev 3
[    0.000000] Determined physical RAM map:
[    0.000000]  memory: 08000000 @ 00000000 (usable)
[    0.000000] User-defined physical RAM map:
[    0.000000]  memory: 08000000 @ 00000000 (usable)
[    0.000000] Initrd not found or empty - disabling initrd
[    0.000000] No valid device tree found, continuing without
[    0.000000] Zone ranges:
[    0.000000]   Normal   [mem 0x0000000000000000-0x0000000007ffffff]
[    0.000000] Movable zone start for each node
[    0.000000] Early memory node ranges
[    0.000000]   node   0: [mem 0x0000000000000000-0x0000000007ffffff]
[    0.000000] Initmem setup node 0 [mem 0x0000000000000000-0x0000000007ffffff]
[    0.000000] Primary instruction cache 64kB, VIPT, 4-way, linesize 32 bytes.
[    0.000000] Primary data cache 32kB, 4-way, VIPT, cache aliases, linesize 32 bytes
[    0.000000] Built 1 zonelists in Zone order, mobility grouping on.  Total pages: 32512
[    0.000000] Kernel command line: parts=1 boot_part_size=4194304 gpio=249387 HZ=375000000 mem=128M kmac=4C:5E:0C:C7:10:60 board=2011r5 ver=3.18 boot=1 mlc=6 rootfstype=squashfs noinitrd
[    0.000000] PID hash table entries: 512 (order: -1, 2048 bytes)
[    0.000000] Dentry cache hash table entries: 16384 (order: 4, 65536 bytes)
[    0.000000] Inode-cache hash table entries: 8192 (order: 3, 32768 bytes)
[    0.000000] Writing ErrCtl register=00000000
[    0.000000] Readback ErrCtl register=00000000
[    0.000000] Memory: 125012K/131072K available (3394K kernel code, 174K rwdata, 468K rodata, 244K init, 203K bss, 6060K reserved, 0K cma-reserved)
[    0.000000] SLUB: HWalign=32, Order=0-3, MinObjects=0, CPUs=1, Nodes=1
[    0.000000] NR_IRQS:51
[    0.000000] Clocks: CPU:750.000MHz, DDR:400.000MHz, AHB:400.000MHz, Ref:25.000MHz
[    0.000000] clocksource: MIPS: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 5096694524 ns
[    0.000007] sched_clock: 32 bits at 375MHz, resolution 2ns, wraps every 5726622718ns
[    0.008840] Calibrating delay loop... 373.55 BogoMIPS (lpj=1867776)
[    0.071916] pid_max: default: 32768 minimum: 301
[    0.077259] Mount-cache hash table entries: 1024 (order: 0, 4096 bytes)
[    0.084774] Mountpoint-cache hash table entries: 1024 (order: 0, 4096 bytes)
[    0.094906] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 19112604462750000 ns
[    0.107030] NET: Registered protocol family 16
[    0.116391] MIPS: machine is Mikrotik RouterBOARD 2011UiAS-2HnD
[    0.569421] clocksource: Switched to clocksource MIPS
[    0.576064] NET: Registered protocol family 2
[    0.581827] TCP established hash table entries: 1024 (order: 0, 4096 bytes)
[    0.589790] TCP bind hash table entries: 1024 (order: 0, 4096 bytes)
[    0.597008] TCP: Hash tables configured (established 1024 bind 1024)
[    0.604308] UDP hash table entries: 256 (order: 0, 4096 bytes)
[    0.610979] UDP-Lite hash table entries: 256 (order: 0, 4096 bytes)
[    0.618318] NET: Registered protocol family 1
[    0.627028] futex hash table entries: 256 (order: -1, 3072 bytes)
[    0.634052] Crashlog allocated RAM at address 0x3f00000
[    0.652051] squashfs: version 4.0 (2009/01/31) Phillip Lougher
[    0.658671] jffs2: version 2.2 (NAND) (SUMMARY) (LZMA) (RTIME) (CMODE_PRIORITY) (c) 2001-2006 Red Hat, Inc.
[    0.672088] io scheduler noop registered
[    0.676537] io scheduler deadline registered (default)
[    0.682653] Serial: 8250/16550 driver, 16 ports, IRQ sharing enabled
[    0.692170] console [ttyS0] disabled
[    0.716310] serial8250.0: ttyS0 at MMIO 0x18020000 (irq = 11, base_baud = 1562500) is a 16550A

```
At this point tho, the serial port has been disabled, it is normal. you may continue with step 7. 

If you make a mistake, repeat netboot and try again. 

**7.You will need to plug in to any of the fast ethernet port in order to reach the 192.168.1.1.**
Not sure about why, you may need to configure something on the Luci webGUI.

**8.You are all Done! Enjoy OpenWRT on your MT RB2011.** 
