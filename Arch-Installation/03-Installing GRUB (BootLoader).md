Now we have to install GRUB bootloader using command mentioned below.

```bash
pacman -S grub efibootmgr dosfstools os-prober mtools
```

![](Attachments/Screenshot%20from%202022-12-19%2022-14-30.png)

Create new directory _/boot/EFI_ and mount EFI partition to that location.

```bash
mkdir /boot/EFI
mount /dev/sda1 /boot/EFI
```

![None](Attachments/Pasted%20image%2020221219221945.png)

We have to install the GRUB according to the type of installation we have done. (In this example we have done it with uefi)

```bash
grub-install --target=x86_64-efi --bootloader-id==grub_uefi --recheck
```

![](Attachments/Pasted%20image%2020221219222236.png)

To view grub messages in english language.

```bash
cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo
```

![](Attachments/Pasted%20image%2020221219222436.png)

Generate the GRUB configuration

```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

![](Attachments/Pasted%20image%2020221219222745.png)

<ins>**Note:**</ins> After generating grub config if you get some warning regarding `os-prober` won't be detecting the other os then excute following commands

```bash
nano /etc/default/grub

# Uncomment or add "GRUB_DISABLE_OS_PROBER=false" (without quotes "") if not present and save and rerun the grub-mkconfig command
```

<ins>**DO NOT REBOOT UNTIL THE DESKTOP ENVRONMENT IS INSTALLED!!!**</ins>
