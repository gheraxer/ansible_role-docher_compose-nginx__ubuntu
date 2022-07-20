Role Name  - deploy_docker_nginx
данный плэйбук работает только на Debian
Позже доработаю его и будет годен и для RED HAT.

=========
Роль так настроена.

конфиг инсибла что находится по пути
/etc/ansible/ansible.cfg
нужно добавить эти строки    (чтобы каждый раз не писать -i во время вызова плэйбука.) 
*************************/etc/ansible/ansible.cfg******************************
[defaults]
host_key_cheking = false
inventory        = /home/{{user_name}}/ansible/hosts.txt
#####(всё верно, это отдельно созданная папка для ансибла и указал её по умолчанию, в следующих версиях надо будет это поправить).

А что находится в hosts.txt 
*****************************
[test]
linux ansible_host=IP_машины. 

[test:vars]
ansible_user=root
ansible_ssh_private_key_file=/home/ubuntu/.ssh/test.pem
*************************************************************************

Ну а теперь, находясь в папке /home/{{user_name}}/ansible/docker_insatal.yml"
запускаем плэйбук
ansible-playbook docker_insatal.yml

внутри которого 
*********************docker_insatal.yml*********************
---
- hosts: all
  remote_user: root
  become: true
  roles:
   - { role: deploy_docker_nginx, when: ansible_distribution_file_variety == "Debian" }
************************************************************
этого я не добавил в гит, так же есть над чем поработать и исправить. 



Example Playbook   
Кусочек таска*
- name: Run docker-compose
  become: true
  shell:
    cmd: "docker-compose up --build -d"  


----------------

если нужно заменить изображение, тогда в папку 
/ansible/deploy_docker_nginx/files/
кидаем картинку с именем " 1.jpg " .
Если нужно больше колличество - тогда нужно в task/main.yml  "name: Copy JPG and Index "
добавить после
loop:
  - "имя фаила_что_расположен_в_files"
     


Author Information
------------------
Герман Осипов 
Это мой первый проэкт в Гите для Ansible, но мне понравилось.

