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

===============================================================

3 - Автоматизация администрирования. Ansible-1

Файлы
hosts.ini - inventory file
playbook.yml - ansible playbook file
ansible.cfg - ansible config file 
nginx.conf.j2 - nginx config file 

Хосты
vm1-otus - vm vagrant
vm-cli1 - vm

Проверка доступности хостов
ansible all -i hosts.ini -m ping
BECOME password: 
vm-cli1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
vm1-otus | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
root@vm-srv1:/home/rg/vm/vagrant/vm1# 

Выполнение на хостах в секции vagrant (в hosts.ini)
ansible-playbook -i hosts.ini playbook.yml --diff --limit vagrant
Выполнение на хостах в секции vm (в hosts.ini)
ansible-playbook -i hosts.ini playbook.yml --diff --limit vm

Выполнение на хосту
ansible-playbook -i hosts.ini playbook.yml --limit vm1-otus --diff
ansible-playbook -i hosts.ini playbook.yml --limit vm-cli1 --diff -b -k --ask-become-pass --become-user root -u rg


Повышение привелегий ползователя в команде (--ask-become-pass) добавление в ansible.cfg секцию
[privilege_escalation]
become_ask_pass = True
ansible-playbook -i hosts.ini playbook.yml --limit vm-cli1 --diff -b -k --become-user root -u rg

ansible-playbook -i hosts.ini playbook.yml --limit vm-cli1 -b -k --become-user root -u rg
ansible-playbook -i hosts.ini playbook.yml --limit vm-cli1 -b -k --ask-become-pass --become-user root -u rg

Выполнение shell
ansible vm1-otus -i hosts.ini -m shell -a "uname -a" -k -u vagrant
SSH password: 
BECOME password[defaults to SSH password]: 
vm1-otus | CHANGED | rc=0 >>
Linux vm1-otus 5.4.0-148-generic #165-Ubuntu SMP Tue Apr 18 08:53:12 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux

ansible vm-cli1 -i hosts.ini -m shell -a "uname -a" -b -k --become-user root -u rg
SSH password: 
BECOME password[defaults to SSH password]: 
vm-cli1 | CHANGED | rc=0 >>
Linux vm-cli1 5.4.0-204-generic #224-Ubuntu SMP Thu Dec 5 13:38:28 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux

===============================================================

