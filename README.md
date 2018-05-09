# ArchLinux

## Install ArchLinux on VirtualBox

1. Create ArchLinux VM, Enable EFI under Settings -> System, boot to installation media
2. Using cfdisk, create:
      
      * /dev/sda1: 550M EFI System
      * /dev/sda2: xG  Linux filesystem

3. Make filesystems

    ```
    mkfs.fat -F32 /dev/sda1
    mkfs.ext4 /dev/sda2
    ```
4. Mount partitions
    ```
    mount /dev/sda2 /mnt
    mkdir /mnt/boot
    mount /dev/sda1 /mnt/boot
    ```
5. *(Optional)* If behind a proxy, export http_proxy and https_proxy for pacman downloads

    If your proxy cannot resolve certain domains or is too slow with certain mirrors, sort your mirror list as so:
    
    ```
    pacman -S reflector
    reflector --verbose --sort rate --save /etc/pacman.d/mirrorlist
    ``` 
6. Follow ArchWiki Installation steps up to and including setting the root password: https://wiki.archlinux.org/index.php/installation_guide#Installation
7. Setup GRUB bootloader
    ```
    pacman -S grub efibootmgr
    grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=arch_grub
    grub-mkconfig -o /boot/grub/grub.cfg
    
    # Configure bootloader for VirtualBox
    mkdir /boot/EFI/boot
    cp /boot/EFI/arch_grub/grubx64.efi /boot/EFI/boot/bootx64.efi
    ```
8. Remove installation media and reboot VM
