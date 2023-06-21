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
- Клонируйте репозиторий и перейдите в него в командной строке.
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
- Запустите проект с помощью Docker compose:
```
docker compose -f docker-compose.production.yml up
```
- Либо запустите проект в 'фоновом режиме':
```
sudo docker compose -f docker-compose.production.yml up -d
```
### Автор
- [Мальшаков Александр](https://github.com/amalshakov)
