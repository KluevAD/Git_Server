# Создание собственного Git-сервера на Linux.
***
## Подготовка
Для начала Вам потребуется ОС Linux. 

Далее требуется установить Git на Ваш сервер.

    sudo apt update && sudo apt install git

## Настройка сервера
Создадим нового пользователя, который будет управлять репозиториями Git.

Для возможности входа в Git только с использованием ключей ssh создадим пользователя без пароля.

User - имя пользователя;

/home/user - домашний каталог пользователя, в котором будут храниться все репозитории.

    sudo useradd -r -m -U -d /home/User -s /bin/bash User

Переключаемся на созданного пользователя.

    sudo su User

Следующая команда перенесет пользователя в домашний каталог.

    cd

Для реализации входа по ssh следует создать каталог ssh, 
установить правильные разрешения и создать в этом каталоге файл, который будет хранить ключи авторизованных пользователей.

    mkdir -p ~/.ssh && chmod 0700 ~/.ssh
    
и
    
    touch ~/.ssh/authorized_keys && chmod 0600 ~/.ssh/authorized_keys

В созданный файл следует размещать ssh-ключи авторизованных пользователей.

На этом настройка сервера завершена.

Теперь нужно создать пустой репозиторий.

Project - имя репозитория.

    git init --bare ~/Project.git

Если все сделано правильно Вы увидите следующий вывод:

*Initialized empty Git repository in /home/User/Project.git/*

## Настройка локального компьютера
Требуется установить Git на Ваш локальный компьютер.

    sudo apt update && sudo apt install git

Далее создаем ssh-ключ.

    ssh-keygen -t rsa -b 4096

Сформированный ssh-ключ требуется доставить на сервер в ранее созданный файл "authorized_keys".

Один из вариантов:

Скопировать вывод команды и вставить в нужный файл.

    cat ~/.ssh/id_rsa.pub

**Важно!**

В файле "authorized_keys" ключ должен размещаться в **одну** строку.

Если у Вас есть активный проект, перемещаетесь в него. Если нет, создаете новый каталог.

После чего выполняете команду:

    git init

Далее нужно подключиться к Git-серверу.

    git remote add origin User@*Git-сервер IP*:Project.git

На данном моменте настройка локального компьютера закончена.

Если все шаги были выполнены верно при переносе изменений локального репозитория на сервер пароль запрашиваться не будет.











