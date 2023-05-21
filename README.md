# Zabbix
## Установка PostgreSQL
### Создадим конфигурацию репозитория файлов:
<code>sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'</code>

### Импортируем ключ подписи репозитория:
<code>wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -</code>

### Обновите списки пакетов:
<code>sudo apt-get update</code>

### Установите последнюю версию PostgreSQL.
### Если вам нужна конкретная версия, используйте 'postgresql-12' или аналогичный вместо 'postgresql':
<code>sudo apt-get -y install postgresq</code>
