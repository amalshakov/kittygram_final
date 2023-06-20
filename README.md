# Kittygram
Cоциальная сеть для обмена фотографиями любимых питомцев. Это рабочий проект, который состоит из бэкенд-приложения на Django и фронтенд-приложения на React. Проект Kittygram позволяет пользователям поделиться своим котиком и рассказать о его достижениях!
### Стек:
- Django 3.2.3
- DjangoRestFramework 3.12.4
- djoser 2.1.0
- python 3.9
- gunicorn 20.1.0
- nginx
- docker
- PostgresQL
- React
### Как запустить проект:
- Установите Docker на сервер:
1. Обновите репозиторий пакетов для установки в Ubuntu
```
sudo apt update
```
2. Установите утилиту 'curl'
```
sudo apt install curl
```
3. С помощью утилиты curl скачайте скрипт для установки докера с официального сайта
```
curl -fSL https://get.docker.com -o get-docker.sh
```
4. Запустите сохраненный скрипт с правами суперпользователя:
```
sudo sh ./get-docker.sh
```
5. Дополнительно к Docker установите утилиту Docker Compose
```
sudo apt-get install docker-compose-plugin
```
6. Проверьте, что Docker работает
```
sudo systemctl status docker
```
- Установите Nginx на сервер:
1. Установите Nginx
```
sudo apt install nginx -y
```
2. Запустите Nginx
```
sudo systemctl start nginx
```
- Настройте Файрвол:
1. Активируйте разрешение принимать запросы на порты 80 и 443.
```
sudo ufw allow 'Nginx Full'
```
2. Активируйте разрешение для порта 22 (стандартный порт для соединения по SSH).
```
sudo ufw allow OpenSSH 
```
3. Включите файрвол.
```
sudo ufw enable
```
4. Проверьте внесённые изменения.
```
sudo ufw status
```
- Через редактор Nano откройте файл конфигурации веб-сервера (Nginx):
```
sudo nano /etc/nginx/sites-enabled/default
```
- Удалите все настройки из файла, запишите и сохраните новые:
```
 server {
    server_name <доменное_имя>;
    
    location / {
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:9000;
    }
 }
```
- Проверьте файл конфигурации на ошибки:
```
sudo nginx -t
```
- Перезагрузите конфигурацию Nginx:
```
sudo systemctl reload nginx
```
- Получите и настройте SSL-сертификат:
1. Установите пакетный менеджер snap (необходим для дальнейшей уствновки certbot):
```
sudo apt install snapd
```
2. Установите и обновите зависимости для пакетного менеджера snap:
```
sudo snap install core; sudo snap refresh core
```
3. Установите пакет certbot:
```
sudo snap install --classic certbot
```
4. Создайте ссылки на certbot в системной директории, чтобы у пользователя с правами администратора был доступ к этому пакету:
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
5. Запустите процесс оформления сертификата для Nginx:
```
sudo certbot --nginx
```
В процессе оформления сертификата вам нужно будет указать свою электронную почту и ответить на несколько вопросов.
Enter email address (англ. «введите почту»). Почта нужна для предупреждений, что сертификат пора обновить.
Please read the Terms of Service... (англ. «прочитайте условия обслуживания»). Прочитайте правила по ссылке, введите Y и нажмите Enter.
Would you be willing to share your email address with the Electronic Frontier Foundation? (англ. «хотите ли вы поделиться своей почтой с Фондом электронных рубежей»). Отметьте на своё усмотрение Y (да) или N (нет) и нажмите Enter.
Далее система оповестит вас о том, что учётная запись зарегистрирована и попросит указать имена, для которых вы хотели бы активировать HTTPS:
```
Account registered.

Which names would you like to activate HTTPS for?
We recommend selecting either all domains, or all domains in a VirtualHost/server block.

1: <доменное_имя_вашего_проекта>

Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel):
```
Введите 1 или не вводите ничего и нажмите Enter.
После этого certbot отправит ваши данные на сервер Let's Encrypt, и там будет выпущен сертификат, который автоматически сохранится на вашем сервере в системной директории /etc/ssl/. Также будет автоматически изменена конфигурация Nginx: в файл /etc/nginx/sites-enabled/default добавятся новые настройки и будут прописаны пути к сертификату.
- Откройте файл /etc/nginx/sites-enabled/default и убедитесь в этом:
```
sudo nano /etc/nginx/sites-enabled/default
```
В файле должны появиться новые настройки и описание путей к сертификату.
- Перезагрузите конфигурацию Nginx:
```
sudo systemctl reload nginx
```
- Клонируйте РЕПОЗИТОРИЙ и перейдите в него в командной строке.
```
git clone git@github.com:amalshakov/kittygram_final
cd kittygram_final
```
- Для хранения секретов создайте настоящий файл .env ориентируясь на образец .env.example. Заполните переменные окружения.
```
POSTGRES_DB=kittygram
POSTGRES_USER=kittygram_user
POSTGRES_PASSWORD=kittygram_password
DB_HOST=
DB_PORT=
SECRET_KEY=
DEBUG=
ALLOWED_HOSTS=
```
- На удаленном сервере создайте папку kittygram и перейдите в нее:
```
mkdir kittygram
cd kittygram
```
- Скопируйте файл docker-compose.production.yml и файл .env из локального репозитория на удаленный сервер при помощи данных команд (либо создайте новые файлы на удаленном сервере и скопируйте в них содежимое файлов)
```
scp -i path_to_SSH/SSH_name docker-compose.production.yml username@server_ip:/home/username/kittygram/docker-compose.production.yml
scp -i path_to_SSH/SSH_name .env username@server_ip:/home/username/kittygram/.env
```
- Хранение секретов в GitHub Actions:
1. Перейдите в настройки репозитория — Settings, выберите на панели слева Secrets and Variables → Actions, нажмите New repository secret.
2. Сохраните переменные с необходимыми значениями: введите имя секрета и его значение, затем нажмите Add secret. Сделайте это для следующих переменных:
```
DOCKER_USERNAME (логин для Docker Hub)
DOCKER_PASSWORD (пароль для Docker Hub)
USER (имя пользователя на сервере)
HOST (IP-адрес вашего сервера)
SSH_KEY (содержимое текстового файла с закрытым SSH-ключом для доступа к серверу)
SSH_PASSPHRASE (значение passphrase для доступа к серверу)
TELEGRAM_TO (ID вашего телеграм-аккаунта)
TELEGRAM_TOKEN (токен вашего бота. Получить этот токен можно у телеграм-бота @BotFather)
```
- Сделайте Пуш:
```
git add .
git commit -m '<message for our commit>'
git push
```
- В результате Пуша произойдет следующее:
1. Тестирование проекта.
2. Сборка и публикация образа.
3. Автоматический деплой.
4. Отправка уведомления в персональный чат.
### Автор
- [Мальшаков Александр](https://github.com/amalshakov)
