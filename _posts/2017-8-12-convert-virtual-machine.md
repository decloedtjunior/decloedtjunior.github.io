---
layout: post
sitemap: true
noindex: false
title: qemu-img for Windows (VM Disk Convert)
categories: vhdx vdi hyper-v virtualbox
---

QEMU disk image utility for Windows. It is used for converting, creating and consistency checking of various virtual disk formats. It’s compatible with Hyper-V, KVM, VMware, VirtualBox and Xen virtualization solutions.This build has been optimized for Windows Server (x64), including Windows Nano Server.

## Usage examples

Convert a QCOW2, RAW, VMDK or VDI image to VHDX

```
qemu-img.exe convert source.img -O vhdx -o subformat=dynamic dest.vhdx
```

Check a virtual disk for consistency
```
qemu-img.exe check source.qcow2
```

Get info about a virtual disk
```
Get info about a virtual disk
```

More info at [https://cloudbase.it/qemu-img-windows/](https://cloudbase.it/qemu-img-windows/)