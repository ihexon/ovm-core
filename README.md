# 比常规 Linux 发行版快那么一点的 Linux 可启动镜像

使用 Qemu 启动镜像：

```sh
sudo \
        PATH=/home/ihexon/qemu_bins/bin/:$PATH \
        qemu-system-aarch64 -nographic -enable-kvm -cpu max -smp 4 -m 2G \
        -machine "virt,gic-version=host" \
        -kernel  vmlinuz-virt \
        -initrd  initramfs-virt  \
        -append  "root=UUID=c6c11cf4-a873-4172-ae9b-6e700fc96bcd ro modules=sd-mod,usb-storage,ext4 quiet rootfstype=ext4" \
        -hda     alpine_disk.qcow2 \
        -netdev "user,id=net0,restrict=n,hostfwd=tcp:127.0.0.1:10024-:22" -device virtio-net-pci,netdev=net0 \
        -device "virtio-balloon-pci,id=balloon0" \
        -device "vhost-vsock-pci,guest-cid=455"
```

在 rk3588 SoC上测试，除去 Qemu 和依赖库本身载入内存的时间，真正 Kernel 从启动该到 Podman 启动耗时 2s 左右。

# TODO:
- [ ] 从 rk3399 SoC 真机启动
- [ ] 理论上镜像还能更小
