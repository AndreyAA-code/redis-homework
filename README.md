Redis Ansible Role
==================

Роль для установки и настройки Redis в dev и prod окружениях.

Структура
---------

.
├── playbook.yaml                   # основной плейбук
├── secrets.yml                     # зашифрованный пароль Redis (Ansible Vault)
├── inventories/
│   ├── dev/                        # порт 6380, 4 БД, /mnt/redis, appendonly no
│   └── prod/                       # порт 6379, 16 БД, /var/lib/redis, appendonly yes
└── roles/redis/
    ├── defaults/main.yml           # значения по умолчанию
    ├── handlers/main.yml           # Restart Redis
    ├── tasks/main.yml              # задачи роли
    └── templates/redis.conf.j2     # Jinja2-шаблон конфига

Запуск
------

Dev:
    ansible-playbook playbook.yaml -i inventories/dev --ask-vault-pass

Prod:
    ansible-playbook playbook.yaml -i inventories/prod --ask-vault-pass

Пароль Vault можно передать через файл:

    echo "your-password" > ~/.vault_pass
    chmod 600 ~/.vault_pass
    ansible-playbook playbook.yaml -i inventories/dev --vault-password-file ~/.vault_pass

Переменные
----------

Переменная         dev          prod
redis_port         6380         6379
redis_databases    4            16
redis_dir          /mnt/redis   /var/lib/redis
redis_appendonly   no           yes
redis_password     в Vault      в Vault

Проверка
--------

    ansible-playbook playbook.yaml -i inventories/dev --ask-vault-pass
    ansible-playbook playbook.yaml -i inventories/dev --ask-vault-pass  # changed=0
