# Zabbix
## Установка PostgreSQL
### Создадим конфигурацию репозитория файлов:
<code>sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'</code>

### Импортируем ключ подписи репозитория:
<code>sudo wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -</code>

### Обновим списки пакетов:
<code>sudo apt-get update</code>

### Установим последнюю версию PostgreSQL.
### Если нам нужна конкретная версия, используем 'postgresql-12' или аналогичный вместо 'postgresql':
<code>sudo apt-get -y install postgresq</code>

### Проверим статус после установки
<code>systemctl status postgresql</code>

### Установим Netplan tools и проверим открытые порты
<code>sudo apt install net-tools</code>
### Должны увидеть открытый порт 5432 для PostgreSQL

## Установка Zabbix
### Устанавливаем репозиторий Zabbix
<code>sudo wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu22.04_all.deb</code>

<code>sudo dpkg -i zabbix-release_6.4-1+ubuntu22.04_all.deb</code>

<code>sudo apt update</code>

### Установим Zabbix сервер, интерфейс, агент
<code>sudo apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-nginx-conf zabbix-sql-scripts zabbix-agent</code>

### Создаём пользоватля "zabbix" и пароль для него
<code>sudo -u postgres createuser --pwprompt zabbix</code>

### Создаем БД  "zabbix"
<code>sudo -u postgres createdb -O zabbix zabbix</code>

### На хост сервера Zabbix импортируем исходную схему и данные. Нам будет предложено ввести только что созданный пароль.
<code>sudo zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix</code>

<code></code>
<code></code>
<code></code>
<code></code>
<code></code>
