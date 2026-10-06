# polyd
Implements rarpd, tftpd, portmapd, bootparamd and nfsd in 1 executable to simplify SunOS netboot.

To avoid polluting your machine with a whole raft of ancient services you'll never use again, this executable implements all the services required to netboot a SunOS workstation'

Its a single file to compile with:

cc -o polyd polyd.c

And you provide the config right on the command line - for example:

sudo ./polyd en0 -hostname cruella -addr 192.168.1.71 -mac 08:00:20:7f:00:00 -base /Users/Shared/export/tftp -fs /Users/Shared/export/root/cruella -swap /Users/Shared/export/swap/cruella -dump /Users/Shared/export/dump

where "en0" is the network interface you are using.
-base XXX   path to folder with the boot.sun4c but renamed as hex IP address of the Sun  (C0A80147.SUN4C for addr 192.168.1.71)
-fs root filesystem
-swap contiguous swap file at least the size of the RAM you have
-dump place to dump memory

