`sudo blkid | grep sdb`
![](Assets/Pasted%20image%2020230104152137.png)

#### Formatting

![](Assets/Pasted%20image%2020230104153740.png)

![](Assets/Pasted%20image%2020230104155119.png)

![](Assets/Pasted%20image%2020230104155315.png)

![](Assets/Pasted%20image%2020230104155742.png)

## To add partitions into fstab file (Important if we want our partitions to work after we reboot/ shutdown).

![](Assets/Pasted%20image%2020230104164132.png)

$ `sudo nano /etc/fstab`
![](Assets/Pasted%20image%2020230104164250.png)

![](Assets/Pasted%20image%2020230104164404.png)

## Unmounting

![](Assets/Pasted%20image%2020230104160327.png)

![](Assets/Pasted%20image%2020230104160443.png)

_Note_: Never forget to remove fstab entries after unmounting and removing the block devices.
