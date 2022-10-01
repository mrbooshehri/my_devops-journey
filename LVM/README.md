# Create a LVM partition

1. Create pv (physical volume)
```bash
pvcreate /dev/sdX
```

2. Check the ```pv``` status use one of the following
```bash
pvscan
# or
pvdisplay
# or
pvs
```

3. Create volume group
```bash
vgcreate <vg_name> <physical volume>
# vgcreate vg01 /dev/sdX
```

4. Check the ```vg``` status use one of the following
```bash
vgscan
# or
vgdisplay
# or
vgs
```

5. Create logical vomlue
```bash
lvcreate -L <Size> -n <lv_name>  <vg_name>
# lvcreate -L 9.99G -n lv01 vg01
```

6. Check the ```vg``` status use one of the following
```bash
lvscan
lvdisplay
lvs
```

7. make the proper filesystem
```bash
mkfs.ext4 /dev/sdX
```

8. get the ```lv``` path and mount it into fstab
```bash
lvdisplay
```

Related:
```
* https://www.linuxbuzz.com/create-lvm-partition-in-linux/
```

# Extend LVM

## Scan added list
```bash
rescan-scsi-bus.sh -a
```
## Create pv
```bash
pvcreate /dev/sdc
```
## Extend volume group
```bash
vgextend ubuntu-vg /dev/sdc
```
## Extend logical volumek
```bash
lvextend /dev/ubuntu-vg/ubuntu-lv -L 75G
```
## resize filysystem
```bash
resize2fs /dev/ubuntu-vg/ubuntu-lv
```

