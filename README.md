# polyd
Implements **rarpd, tftpd, portmapd, bootparamd** and **nfsdV2** in 1 executable to simplify SunOS netboot.

To avoid polluting your machine with a whole raft of ancient services you'll never use again, this executable implements all the services required to netboot a SunOS workstation.

Its a single file to compile with:

*cc -o polyd polyd.c*

And you provide all the config right on the command line - for example:

*sudo ./polyd en0 -hostname cruella -addr 192.168.1.71 -mac 08:00:20:7f:00:00 -base /Users/Shared/export/tftp -fs /Users/Shared/export/root/cruella -swap /Users/Shared/export/swap/cruella -dump /Users/Shared/export/dump*

where "en0" is the network interface you are using.
- -base XXX   path to folder with the boot.sun4c you are using, but renamed as hex IP address of the Sun  (C0A80147.SUN4C for addr 192.168.1.71)
- -fs XXX path to root filesystem
- -swap XXX path to contiguous swap file at least the size of the RAM you have
- -dump XXX path to place to dump memory

**Note that -fs and -swap filepaths need to end with the hostname you are using**

You may need to pause your system portmapper so polyd gets the requests:

sudo systemctl stop portmap bootparamd

# Installing
- Get a SunOS .iso and unpack it onto your host computer.  Inside the unpacked files you need to locate and copy the minimal root Unix install to boot - from which you can do a full install.  It is named 'miniroot' and on *SunOS4.1.4* it is located in:

**EXEC / KVM / SUN4C_SUNOS_4_1_1 / MINIROOT_SUN4C**

This single file is actually an archive of a filesystem that needs unpacking to your '-fs' path.

- Once unpacked, copy the top level file 'boot.sun4c' to your '-base' path so it can be served by the tftpd. Rename it to:

  C0A80147.SUN4C    (your '-addr' you will be using but in hexadecimal, and ALL CAPS)

- Turn on your Sun worksation and at the boot prom ok prompt, type: 

*boot net -s*
