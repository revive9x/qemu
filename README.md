# Qemu with 3dfx patches

### How to build:

```bash
mkdir build
cd build
../configure --target-list=i386-softmmu
make -j$(nproc)
```

### Installing Windows 98 SE

Create a qcow2 disk image:

```
qemu-img create -f qcow2 win98.qcow2 1024M
```

Run QEMU with the patcher9x boot floppy and win98se iso image attached:

```
./qemu-system-i386 -nodefaults -rtc base=localtime -display sdl \
    -M pc,accel=kvm,hpet=off,usb=off -cpu pentium2 \
    -device VGA -device lsi -device ac97 \
    -netdev user,id=net0 -device pcnet,rombar=0,netdev=net0 \
    -drive id=win98,if=none,file=win98.qcow2 -device scsi-hd,drive=win98 \
    -drive if=floppy,format=raw,file=fd.ima \
    -cdrom w98.iso -boot a
```

Create a primary partition using `fdisk` and restart, then use

```
format.exe C:
```

to format the created partition. Then run:

```
xcopy.exe /E D:\win98\ C:\setupcd\
```

to copy the installer files to the C:\ drive. Run patch9x to apply needed patches to the installer files:

```
patch9x.exe C:\setupcd\
```

Now run the Windows setup:

```
C:
cd C:\setupcd
setup.exe /nm /pj
```

After completing the first stage of setup (First reboot of windows), switch to the command below:

### How to run (Win98SE):

This command allows you to run a KVM accelerated Win9x guest with OpenGL passthrough. It also enables a pcnet network card.

```
./qemu-system-i386 -nodefaults -rtc base=localtime -display sdl \
    -M pc,accel=kvm,hpet=off,usb=off -cpu pentium2 \
    -device VGA -device lsi -device ac97 \
    -netdev user,id=net0 -device pcnet,rombar=0,netdev=net0 \
    -drive id=win98,if=none,file=win98.qcow2 -device scsi-hd,drive=win98 \
    -drive id=vm3d,if=none,media=cdrom,file=vmaddons.iso -device scsi-cd,drive=vm3d
```

### Up next:

- Properly document in-guest setup
- Figure out how to reliably build the guest wrappers (Docker?)
- Requires modifications to the 98SE setup CD (Figure out how to do that on Linux, or use ImgBurn on Windows)
