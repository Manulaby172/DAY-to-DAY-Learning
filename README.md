# DAY-to-DAY-Learning

Oct-9 "Storage in Linux"


/       Root of the filesystem
/bin    Essential user binaries (ls, cat, etc.)
/sbin   System binaries (shutdown, mount, etc.)
/etc    Configuration files
/home   User home directories
/var    Variable data (logs, spool, cache)
/tmp    Temporary files
/usr    User programs, libraries
/lib    Shared libraries needed by binaries
/boot   Kernel and bootloader files
/dev    Device files (disks, terminals)
/proc   Virtual filesystem for process info
/mnt & /media   Mount points for external drives

# Created a VM using Virtual Box. Attached another physical disk
# block device can be found under /dev directory  ( lsblk, ls -l /dev, sudo fsidk -l /dev/sda )
# Partitions, ( Creating partitions use gdisk /dev/vde )
#to write to a disk or partition we must create a file system and then mount the system.
#use (blkid) command to see file system type.
#(to create a file system-- "mkfs.ext4 /dev/sda", to mount it "mount /dev/sda /mnt/data" )  The mount is not persistent in FSTAB yet. If the system is rebooted, the filesystem will not be mounted.
Add it in the FSTAB

Run: sudo vi /etc/fstab

Add the line /dev/vde /mnt/data ext4 rw 0 0

Save and Exit.

#inode-- An inode stores information about the file, but it does not store the file's contents or its filename.


LVM---
once LV is created we need to create a file system and



---------- OCT 10 (Day-2)

If yum is not working due to unreachable, then DNS issue, config DNSfile-- /etc/resolv.confg.

Add nameserver=8.8.8.8 to /etc/resolv.confg
Then try installing the required app using yum/dnf (Centos).

Enabling the installed app, (sudo systemctl enable --now firewalld)

Configuring the Firewall Rules
sudo firewall-cmd --permanent --add-port=22/tcp
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --permanent --add-port=8081/tcp
sudo firewall-cmd --reload

nohup go command--- When you run a Go program with nohup, it means you’re starting it in the background and making sure it won’t stop if you log out of your terminal session.

(nohup → prevents the process from being killed when you log out or close the terminal.

go run main.go → runs your Go program.

& → sends it to the background immediately.)



Day 3 --

group add, (sudo groupadd name )

to config resource limit" for the "devs" group so that this group (members of the group) can not run more than "30 processes" in their session. This should be both a "hard limit" and a "soft limit" ---- ( edit file /etc/security/ limits.d/devs.conf)
Add into the fille (nproc) --  \
@devs   hard   nproc   30
@devs   soft   nproc   30
or @devs - nproc 30


to run only the dnf command via sudo without entering a password, you need to configure a sudoers rule inside /etc/sudoers.d ( always use visudo for editing sudoers)

Sudo visudo /etc/sudoers.d/devs

and then add ( %devs ALL=(ALL) NOPASSWD: /usr/bin/dnf )
%devs applies the rule to all members of the devs group.

NOPASSWD: allows running the command without a password prompt.

/usr/bin/dnf specifies the exact path to the allowed command.

setting quota inside group ---- sudo setquota -g devs 100M 500M 0 0 /dev/vdb1

To set the ownership and permissions for the /data directory as specified, you need to use two commands: chown (to change ownership to user bob and group devs) and chmod (to set full permissions for user and group, and none for others).

bob: Sets the user owner.

devs: Sets the group owner.

/data: The target directory.



sudo setfacl -R -m g:admins:rwx /data  ((To give the admins group full permissions on the /data directory without changing the existing primary group owner (devs), you can use POSIX Access Control Lists (ACLs). This allows you to grant specific permissions to an additional group.
