First, install all the necessary packages at /mnt (which will become the root directory of the OS afterwards.) using pacstrap.

```bash
pacstrap -i /mnt base
```

<ins>**Note:**</ins> If you face `File /mnt/var/cache/pacman/pkg/xyz.pkg.tar.zst is corrupted (invalid or corrupted package (PGP signature))` then you are most probably using older archiso just update the keyring and rerun the command it should work now.

```bash
# Update keyring
pacman -Sy archlinux-keyring
```

![](Attachments/Pasted%20image%2020221219203153.png)

After installing packages, change root to /mnt (the new root) using arch-chroot.

```bash
arch-chroot /mnt
```

![](Attachments/Pasted%20image%2020221219205710.png)

Now, install linux kernel and headers.
_Note:_ The linux and linux-lts both can be installed at the same time but only one is required, using both simultanously gives user the extra option that is, for some reason some application is broken after new linux version then we can revert back to the _lts_ version for better compatibility. Other than that it is non-mandetory.

```bash
pacman -S linux linux-headers linux-lts linux-lts-headers linux-firmware
```

![](Attachments/Pasted%20image%2020221219210142.png)

## Installing optional (but very useful) packages.

It's completely optional but, the packages we are installing now are quite useful and we may need them now or in near future e.g. text editor, ssh-client and server, etc. So it is a very good idea to install them while installing the OS.

```bash
pacman -S nano base-devel openssh
```

![](Attachments/Pasted%20image%2020221219211147.png)

To install network tools and network manager.

```bash
pacman -S networkmanager wpa_supplicant wireless_tools netctl dialog
```

![](Attachments/Pasted%20image%2020221219211521.png)

Enable wpa_supplicant service.

```bash
systemctl enable wpa_supplicant
```

Enable the network manager service.

```bash
# the service name is case-sensitive
systemctl enable NetworkManager
```

![](Attachments/Pasted%20image%2020221219211810.png)

Install LVM tools using pacman (package manager)

```bash
pacman -S lvm2
```

![](Attachments/Pasted%20image%2020221219212001.png)

Now we need to add `lvm2` into the hooks section of `/etc/mkinitcpio.conf`

```bash
nano /etc/mkinitcpio.conf

# To save and exit use "ctr + x" to exit and "y" to save the file and "enter".
```

![](Attachments/Pasted%20image%2020221219212643.png)

<ins>**Note:**</ins> In below screenshot I have added `lvm2` in the `commented` line you need to find the line which is `uncommented` which you will find one or two lines below one shown in the following image.

![](Attachments/Pasted%20image%2020221219212608.png)

To make those options take effect we have to generate new image and it can be done using `mkinitcpio -P` (-P is used to build for all installed kernels at the same time; otherwise -p can be used followed by the kernel name linux or linux-lts).

```bash
mkinitcpio -P
```

![](Attachments/Pasted%20image%2020221219213132.png)

We can change the locale (the language OS will use) into whatever we want. And it can be done by steps performed below.

```bash
nano /etc/locale.gen
```

![](Attachments/Pasted%20image%2020221219213402.png)
Uncomment the desired locale (basically remove the # symbol)

![](Attachments/Pasted%20image%2020221219213331.png)

```bsah
locale-gen
```

![](Attachments/Pasted%20image%2020221219213659.png)

### User Management

Even though this can be done afterwards but it is a good idea do it now.

Set root password

```bash
passwd
```

![](Attachments/Pasted%20image%2020221219213848.png)

We should also add a local user using useradd.

```bash
# Here -m is used to create new home directory;
# -g is to create new primary group and add the user in it;
# -G is to add the user to another existing group which will provide sudo access to the user.
# username is the name of the user
useradd -m -g users -G wheel username

# set password for the created user
passwd username
```

![](Attachments/Pasted%20image%2020221219214133.png)

Now we also have to give group sudo privileges.

```bash
EDITOR=nano visudo
```

![](Attachments/Pasted%20image%2020221219214433.png)
![](Attachments/Pasted%20image%2020221219214402.png)

<ins>**DO NOT REBOOT UNTIL THE DESKTOP ENVRONMENT IS INSTALLED!!!**</ins>
