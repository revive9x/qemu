# Qemu with 3dfx patches

### How to build:

```bash
mkdir build
cd build
../configure --target-list=i386-softmmu
make -j$(nproc)
```
