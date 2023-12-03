## 5.1 KDE PLASMA

First install all the kde plasmna packages.

```bash
sudo pacman -S plasma-meta kde-applications
```

![](Attachments/Pasted%20image%2020221220172406.png)

Now, enable display manager.

```bash
sudo systemctl enable sddm
```

![](Attachments/Pasted%20image%2020221220173032.png)

Reboot.

## 5.2 GNOME Desktop

First install all the packages for gnome.

```bash
sudo pacman -S gnome gnome-tweaks
```

![](Attachments/Pasted%20image%2020221220180941.png)

Then Enable the GDM service.

```bash
sudo systemctl enable gdm
```

Reboot.

## 5.3 XFCE Desktop

Install the xfce packages.

```bash
sudo pacman -S xfce4 xfce4-goodies
```

![](Attachments/Pasted%20image%2020221221174954.png)

also install the display manager. (In this case install 'lightdm')

```bash
sudo pacman -S lightdm
```

![](Attachments/Pasted%20image%2020221221175320.png)

```bash
sudo systemctl enable lightdm
```

Now, enable the lidhtdm service.
![](Attachments/Pasted%20image%2020221221175359.png)

Reboot.
