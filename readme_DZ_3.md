# kurs_otus

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
