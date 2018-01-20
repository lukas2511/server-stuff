# create debian iso using serial port as console

(i'm using ttyS1, you might want to use ttyS0 and the corresponding `serial 0` and `console 0` below)

install syslinux and libisoburn (or whatever it's called in the currently running distro, we need xorriso)

download iso: `wget https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-9.3.0-amd64-netinst.iso`

mount iso: `mkdir orig; mount -o loop debian-9.3.0-amd64-netinst.iso orig`

copy files: `cp -Ra orig debilian`

umount iso: `umount orig; rmdir orig`

open `debilian/isolinux/txt.cfg`, add `console=ttyS1,115200n8` before `--- quiet` (add this to other files like `adtxt.cfg` if you want advanced setup).
also replace `vga=whatever` with `vga=off`

open `debilian/isolinux/isolinux.cfg`, add `serial 1 115200` and `console 1` before `path`, replace `vesamenu.c32` with `menu.c32`

put some stuff in there: `cp /usr/lib/syslinux/bios/{isolinux.bin,menu.c32,ldlinux.c32,libcom32.c32,libutil.c32} debilian/isolinux/`

generate new iso: `xorriso -as mkisofs -r -J -joliet-long -l -cache-inodes -isohybrid-mbr /usr/lib/syslinux/bios/isohdpfx.bin -partition_offset 16 -A "Debilian" -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table -o debilian-serial-install.iso debilian`

remove temp files: `rm -r debilian`

write iso to stick: `dd if=debilian-serial-install.iso of=/dev/sdX bs=1M status=progress oflag=dsync conv=fdatasync`

boot thing from stick

# use a fast stick

Use a fast (USB3) stick so you can try again without needing several minutes of `dd` each time...

# try in qemu

You can test it with qemu or something similar if you have a slow booting server and don't want to try to boot it several times...

For the second serial port (ttyS1) this works: `qemu-system-x86_64 -cdrom debilian-serial-install.iso -serial null -serial stdio -nographic -monitor null`

For the first (ttyS0) this should work: `qemu-system-x86_64 -cdrom debilian-serial-install.iso -serial stdio -nographic -monitor null`
