# kurs_otus

5 - Дисковая подсистема

vagrant up

root@node1:~# lsblk
NAME    MAJ:MIN RM  SIZE RO TYPE   MOUNTPOINT
loop0     7:0    0 63.7M  1 loop   /snap/core20/2434
loop1     7:1    0 91.9M  1 loop   /snap/lxd/29619
loop2     7:2    0 44.4M  1 loop   /snap/snapd/23545
sda       8:0    0   40G  0 disk
└─sda1    8:1    0   40G  0 part   /
sdb       8:16   0   10M  0 disk
sdc       8:32   0  100M  0 disk
└─md127   9:127  0  245M  0 raid10 /mnt/md127
sdd       8:48   0  100M  0 disk
└─md127   9:127  0  245M  0 raid10 /mnt/md127
sde       8:64   0  100M  0 disk
└─md127   9:127  0  245M  0 raid10 /mnt/md127
sdf       8:80   0  100M  0 disk
└─md127   9:127  0  245M  0 raid10 /mnt/md127
sdg       8:96   0  100M  0 disk
└─md127   9:127  0  245M  0 raid10 /mnt/md127
root@node1:~#
root@node1:~# mdadm -D /dev/md127
/dev/md127:
           Version : 1.2
     Creation Time : Mon Feb 24 19:41:37 2025
        Raid Level : raid10
        Array Size : 250880 (245.00 MiB 256.90 MB)
     Used Dev Size : 100352 (98.00 MiB 102.76 MB)
      Raid Devices : 5
     Total Devices : 5
       Persistence : Superblock is persistent

       Update Time : Mon Feb 24 19:43:00 2025
             State : clean
    Active Devices : 5
   Working Devices : 5
    Failed Devices : 0
     Spare Devices : 0

            Layout : near=2
        Chunk Size : 512K

Consistency Policy : resync

              Name : node1:127  (local to host node1)
              UUID : 33ed1e43:8a033f79:ddfc2785:ec4a45a2
            Events : 17

    Number   Major   Minor   RaidDevice State
       0       8       32        0      active sync   /dev/sdc
       1       8       48        1      active sync   /dev/sdd
       2       8       64        2      active sync   /dev/sde
       3       8       80        3      active sync   /dev/sdf
       4       8       96        4      active sync   /dev/sdg
root@node1:~#


/ansible-mdadm
playbook_mdraid.yml
ansible-playbook -i hosts.ini playbook_mdraid.yml --limit node1


root@vm-srv1:/home/rg/vm# tree
.
├── ansible
│   ├── ansible.cfg
│   ├── ansible-mdadm
│   │   ├── defaults
│   │   │   └── main.yml
│   │   ├── tasks
│   │   │   ├── arrays.yml
│   │   │   ├── debian.yml
│   │   │   ├── main.yml
│   │   │   └── wipe_disks.yml
│   │   ├── templates
│   │   │   └── etc
│   │   │       └── mdadm
│   │   │           └── mdadm.conf.j2
│   │   └── vars
│   │       └── main.yml
│   ├── ansible-nginx
│   │   └── nginx.conf.j2
│   ├── hosts.ini
│   ├── playbook_mdraid.yml
│   └── playbook_nginx.yml
├── vagrant
│   ├── Vagrantfile
├── lost+found
├── readme_DZ_1_2.md
├── readme_DZ_3.md
 