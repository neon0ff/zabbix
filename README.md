# Zabbix Server

## Установка PostgreSQL
### Создадим конфигурацию репозитория файлов:
```bash
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
```
### Импортируем ключ подписи репозитория:
```bash
sudo wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
```
### Обновим списки пакетов:
```bash
sudo apt-get update
```
### Установим последнюю версию PostgreSQL.
### Если нам нужна конкретная версия, используем 'postgresql-12' или аналогичный вместо 'postgresql':
```bash
sudo apt-get -y install postgresql
```
### Проверим статус после установки
```bash sudo systemctl status postgresql
```
### Установим Netplan tools и проверим открытые порты
```bash
sudo apt install net-tools
```

```bash
sudo netstat -tulpn
```
### Должны увидеть открытый порт 5432 для PostgreSQL
## Установка Zabbix
### Устанавливаем репозиторий Zabbix
```bash
sudo wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu20.04_all.deb
```

```bash
sudo dpkg -i zabbix-release_6.4-1+ubuntu22.04_all.deb
```

```bash
sudo apt update
```
### Установим Zabbix сервер, интерфейс, агент
```bash
sudo apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-nginx-conf zabbix-sql-scripts zabbix-agent
```
### Создаём пользоватля "zabbix" и пароль для него
```bash
sudo -u postgres createuser --pwprompt zabbix
```
### Создаем БД  "zabbix"
```bash
sudo -u postgres createdb -O zabbix zabbix
```
### На хост сервера Zabbix импортируем исходную схему и данные. Нам будет предложено ввести только что созданный пароль. Придется подождать пока все импортирует
```bash 
sudo zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```
### Зададим пароль для БД. Для этого заходим в конфигурационный файл раскоментим ```bashDBPassword=``` и пропишем пароль
```bash 
sudo nano /etc/zabbix/zabbix_server.conf
```
### Настроим PHP для интерфейса Zabbix. Отредактируем файл **nginx.conf**, раскомментируем и установии директивы **listen** и **server_name**

```bash 
sudo nano /etc/zabbix/nginx.conf
```

```bash
listen 8080;
```

```bash
server_name example.com;
```
### Запускаем сервер Zabbix и процессы агента
```bash 
sudo systemctl restart zabbix-server zabbix-agent nginx php8.1-fpm
```

```bash 
sudo systemctl enable zabbix-server zabbix-agent nginx php8.1-fpm
```
### Если при переходе на наш домен ничего не происходит, перезапускаем nginx
```bash 
sudo systemctl restart nginx
```

# Zabbix Agent
## Agent на Ubuntu 24.04
### Скачиваем Zabbix Agent репозиторий
```bash 
sudo wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu24.04_all.deb
```
```bash
sudo dpkg -i zabbix-release_6.4-1+ubuntu24.04_all.deb
```
### Обновляем пакеты
```bash
sudo apt update
```
### Устанавливаем Zabbix Agent
```
sudo apt install zabbix-agent
```
### Добовляем ip нашего Zabbix Server в белый список для их коннекта
```bash 
sudo nano /etc/zabbix/zabbix_agentd.conf
```

```bash
Sever=example.com
```
### Перезапускаем агента
```bash 
sudo systemctl restart zabbix-agent
```
### В вебинтерфейсе переходим по панели **Data collection->Hosts** и нажимаем в верхнем правом углу **Create host**
### В открывшемся окне даем название хота и создаем группу хоста
### Выбираем items
### Добовляем интерфейс Agent
### Прописываем IP, DNS если требуется и прочее

## Мануалы
* [Zabbix](https://www.zabbix.com/download?zabbix=6.4&os_distribution=ubuntu&os_version=22.04&components=server_frontend_agent&db=pgsql&ws=nginx)
* [PostgreSQL](https://www.postgresql.org/download/linux/ubuntu/)
