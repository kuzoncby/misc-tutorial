# Install CentOS 7 via PXE with UEFI

This guide is talking about install CentOS 7 via PXE with UEFI.

## Install required packages

Those packages are required:

- TFTP Server
- DHCP Server
- xinetd

```sh
yum install tftp-server dhcp xinetd
```

## Config xinetd

Set `disabled` to `no` at `/etc/xinet.d/tftp`

## Config TFTP Server

Create a directory: `/var/lib/tftpboot/pxelinux`

```sh
mkdir /var/lib/tftpboot/pxelinux
```

Generate EFI image

```sh
yum install -y grub2-efi-modules
grub2-mkstandalone -d /usr/lib/grub/x86_64-efi/ -O x86_64-efi --modules="tftp net efinet linux part_gpt efifwsetup" -o bootx64.efi
```

Then copy this `bootx64.efi` to `/var/lib/tftpboot/pxelinux`

Create `grub.cfg` under `/var/lib/tftpboot`

```sh
set default="0"

function load_video {
  insmod efi_gop
  insmod efi_uga
  insmod video_bochs
  insmod video_cirrus
  insmod all_video
}

load_video
set gfxpayload=keep
insmod net
insmod efinet
insmod tftp
insmod gzio
insmod part_gpt
insmod ext2

set timeout=60

set timeout=60
### END /etc/grub.d/00_header ###

search --no-floppy --set=root -l 'CentOS 7 x86_64'

### BEGIN /etc/grub.d/10_linux ###
menuentry 'Install CentOS Linux 7' --class fedora --class gnu-linux --class gnu --class os {
	linuxefi (tftp)/vmlinuz ip=dhcp inst.repo=http://repo.example.com/centos/7/os/x86_64/
	initrdefi (tftp)/initrd.img
}
```

Copy system image to `/var/lib/tftpboot`

```sh
cp /path/to/x86_64/os/images/pxeboot/{vmlinuz,initrd.img} /var/lib/tftpboot
```

## Config DHCP Server

Edit `/etc/dhcp/dhcpd.conf`

```sh
log-facility local7;
allow booting;
allow bootp;
option space pxelinux;
option pxelinux.magic code 208 = string;
option pxelinux.configfile code 209 = text;
option pxelinux.pathprefix code 210 = text;
option pxelinux.reboottime code 211 = unsigned integer 32;
option architecture-type code 93 = unsigned integer 16;

subnet 10.0.0.0 netmask 255.255.255.0 {
	option routers 10.0.0.1;
	range 10.0.0.100 10.0.0.200;

	next-server 10.0.0.10;
	filename "bootx64.efi";
	
	host hypervisor {
		hardware ethernet 00:0C:29:5D:DF:30;
		fixed-address 10.0.0.202;
	}
}
```
## Start Service

```sh
systemctl start xinetd dhcpd vsftpd
```

## Special thanks to

- [sungm1984](http://blog.chinaunix.net/uid-22621471-id-4980582.html)