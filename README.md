# MLOP28-Mezinova-Ansible
# Домашняя работа к занятию “Ansible”

Для реализации задания была создана инфраструктура с двумя docker-контейнерами:
1. Управляющая нода (Ansible)
2. Целевая нода, к которой Ansible подключается по SSH (далее нода будет называться Target).
   
Они находятся в одной Docker-сети, и Ansible выполняет playbook на второй ноде Target. Для каждой ноды был создан docketfile с требуемыми параметрами и собраны в docker-compose.
Также создан ansible.cfg 
````
[defaults]
host_key_checking = False
inventory = inventory.ini
```
Все файлы размещены на ноде ansible в папке ansible. 

* Homework.yaml
````
---
- name: Setup netology-ml target
  hosts: netology-ml
  become: true
  vars_files:
    - group_vars/all.yaml

  tasks:

    - name: Проверка пинга
      ansible.builtin.ping:

    - name: Обновление пакетов
      ansible.builtin.apt:
        update_cache: yes
        upgrade: dist

    - name: Установка пакетов
      ansible.builtin.apt:
        name: "{{ packages_to_install }}"
        state: present

    - name: Копирование файла test.txt
      ansible.builtin.copy:
        src: test.txt
        dest: /tmp/test.txt

    - name: Создание пользователей
      ansible.builtin.user:
        name: "{{ item.name }}"
        state: present
        create_home: yes
        shell: /bin/bash
      loop: "{{ users }}"
````

* inventory.ini
````
[netology-ml]
target ansible_user=ansible ansible_ssh_pass=ansible ansible_port=22 ansible_connection=ssh
````
* group_vars\all.yaml
````
packages_to_install:
  - net-tools
  - git
  - tree
  - htop
  - mc
  - vim

users:
  - name: devops_1
  - name: test_1
````


