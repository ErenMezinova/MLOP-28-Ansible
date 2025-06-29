# MLOP28-Mezinova-Ansible
# Домашняя работа к занятию “Ansible”

* Homework.yaml
````
---
- name: Netology ML Playbook
  hosts: netology-ml
  become: true
  vars_files:
    - group_vars/all.yaml

  tasks:

    - name: Проверка доступности хоста
      ansible.builtin.ping:

    - name: Обновление пакетов
      ansible.builtin.apt:
        update_cache: yes

    - name: Установка необходимых пакетов
      ansible.builtin.apt:
        name: "{{ packages_to_install }}"
        state: present
      
    - name: Копирование файла test.txt
      ansible.builtin.copy:
        src: test.txt
        dest: /tmp/test.txt
        mode: '0644'

    - name: Создание пользователей и их home-директорий
      ansible.builtin.user:
        name: "{{ item.name }}"
        state: present
        shell: /bin/bash
        create_home: yes
      loop: "{{ users }}"
````

* inventory.ini
````
[netology-ml]
192.168.100.10 ansible_user=ansible ansible_ssh_private_key_file=/root/.ssh/id_rsa
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


