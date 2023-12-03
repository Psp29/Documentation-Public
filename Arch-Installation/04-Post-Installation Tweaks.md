## Adding Swap space.

Create a swapfile

```bash
# here we have created swapfile of 16GB as the installed RAM is 16GB
dd if=/dev/zero of=/swapfile bs=1M count=16384 status=progress
```

![](Attachments/Pasted%20image%2020221220155525.png)

```bash
chmod 600 /swapfile
mkswap /swapfile
```

![](Attachments/Pasted%20image%2020221220155654.png)

Adding swap entry in fstab file.

```bash
echo '/swapfile none swap sw 0 0' | tee -a /etc/fstab
```

![](Attachments/Pasted%20image%2020221220155935.png)

To check if the swap is active or not we can use command _free -m_ command.

```bash
free -m
```

![](Attachments/Pasted%20image%2020221220160042.png)

Now we have to activate the swap.

```bash
swapon -a
free -m
```

![](Attachments/Pasted%20image%2020221220160223.png)

## Changing timezones.

First we have to find the desired (or near region) timezone.

```bash
timedatectl list-timezones | grep "Kolkata"
```

![](Attachments/Pasted%20image%2020221220160839.png)

now change the timezone.

```bash
timedatectl set-timezone Asia/Kolkata
```

![](Attachments/Pasted%20image%2020221220161203.png)

Activate/Enable the timedate service.

```bash
systemctl enable systemd-timesyncd
```

![](Attachments/Pasted%20image%2020221220161336.png)

## Setting hostname and configuring hosts file.

```bash
hostnamectl set-hostname charlinux
cat /etc/hostname
```

![](Attachments/Pasted%20image%2020221220161558.png)

## Installing CPU-specific packages

if host system consists of amd cpu the package name is `amd-ucode` and for intel users name is `intel-ucode` (either installing bare metal or on a hypervisor the package is important).
![](Attachments/Pasted%20image%2020221220162059.png)
![](Attachments/Pasted%20image%2020221220162434.png)

### Installing xorg-server

```bash
pacman -S xorg-server
```

![](Attachments/Pasted%20image%2020221220162339.png)

### \*_Only for VMWare and Virtualbox users._

\*Only if installing on Virtualbox
Also enable the virtualbox service.
![](Attachments/Pasted%20image%2020221220163153.png)

_For Vmware workstation_
![](Attachments/Pasted%20image%2020221220163237.png)
Also enable respective services
![](Attachments/Pasted%20image%2020221220163444.png)
For more configurations please refer official archwiki [post](https://wiki.archlinux.org/title/VMware/Install_Arch_Linux_as_a_guest#VMware_Tools_versus_Open-VM-Tools).

<ins>**DO NOT REBOOT UNTIL THE DESKTOP ENVRONMENT IS INSTALLED!!!**</ins>
