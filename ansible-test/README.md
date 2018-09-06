[root@test-server ansible-test]# ansible -i hosts --key-file=sshkeys/ivp-server -m shell -a "date;hostname" -u root all
test-server2 | SUCCESS | rc=0 >>
Sat Jun 23 19:25:19 UTC 2018
test-server2

test-server1 | SUCCESS | rc=0 >>
Sat Jun 23 19:25:19 UTC 2018
test-server1

test-server3 | SUCCESS | rc=0 >>
Sat Jun 23 19:25:19 UTC 2018
test-server3

[root@test-server ansible-test]# ansible -i hosts -m ping all
test-server2 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
test-server3 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}
test-server1 | SUCCESS => {
    "changed": false, 
    "ping": "pong"
}

ansible -i hosts -m shell -a "whoami" -u root all
test-server2 | SUCCESS | rc=0 >>
root

test-server1 | SUCCESS | rc=0 >>
root

test-server3 | SUCCESS | rc=0 >>
root

#ansible -i hosts -m shell -a "getent passwd | grep root" all
test-server2 | SUCCESS | rc=0 >>
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
dockerroot:x:997:994:Docker User:/var/lib/docker:/sbin/nologin

test-server3 | SUCCESS | rc=0 >>
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
dockerroot:x:997:994:Docker User:/var/lib/docker:/sbin/nologin

test-server1 | SUCCESS | rc=0 >>
root:x:0:0:root:/root:/bin/bash
operator:x:11:0:operator:/root:/sbin/nologin
dockerroot:x:997:994:Docker User:/var/lib/docker:/sbin/nologin

==========
### Adding roles : rolename: lb
=========

cat playbook.yaml 
---
- hosts: all
  become: true
  roles:
  - lb
---

#cat hosts
test-server1
test-server2
test-server3

# ansible-playbook -i hosts playbook.yaml 


cat lb/tasks/main.yml
---
# tasks file for lb
- name: "installing VIM"
  yum: pkg=vim state=installed

- name: "installing tcpdump"
  yum: pkg=tcpdump state=installed

- name: "installing multiple softwares"
  yum: pkg={{ item }} state=installed
  with_items:
  - vim
  - ntp
  - tcpdump
  - at

