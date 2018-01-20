# update bios

makefile for creating usb bootable image with tinycore and flashrom:

```
all: boot.img

ROM_VERSION = 160311
MBR = /usr/lib/syslinux/bios/mbr.bin

tmp/apu_tinycore.tar.bz2:
	mkdir -p tmp
	wget -O $@ http://pcengines.ch/file/apu_tinycore.tar.bz2

tmp/apu_tinycore/.unpacked: tmp/apu_tinycore.tar.bz2
	mkdir -p tmp/apu_tinycore
	bsdtar -xf $< -C tmp/apu_tinycore/
	touch tmp/apu_tinycore/.unpacked

tmp/apu2_$(ROM_VERSION).zip:
	mkdir -p tmp
	wget -O $@ http://www.pcengines.ch/file/apu2_$(ROM_VERSION).zip

tmp/apu2_$(ROM_VERSION).rom: tmp/apu2_$(ROM_VERSION).zip
	bsdtar -xf $< -C tmp/
	touch $@

tmp/empty.img: $(MBR)
	mkdir -p tmp
	dd if=/dev/zero of=$@ bs=1M count=64
	parted $@ mklabel msdos
	parted $@ mkpart primary fat16 1 100%
	parted $@ set 1 boot on
	dd if=$< of=$@ conv=notrunc

tmp/fat.img: tmp/empty.img tmp/apu_tinycore/.unpacked tmp/apu2_$(ROM_VERSION).rom
	dd if=$< of=$@ bs=512 skip=2048
	mformat -i $@ -h 255 -s 63 -t 9 -m 0xf8 -v "tinycore" -H 0 -N 13372511 -d 2 -H 0 ::
	mcopy -i $@ $(wildcard tmp/apu_tinycore/*) tmp/apu2_$(ROM_VERSION).rom ::
	syslinux --install $@

boot.img: tmp/empty.img tmp/fat.img
	cp tmp/empty.img $@
	dd if=tmp/fat.img of=$@ bs=512 seek=2048

clean:
	rm -rf boot.img tmp/*.img tmp/apu_tinycore tmp/*.rom
```
