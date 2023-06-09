# GB Системы управления конфигурациями

## Урок 3. Используем Ansible на практике (base)

### Домашнее задание №3

Установка приложения MySQL + WordPress + FPM + nginx.

_playbook.yml_

```yaml
---
- name: Install datablese
  become: true
  hosts: db
  roles:
    - mysql

- name: Install php
  become: true
  hosts: php
  roles:
    - php

- name: Install nginx and wordpress
  become: true
  hosts: nginx
  roles:
    - nginx
    - wordpress
```

_inventory hosts.yml_

```yaml
---
YNDX:
  vars:
    ansible_user: ansible
    ansible_port: 22
    ansible_ssh_private_key_file: '{{ inventory_dir }}/group_vars/id_ed25519'
  hosts:
    db:
      ansible_host: 158.160.17.110
    nginx:
      ansible_host: 158.160.17.110
    php:
      ansible_host: 158.160.17.110
```

_deploy_

`ansible-playbook playbook.yml`

or

`ansible-playbook playbook.yml  --flush-cache`

_logs_

```log
2023-03-21 19:41:58,345 p=29456 u=agat n=ansible | PLAY [Install datablese] **************************************************************************************************************************************************************************
2023-03-21 19:41:58,360 p=29456 u=agat n=ansible | TASK [Gathering Facts] ****************************************************************************************************************************************************************************
2023-03-21 19:42:00,062 p=29456 u=agat n=ansible | ok: [db]
2023-03-21 19:42:00,090 p=29456 u=agat n=ansible | TASK [mysql : Role mysql | install mariadb] *******************************************************************************************************************************************************
2023-03-21 19:43:09,934 p=29456 u=agat n=ansible | changed: [db]
2023-03-21 19:43:09,946 p=29456 u=agat n=ansible | TASK [mysql : Role mysql | install pymysql using pip] *********************************************************************************************************************************************
2023-03-21 19:43:12,332 p=29456 u=agat n=ansible | changed: [db]
2023-03-21 19:43:12,340 p=29456 u=agat n=ansible | TASK [mysql : Role mysql | Started mariadb service] ***********************************************************************************************************************************************
2023-03-21 19:43:13,054 p=29456 u=agat n=ansible | ok: [db]
2023-03-21 19:43:13,063 p=29456 u=agat n=ansible | TASK [mysql : Role mysql | Set the root password] *************************************************************************************************************************************************
2023-03-21 19:43:13,564 p=29456 u=agat n=ansible | changed: [db]
2023-03-21 19:43:13,586 p=29456 u=agat n=ansible | TASK [mysql : Role mysql | remove anonymous user accounts] ****************************************************************************************************************************************
2023-03-21 19:43:13,971 p=29456 u=agat n=ansible | ok: [db]
2023-03-21 19:43:13,979 p=29456 u=agat n=ansible | TASK [mysql : Role mysql | remove the MySQL test database] ****************************************************************************************************************************************
2023-03-21 19:43:14,486 p=29456 u=agat n=ansible | ok: [db]
2023-03-21 19:43:14,499 p=29456 u=agat n=ansible | TASK [mysql : Role mysql | create database] *******************************************************************************************************************************************************
2023-03-21 19:43:14,779 p=29456 u=agat n=ansible | changed: [db]
2023-03-21 19:43:14,801 p=29456 u=agat n=ansible | TASK [mysql : Role mysql | create MySQL admin user for database] **********************************************************************************************************************************
2023-03-21 19:43:15,200 p=29456 u=agat n=ansible | changed: [db]
2023-03-21 19:43:15,244 p=29456 u=agat n=ansible | PLAY [Install php] ********************************************************************************************************************************************************************************
2023-03-21 19:43:15,254 p=29456 u=agat n=ansible | TASK [Gathering Facts] ****************************************************************************************************************************************************************************
2023-03-21 19:43:16,030 p=29456 u=agat n=ansible | ok: [php]
2023-03-21 19:43:16,064 p=29456 u=agat n=ansible | TASK [php : Role php | install PHP server and plugins] ********************************************************************************************************************************************
2023-03-21 19:43:39,674 p=29456 u=agat n=ansible | changed: [php]
2023-03-21 19:43:39,682 p=29456 u=agat n=ansible | TASK [php : Role php | start service] *************************************************************************************************************************************************************
2023-03-21 19:43:40,194 p=29456 u=agat n=ansible | ok: [php]
2023-03-21 19:43:40,235 p=29456 u=agat n=ansible | PLAY [Install nginx and wordpress] ****************************************************************************************************************************************************************
2023-03-21 19:43:40,247 p=29456 u=agat n=ansible | TASK [Gathering Facts] ****************************************************************************************************************************************************************************
2023-03-21 19:43:41,034 p=29456 u=agat n=ansible | ok: [nginx]
2023-03-21 19:43:41,078 p=29456 u=agat n=ansible | TASK [nginx : Role nginx | install nginx] *********************************************************************************************************************************************************
2023-03-21 19:43:47,258 p=29456 u=agat n=ansible | changed: [nginx]
2023-03-21 19:43:47,295 p=29456 u=agat n=ansible | TASK [wordpress : Role wordpress | Download and unpack latest WordPress] **************************************************************************************************************************
2023-03-21 19:43:53,542 p=29456 u=agat n=ansible | changed: [nginx]
2023-03-21 19:43:53,550 p=29456 u=agat n=ansible | TASK [wordpress : Role wordpress | Set ownership] *************************************************************************************************************************************************
2023-03-21 19:43:54,114 p=29456 u=agat n=ansible | changed: [nginx]
2023-03-21 19:43:54,125 p=29456 u=agat n=ansible | TASK [wordpress : Role wordpress | Add site to sites folder in Nginx] *****************************************************************************************************************************
2023-03-21 19:43:54,990 p=29456 u=agat n=ansible | changed: [nginx]
2023-03-21 19:43:55,011 p=29456 u=agat n=ansible | TASK [wordpress : Role wordpress | remove default page] *******************************************************************************************************************************************
2023-03-21 19:43:55,279 p=29456 u=agat n=ansible | changed: [nginx]
2023-03-21 19:43:55,297 p=29456 u=agat n=ansible | TASK [wordpress : Role wordpress | add wp-config] *************************************************************************************************************************************************
2023-03-21 19:43:56,027 p=29456 u=agat n=ansible | changed: [nginx]
2023-03-21 19:43:56,049 p=29456 u=agat n=ansible | TASK [wordpress : Role wordpress | activate site] *************************************************************************************************************************************************
2023-03-21 19:43:56,299 p=29456 u=agat n=ansible | changed: [nginx]
2023-03-21 19:43:56,352 p=29456 u=agat n=ansible | RUNNING HANDLER [wordpress : Service | reload nginx] **********************************************************************************************************************************************
2023-03-21 19:43:56,789 p=29456 u=agat n=ansible | changed: [nginx]
2023-03-21 19:43:56,825 p=29456 u=agat n=ansible | PLAY RECAP ****************************************************************************************************************************************************************************************
2023-03-21 19:43:56,825 p=29456 u=agat n=ansible | db                         : ok=9    changed=5    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
2023-03-21 19:43:56,825 p=29456 u=agat n=ansible | nginx                      : ok=9    changed=8    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
2023-03-21 19:43:56,826 p=29456 u=agat n=ansible | php                        : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
