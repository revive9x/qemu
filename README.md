# Qemu with 3dfx patches

### How to build:

```bash
mkdir build
cd build
../configure --target-list=i386-softmmu
make -j$(nproc)
```

### How to run (Win98SE):

```
./qemu-system-i386 -monitor stdio -nodefaults -rtc base=localtime \
    -cpu pentium2 -m 256 -display sdl \
    -M pc,accel=kvm,hpet=off,kernel-irqchip=off,usb=off \
    -device VGA -device lsi -device ac97 \
    -drive id=win98,if=none,file=w98.qcow2 -device scsi-hd,drive=win98 \
    -drive id=scd0,if=none,media=cdrom,file=../vmaddons.iso -device scsi-cd,drive=scd0
```
