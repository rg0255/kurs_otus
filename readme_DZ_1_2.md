# kurs_otus

1 - create Ubuntu 20.04 VM

===============================================================

2 - upgrade kernel Ubuntu 20.04

root@vm1-otus:/vagrant# uname -r
5.4.0-148-generic
root@vm1-otus:/vagrant#

root@vm1-otus:/vagrant# sudo apt update
root@vm1-otus:/vagrant# apt list --upgradable
root@vm1-otus:/vagrant# sudo apt -y upgrade

root@vm1-otus:/vagrant# sudo apt install --install-recommends linux-generic-hwe-20.04
root@vm1-otus:/vagrant# sudo reboot

vagrant@vm1-otus:~$ uname -r
5.15.0-126-generic

vagrant@vm1-otus:~$
root@vm1-otus:/home/vagrant# wget https://raw.githubusercontent.com/pimlie/ubuntu-mainline-kernel.sh/master/ubuntu-mainline-kernel.                       sh
root@vm1-otus:/home/vagrant# sudo install ubuntu-mainline-kernel.sh /usr/local/bin/
root@vm1-otus:/home/vagrant# ubuntu-mainline-kernel.sh -i
root@vm1-otus:/vagrant# sudo reboot

vagrant@vm1-otus:~$ uname -r
6.12.1-061201-generic
