### Node Prerequisite

#### System Upgrade & Install

1. We need to upgrade existing distribution & install qemu-guest-agent on each node(s) :
```
sudo apt-get upgrade -y && sudo apt-get install qemu-guest-agent -y && sudo shutdown -r now
```

1. Wipe disk(s) used for docker-storage and glusterfs Node(s) :

  **a. node oso-master.mpti.co.id**

  - check available disk(s)
```
mptadmin@oso-master:~$ lsblk
NAME                       MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                          8:0    0   80G  0 disk
├─sda1                       8:1    0  487M  0 part /boot
├─sda2                       8:2    0    1K  0 part
└─sda5                       8:5    0 79.5G  0 part
  ├─oso--master--vg-root   252:0    0 63.5G  0 lvm  /
  └─oso--master--vg-swap_1 252:1    0   16G  0 lvm  [SWAP]
sdb                          8:16   0  100G  0 disk
sr0                         11:0    1  825M  0 rom
mptadmin@oso-master:~$
```
used /dev/sdb as docker storage  , we need to wipe the disk(s)
```
mptadmin@oso-master:~$ sudo dd if=/dev/zero of=/dev/sdb bs=512 count=1
[sudo] password for mptadmin:
1+0 records in
1+0 records out
512 bytes copied, 0.0318988 s, 16.1 kB/s
```
  **b. node oso-infra.mpti.co.id**

  - check available disk(s)
```
mptadmin@oso-infra:~$ lsblk
NAME                       MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                          8:0    0   80G  0 disk
├─sda1                       8:1    0  487M  0 part /boot
├─sda2                       8:2    0    1K  0 part
└─sda5                       8:5    0 79.5G  0 part
  ├─oso--master--vg-root   252:0    0 63.5G  0 lvm  /
  └─oso--master--vg-swap_1 252:1    0   16G  0 lvm  [SWAP]
sdb                          8:16   0  100G  0 disk
sr0                         11:0    1  825M  0 rom
mptadmin@oso-infra:~$
```
used **/dev/sdb** as docker storage  , we need to wipe the disk(s)
```
mptadmin@oso-master:~$ sudo dd if=/dev/zero of=/dev/sdb bs=512 count=1
[sudo] password for mptadmin:
1+0 records in
1+0 records out
512 bytes copied, 0.0318988 s, 16.1 kB/s
```
**c. node oso-[n0-n2].mpti.co.id**

  - check available disk(s)
    ```
mptadmin@oso-n0:~$ lsblk
NAME                   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                      8:0    0   80G  0 disk
├─sda1                   8:1    0  487M  0 part /boot
├─sda2                   8:2    0    1K  0 part
└─sda5                   8:5    0 79.5G  0 part
  ├─oso--n0--vg-root   252:0    0 69.5G  0 lvm  /
  └─oso--n0--vg-swap_1 252:1    0   10G  0 lvm  [SWAP]
sdb                      8:16   0  200G  0 disk
sdc                      8:32   0  100G  0 disk
sr0                     11:0    1  825M  0 rom
```
used **/dev/sdc** as docker storage and **/dev/sdb** as glusterfs , we need to wipe the disk(s)

    ```
mptadmin@oso-n0:~$ sudo dd if=/dev/zero of=/dev/sdb bs=512 count=1
[sudo] password for mptadmin:
1+0 records in
1+0 records out
512 bytes copied, 0.0410317 s, 12.5 kB/s
mptadmin@oso-n0:~$ sudo dd if=/dev/zero of=/dev/sdc bs=512 count=1
1+0 records in
1+0 records out
512 bytes copied, 0.0372588 s, 13.7 kB/s
mptadmin@oso-n0:~$
```
