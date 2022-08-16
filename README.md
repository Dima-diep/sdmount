# sdmount

## What is it?
It's utility for mounting devices, virtual filesystems (tmpfs, ramfs) into /sdcard

## Why I can't use usual mount command?

Firstly, Android uses mount namespaces. If you mounts into /sdcard, your files will not be availabled for other apps or even console sessions

Secondly, you need special mount options for mounting into /sdcard for availability to see your files by other apps. These options are `nosuid,nodev,noexec,noatime,context=u:object_r:sdcardfs:s0,uid=0,gid=9997,fmask=660,dmask=771,nouser_xattr`.

Thirdly, you mustn't mount just into `/sdcard`. Your real mount point have to be as `/mnt/runtime/write/emulated/(mountpoint)`.

## Supported filesystems

All filesystems which your device support besides "overlay"-based filesystems *(aufs, unionfs, mhddfs)*

## Usage
```
 ~ # sdmount /dev/sda4 -t ext3 -o ro /0/Music
# Mounts /dev/sda4 into /storage/emulated/0/Music
 ~ # sdmount ./loop.img -t ext2 -o rw /0/loop
# Mounts ./loop.img into /storage/emulated/0/loop
# there is auto-losetup for loop devices
```
For overlayfs, use `sdmount-overlay`:
```
 ~ # sdmount-overlay -lowerdir /data/lowerdir,/data/lowerdir2 -upperdir /data/upperdir -workdir /data/workdir -o (opts) /0/overlayed
```
