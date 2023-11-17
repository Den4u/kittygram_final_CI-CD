# :3  Учебный проект Kittygram_final  :3

## Автор проекта: https://github.com/Den4u

## Kittygram_final - позволяет пользователям публиковать посты своих любимых котиков, делиться их фотографиями и достижениями. 

## Выволненные задачи:
Настроен запуск проекта Kittygram в контейнерах и CI/CD с помощью GitHub Actions на удалённом сервере (проверка тестами, обновление образов на Docker Hub, запуск контейнеров из обновленных образов, отчёт в ТГ в виде сообщения от бота).

## Стек технологий:
• Python • 
• Django •
• Django REST Framework •
• PostgreSQL •
• Docker • 
• Nginx • 
• Gunicorn •
• React •


## Запуск проекта на удалённом сервере:

1. Подключение к удалённому серверу:
``` 
ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом имя_пользователя@ip_адрес_сервера 
``` 
2. Создать директорию kittygram
``` 
sudo mkdir kittygram
``` 
3. При необходимости почистить диск(кэш npm/APT/сист.логи):
``` 
npm cache clean --force
``` 
```
sudo apt clean
``` 
```
sudo journalctl --vacuum-time=1d
``` 
4. Устанавливка Docker Compose на сервер (выполните команды поочерёдно):
```
sudo apt update
sudo apt install curl
curl -fSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo apt install docker-compose-plugin
```
5. Скопируйте на сервер в директорию kittygram/ файл docker-compose.production.yml:
```
scp -i path_to_SSH/SSH_name docker-compose.production.yml \
    username@server_ip:/home/username/taski/docker-compose.production.yml
```
6. Cоздать файл .env (в директории /kittygram), содержащий пары ключ-значение всех переменных окружения.
``` 
POSTGRES_USER=***
POSTGRES_PASSWORD=***
POSTGRES_DB=***
DB_HOST=***
DB_PORT=***
SECRET_KEY=***
DEBUG=***
ALLOWED_HOSTS=***
``` 
7. Запуск Docker Compose в режиме демона:
```
sudo docker compose -f docker-compose.production.yml up -d
```
8. Выполните миграции, соберите статические файлы бэкенда и скопируйте их в /backend_static/static/:
```
sudo docker compose -f docker-compose.production.yml exec backend python manage.py migrate
sudo docker compose -f docker-compose.production.yml exec backend python manage.py collectstatic
sudo docker compose -f docker-compose.production.yml exec backend cp -r /app/collected_static/. /backend_static/static/
```
9. Настройте и перезагрузите конфиг Nginx:
```
sudo nano /etc/nginx/sites-enabled/default
```
```
sudo service nginx reload
```
10. Окройте в браузере страницу вашего проекта:
```
https://ваш_домен/
```
11. :3 Наслаждайтесь котиками :3

## Автоматический деплой проекта на сервер.
Для автоматического деплоя проекта на сервер с помощью GitHub actions необходимо добавить SECRETS в данный репозиторий:

• DOCKER_PASSWORD=<пароль от DockerHub>
• DOCKER_USERNAME=<имя пользователя DockerHub>
• HOST=<ip сервера>
• SSH_KEY=<приватный SSH-ключ>
• SSH_PASSPHRASE=<пароль для сервера>
• TELEGRAM_TO=<id Телеграм-аккаунта>
• TELEGRAM_TOKEN=<токен  бота>
• USER=<username для подключения к удаленному серверу>

Триггером является Push проекта в главную ветку. Наслаждайтесь!