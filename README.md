# VDS
Этот репозиторий содержит описание конфигурации VDS при помощи исходного кода (подход Infrastructure as Code). В частности, с использованием Ansible.

## Примечание
Разработка плейбуков осуществлялась для узлов с дистрибутивом Debian 10.

## Требования
- Установленный Ansible на Control node (хост, с которого будут запускаться команды для конфигурирования VDS, например, localhost), [см. инструкцию](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- Наличие VDS или виртуальной машины, для которой будут запускаться команды для конфигурирования
- Подключение по SSH к VDS или виртуальной машине

## Пример конфигурации /etc/ansible/hosts
Настроенный /etc/ansible/hosts на Control node нужен для подключения к VDS или виртуальной машине, если файл inventory.yaml ещё не описан, [см. инструкцию](https://docs.ansible.com/ansible/latest/getting_started/index.html)

```
[vds_prod]
fvds ansible_host=192.168.1.1

# или так:

[vds_prod]
fvds ansible_host=example.com
```

Проверка соединения:

```
ansible all -m ping
fvds | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

## Создание зашифрованного пароля для пользователя
```
apt update
apt install -y python3 python3-bcrypt
python3 -c 'import crypt,getpass;pw=getpass.getpass();print(crypt.crypt(pw) if (pw==getpass.getpass("Confirm: ")) else exit())'
```
