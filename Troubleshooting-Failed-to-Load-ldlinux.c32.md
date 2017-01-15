# Failed to load ldlinux.c32

When I try to PXE some CentOS 7 via a fedora 25. I follow instruction by [server-world](https://www.server-world.info/en/note?os=CentOS_7&p=pxe&f=2). But when I start my target machine, it's show up **_Failed to load ldlinux.c32_**. Finally after I copy those tree file to tftp root, it's works.

- ldlinux.c32
- libutil.c32
- libcom32.c32

```sh
$ cp /usr/share/syslinux/menu.c32 /var/lib/tftpboot/
$ cp /usr/share/syslinux/ldlinux.c32 /var/lib/tftpboot/
$ cp /usr/share/syslinux/libutil.c32 /var/lib/tftpboot/
$ cp /usr/share/syslinux/libcom32.c32 /var/lib/tftpboot/
$ ls
ldlinux.c32  libcom32.c32  libutil.c32  menu.c32  pxeboot  pxelinux.0  pxelinux.cfg
```