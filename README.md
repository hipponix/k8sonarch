# k8sonarch

*Work in progress*

My shiny notes while installing and configuring Kubenetes on Arch Linux.

# Partition the disk

I stricly followed this excellent document from Arch [here](https://wiki.archlinux.org/title/Parted).

```
(parted) mkpart "swap partition" linux-swap 1025MiB 5121MiB
(parted) mkpart "root partition" ext4 5121MiB 100%
(parted) type 3 4F68BCE3-E8CD-4DB1-96E7-FBCAF984B709
```

## Install and configure the base system

Follow the excellent Arch Linux guide on how to perform the installation [here](https://wiki.archlinux.org/title/Installation_guide).

When you arrive at the `boot-loader` configuration, I assume you are in `chroot` environment already. So in my case I chose `grub` to be the default tool for this, here couple more commands I had to run:

```
pacman -Sy grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

You can refer to the dedicated GRUB section [here](https://wiki.archlinux.org/title/GRUB).

Please note that if you don't install a network utility, then you won't be able to do pretty much anything with the new system as it lacks connectivity to internet and local network.

Therefore the only change I would add to the very well documented installation process, relates the package installation before entering the `chroot` environment:

```
pacstrap -K /mnt base linux linux-firmware networkmanager
```

Just add the `networkmanager` packager and enable its systemctl service when in chroot or when you are in the real system:
```
systemctl enable --now NetworkManager.service
```

More info on configuring the Netwrok can be found [here](https://wiki.archlinux.org/title/Network_configuration#Network_interfaces).

Then, remember to install sshd and add a local user if you want connect remotely, as the deault configuration (luckily) does not allow root login nor the openssh package is installed:

```
useradd -m <choose_username>
pacman -Sy openssh
systemctl enable --now sshd
```

More about user-mgmt [here](https://wiki.archlinux.org/title/Users_and_groups#User_management).

## More to come ...

[..]

## References
- [Arch-Linux Installation Guide](https://wiki.archlinux.org/title/Installation_guide)
- [Arch-Linux Partitioning](https://wiki.archlinux.org/title/Partitioning)
- [Arch-Linux Parted](https://wiki.archlinux.org/title/Parted)
- [Arch-Linux Grub](https://wiki.archlinux.org/title/GRUB)
- [Arch-Linux Networki Interfaces](https://wiki.archlinux.org/title/Network_configuration#Network_interfaces)
- [Arch Linux User Management](https://wiki.archlinux.org/title/Users_and_groups#User_management)
