---
title: Setting up Proxmox GPU Passthrough
date: 2025-03-09 13:00:00 +0400
categories: [homelab]
tags: [homelab, proxmox, server, hardware, nvidia]
image:
  path: assets/img/posts/common/proxmox-logo.png
  lqip: assets/img/posts/common/proxmox-logo-lqip.webp
---

Welcome to this quick guide on how to set up GPU passthrough on a server running Proxmox. In my specific case, I used an `Nvidia` GPU with an `Intel` CPU; however, this guide should also work for other major vendors like AMD with minor adjustments.

## Prerequisites

- Make sure your hardware supports `IOMMU` by either looking up the specification or running the following command in your target Proxmox node shell:

```shell
# if you get a match, then you have support (vmx - Intel, svm - AMD)
cat /proc/cpuinfo | grep -g "vmx|svm"
```

- Make sure `IOMMU` / `VT-d` are enabled in BIOS. 

## Guide

#### 1. Go to the Shell on your Proxmox node

#### 2. Enable IOMMU in Proxmox

```shell
nano /etc/default/grub

# find the following line: GRUB_CMDLINE_LINUX_DEFAULT="quiet".
# replace with this (for Intel):
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt"
```

#### 3. Save the file and update GRUB

```shell
update-grub 
```

#### 4. Reboot the server

```shell
reboot
```

#### 5. After restarting, make sure IOMMU is enabled successfully

```shell
# expected match should look something like this: DMAR: IOMMU enabled
dmesg | grep -i "IOMMU enable"
```

#### 6. Identify your GPU and it's associated devices (make sure to note all the vendor ids for later)

```shell
# vendor ids should be at the end of each line, looking like this: [xxxx:xxxx]
lspci -nn | grep -i vga
lspci -nn | grep -i audio
```

#### 7. Add required VFIO modules

```shell
nano /etc/modules

# add these to the file:
vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd
```

#### 8. Adjust VFIO and KVM settings to improve virtualization compatibility

```shell
echo "options vfio_iommu_type1 allow_unsafe_interrupts=1" > /etc/modprobe.d/iommu_unsafe_interrupts.conf
echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf
```

#### 9. Blacklist default nvidia drivers, since they are known to cause issues with Proxmox

```shell
echo "blacklist nvidia" >> /etc/modprobe.d/blacklist.conf 
```

#### 10. Add your hardware vendor ids to the VFIO file

```shell
# replace 'xxxx:xxxx' with your ids, your number of ids might be different
echo "options vfio-pci ids=xxxx:xxxx, xxxx:xxxx, xxxx:xxxx disable_vga=1" > /etc/modprobe.d/vfio.conf
```

#### 11. Update initramfs

```shell
update-initramfs -u
```

#### 12. Reboot

```shell
reboot
```

#### 13. Additional steps

To connect your GPU to a VM, go to **Hardware**, click **Add** > **PCI** > **Raw device**, then select your GPU.

If you plan to use your GPU for `AI` tasks and workloads, installing `CUDA` drivers in your VM can enhance performance. Follow the instructions on [the official NVIDIA website](https://developer.nvidia.com/cuda-downloads).

Verify that the drivers were installed correctly:

```shell
# should display details about your GPU
nvidia-smi 
```

## Useful links

- <https://pve.proxmox.com/wiki/PCI(e)_Passthrough>
- <https://developer.nvidia.com/cuda-downloads>
- <https://www.youtube.com/watch?v=_hOBAGKLQkI>
- <https://www.youtube.com/watch?v=Il6HhOCfDjI>