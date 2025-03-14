# Ansible
 Автоматизация администрирования. Ansible-1 
Домашнее задание по теме “Первые шаги с Ansible”

Цель домашнего задания: Написать первые шаги с Ansible.

Описание домашнего задания.
Подготовить стенд на Vagrant как минимум с одним сервером. На этом сервере, используя Ansible необходимо развернуть nginx со следующими условиями:
    • необходимо использовать модуль yum/apt
    • конфигурационный файлы должны быть взяты из шаблона jinja2 с переменными
    • после установки nginx должен быть в режиме enabled в systemd
    • должен быть использован notify для старта nginx после установки
    • сайт должен слушать на нестандартном порту - 8080, для этого использовать переменные в Ansible

 После установки необходимых программ видим версию ansible:

Для управления хостами Ansible использует SSH соединение. Поэтому перед стартом необходимо убедиться что у Вас есть доступ до управляемых хостов. ﻿﻿Также на управляемых хостах должен быть установлен Python 2.X.

Создайте каталог Ansible и положите в него этот Vagrantfile
https://drive.google.com/file/d/17MEtg20TFSjKil6ih7PvPez7jmCvo6fb/view?usp=share_link
● Поднимите управляемый хост командой vagrant up и убедитесь, что все прошло успешно и есть доступ по ssh
● Для подключения к хосту nginx нам необходимо будет передать множество параметров - это особенность Vagrant. Узнать эти параметры можно с помощью команды vagrant ssh-config. Вот основные необходимые нам:
Host nginx имя хоста
HostName 127.0.0.1 IP адрес
User vagrant имя пользователя под которым подключаемся
Port 2222 порт, который проброшен на 127.0.0.1
IdentityFile .vagrant/machines/nginx/virtualbox/private_key 
путь до приватного ключа
Создадим свой первый inventory файл ./staging/hosts
Со следующим содержимым:
[web]nginx ansible_host=127.0.0.1 ansible_port=2222  ansible_private_key_file=.vagrant/machines/nginx/virtualbox/private_key
В текущем каталоге создадим файл ansible.cfg со следующим содержанием:
[defaults]
inventory = staging/hosts
remote_user = vagrant
host_key_checking = False
retry_files_enabled = False
И наконец убедимся, что Ansible может управлять нашим хостом.

      Теперь, когда мы убедились, что у нас все подготовлено - установлен
Ansible, поднят хост для теста и Ansible имеет к нему доступ, мы можем
конфигурировать наш хост.
      Для начала воспользуемся Ad-Hoc командами и выполним некоторые
удаленные команды на нашем хосте.
      Посмотрим какое ядро установлено на хосте:

Проверим статус сервиса firewalld

После выполнения всех инструкций получился такой файл:

Запустил файл, он отработал на 100%:

Проверил работу сервиса на виртуальной машине:
