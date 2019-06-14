# How to instal VIRL 1.6.65 with UEFI
This howto was created in order to install VIRL onto a Cisco UCS C220 M5 server with NVME drives.  The NVME drives are 4K sector and therefore require UEFI boot.  Because the VIRL installer is legacy (MBR) boot only and does not support UEFI (GPT), we use the Ubuntu 16.04 Desktop installer to re-configure the already installed VIRL system for UEFI.

## Install VIRL via legacy mode
1. Mount the VIRL 1.6.65 ISO via CIMC or KVM
1. Boot the server in legacy mode
1. Install VIRL to /dev/nvme0n1 using the normal install process for bare metal, using the full disk

## Boot the Ubuntu installer via UEFI
1. After the VIRL install is complete, mount the Ubuntu 16.04 Desktop ISO
1. Boot the server in UEFI mode
1. Select "Try Ubuntu" from the boot menu
1. After Ubuntu boots into memory, open a terminal

## Create an EFI partition on the already installed VIRL disk and change it from MBR to GPT
1. `sudo gdisk /dev/nvme0n1`
1. Use `n` to create a new partition
1. Choose a start and end for the partion from free space on your disk (the partion needs to be at least 1MB)
1. Use `c` to label the partition "EFI-system"
1. Use `w` to write the partition table.
1. Use `sudo partprobe /dev/nvme0n1` to reload the partition table

## Build the filesystem for the ESP
1. `mkfs -t vfat -v /dev/disk/by-partlabel/EFI-system`
1. `sudo apt-get install -y grub-efi-amd64`
1. `sudo mount /dev/nvme0n1p1 /mnt`
    > Note: use the root partition that VIRL was installed to.  In my case that was /dev/nvme0n1p1.
1. `sudo mkdir -p /mnt/boot/efi`
1. `sudo mount /dev/nvme0n1p2 /mnt/boot/efi` 
    > Note: use the partition you created for EFI-system above.  In my case that was /dev/nvme0n1p2.
1. `for d in dev sys proc usr run; do sudo mount -B /$d /mnt/$d; done`
1. `sudo modprobe efivars`
1. `sudo chroot /mnt`
1. `grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=ubuntu --recheck --no-floppy --debug`
1. Unmount the Ubuntu 16.04 Desktop ISO
1. Reboot
1. The system should now boot in VIRL via UEFI
