# Zabbix Server

## Установка PostgreSQL
### Создадим конфигурацию репозитория файлов:
<code>sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'</code>
### Импортируем ключ подписи репозитория:
<code>sudo wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -</code>
### Обновим списки пакетов:
<code>sudo apt-get update</code>
### Установим последнюю версию PostgreSQL.
### Если нам нужна конкретная версия, используем 'postgresql-12' или аналогичный вместо 'postgresql':
<code>sudo apt-get -y install postgresql</code>
### Проверим статус после установки
<code>sudo systemctl status postgresql</code>
### Установим Netplan tools и проверим открытые порты
<code>sudo apt install net-tools</code>

<code>sudo netstat -tulpn</code>
### Должны увидеть открытый порт 5432 для PostgreSQL
## Установка Zabbix
### Устанавливаем репозиторий Zabbix
<code>sudo wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu20.04_all.deb</code>

<code>sudo dpkg -i zabbix-release_6.4-1+ubuntu22.04_all.deb</code>

<code>sudo apt update</code>
### Установим Zabbix сервер, интерфейс, агент
<code>sudo apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-nginx-conf zabbix-sql-scripts zabbix-agent</code>
### Создаём пользоватля "zabbix" и пароль для него
<code>sudo -u postgres createuser --pwprompt zabbix</code>
### Создаем БД  "zabbix"
<code>sudo -u postgres createdb -O zabbix zabbix</code>
### На хост сервера Zabbix импортируем исходную схему и данные. Нам будет предложено ввести только что созданный пароль. Придется подождать пока все импортирует
<code>sudo zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix</code>
### Зададим пароль для БД. Для этого заходим в конфигурационный файл раскоментим <code>DBPassword=</code> и пропишем пароль
<code>sudo nano /etc/zabbix/zabbix_server.conf</code>
### Настроим PHP для интерфейса Zabbix. Отредактируем файл **nginx.conf**, раскомментируем и установии директивы **listen** и **server_name**

<code>sudo nano /etc/zabbix/nginx.conf</code>

<code>listen 8080;</code>

<code>server_name example.com;</code>
### Запускаем сервер Zabbix и процессы агента
<code>sudo systemctl restart zabbix-server zabbix-agent nginx php8.1-fpm</code>

<code>sudo systemctl enable zabbix-server zabbix-agent nginx php8.1-fpm</code>
### Если при переходе на наш домен ничего не происходит, перезапускаем nginx
<code>sudo systemctl restart nginx</code>

# Zabbix Agent

## Agent на Ubuntu 22.04
### Начнием с обновления списков пакетов на удаленном хосте
<code>sudo apt update</code>
### Затем загрузиим файл репозитория Zabbix из центра загрузки Zabbix следующим образом
<code>wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu22.04_all.deb</code>
### Затем запустим его с помощью команды dpkg
<code>sudo dpkg -i zabbix-release_6.4-1+ubuntu22.04_all.deb</code>
### После этого обновим индекс пакета, чтобы обеспечить синхронизацию репозитория
<code>sudo apt update</code>
### Устанавливаем Zabbix Agnet
<code>sudo apt install zabbix-agent</code>
### Далее добовляем ip нашего Zabbix сервера как указано чуть ниже 

## Agent на Ubuntu 22.04
### Устанавливаем Zabbix Agent
<code>sudo apt install zabbix-agent</code>
### Добовляем ip нашего Zabbix Server в белый список для их коннекта
<code>sudo nano /etc/zabbix/zabbix_agentd.conf</code>

<code>Sever=example.com</code>
### Перезапускаем агента
<code>sudo systemctl restart zabbix-agent</code>
### В вебинтерфейсе переходим по панели **Data collection->Hosts** и нажимаем в верхнем правом углу **Create host**
### В открывшемся окне даем название хота и создаем группу хоста
### Выбираем items
### Добовляем интерфейс Agent
### Прописываем IP, DNS если требуется и прочее

## Мануалы
* [Zabbix](https://www.zabbix.com/download?zabbix=6.4&os_distribution=ubuntu&os_version=22.04&components=server_frontend_agent&db=pgsql&ws=nginx)
* [PostgreSQL](https://www.postgresql.org/download/linux/ubuntu/)
