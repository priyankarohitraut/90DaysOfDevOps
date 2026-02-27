
Task 1: Check Current Storage

Run: lsblk, pvs, vgs, lvs, df -h

Task 2: Create Physical Volume
pvcreate /dev/sdb   # or your loop device
pvs
yes

Task 3: Create Volume Group
vgcreate devops-vg /dev/
sdb sudo pvcreate /dev/loop17
sudo pvssudo pvcreate /dev/loop17

Task 4: Create Logical Volume
lvcreate -L 500M -n app-data devops-vg
lvs
sudo lvcreate -L 500M -n app-data devops-vg
sudo lvs

Task 5: Format and Mount
mkfs.ext4 /dev/devops-vg/app-data
mkdir -p /mnt/app-data
mount /dev/devops-vg/app-data /mnt/app-data
df -h /mnt/app-data
yes i did it
but used sudo command also


Task 6: Extend the Volume
lvextend -L +200M /dev/devops-vg/app-data
resize2fs /dev/devops-vg/app-data
df -h /mnt/app-data
yes 




