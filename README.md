# zabbix
# Создадим конфигурацию репозитория файлов:
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

# Импортируем ключ подписи репозитория:
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

# Обновите списки пакетов:
sudo apt-get update

# Установите последнюю версию PostgreSQL.
# Если вам нужна конкретная версия, используйте 'postgresql-12' или аналогичный вместо 'postgresql':
sudo apt-get -y install postgresq
