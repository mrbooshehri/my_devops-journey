# Create a LVM partition

1. Create pv (physical volume)
```bash
pvcreate /dev/sdX
```

2. Check the ```pv``` status use one of the following
```bash
pvscan
pvdisplay
pvs
```

3. Create volume group
```bash
vgcreate  <vg_name>  <pv> <physical volume>
```

4. Check the ```vg``` status use one of the following
```bash
vgscan
vgdisplay
vgs
```

5. Create logical vomlue
```bash
lvcreate -L <Size> -n <lv_name>  <vg_name>
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
