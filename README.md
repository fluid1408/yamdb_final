# CI/CD для проекта Yamdb ![YAMDB_FINAL Status](https://github.com/fluid1408/yamdb_final/actions/workflows/yamdb_workflow.yaml/badge.svg)

## Описание

Проект YaMDb собирает отзывы (Review) пользователей на произведения (Title).
Произведения делятся на категории: "Книги", "Фильмы", "Музыка". Список категорий (Category) может быть расширен.
Сами произведения в YaMDb не хранятся, здесь нельзя посмотреть фильм или послушать музыку.
В каждой категории есть произведения: книги, фильмы или музыка.
Новые жанры может создавать только администратор.
Пользователи оставляют к произведениям текстовые отзывы (Review) и выставляют произведению рейтинг,
а также пишут комментарии (Comments) к отзывам.

## Технологический стек

- Python 3.7
- Django 2.2.16
- Django Rest Framework 3.12.4
- Gunicorn 20.0.4
- Nginx 1.21.3-alpine
- Postres 13.0-alpine
- Docker 20.10.23
- Docker Compose 2.15.1
- Docker Hub
- GitHub Actions
- Yandex.Cloud
- Postman (графическая программа для тестирования API)


## Запуск проекта

### Клонируем проект

Клонировать репозиторий и перейти в него в командной строке:

git clone git@github.com:fluid1408/yamdb_final.git

### Cоздаем и активируем виртуальное окружение

Cоздать и активировать виртуальное окружение:

```
python3 -m venv venv
```

```
source venv/bin/activate
```

### Установим зависимости

```
python3 -m pip install --upgrade pip

pip install -r requirements.txt
```

### Выполним вход на удаленный сервер

### Установим Docker на сервер

```
sudo apt install docker.io
```

### Установим docker-compose на сервер

```
curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

### Отредактируем конфигурационный файл infra/nginx/default.conf - в строке server_name укажем публичный IP своего сервера

### Отредактируем файл settings.py - добавим в ALLOWED_HOST публичный IP своего сервера или доменное имя

### Скопируем файл docker-compose.yaml на сервер.

```
scp docker-compose.yml <username>@<host>:/home/<username>/docker-compose.yml
```

### Создадим на сервере новую директорию nginx. Скопируем в эту директорию конфигурационный файл nginx/default.conf

```
mkdir nginx

scp default.conf <username>@<host>:/home/<username>/nginx/
```

### Cоздадим и заполним файл .env по следующему шаблону:

 - DB_ENGINE=django.db.backends.postgresql
 - DB_NAME=postgres
 - POSTGRES_USER=postgres
 - POSTGRES_PASSWORD=postgres
 - DB_HOST=db
 - DB_PORT=5432
 - DJANGO_SECRET_KEY=<секретный ключ проекта django>

### Для работы с workflow добавим в Secrets GitHub следующие переменные окружения:

```
DB_ENGINE=<django.db.backends.postgresql>
DB_NAME=<имя базы данных postgres>
DB_USER=<пользователь бд>
DB_PASSWORD=<пароль>
DB_HOST=<db>
DB_PORT=<5432>

DOCKER_PASSWORD=<пароль от DockerHub>
DOCKER_USERNAME=<имя пользователя на DockerHub>

DJANGO_SECRET_KEY=<секретный ключ проекта django>

USER=<username для подключения к серверу>
HOST=<IP сервера>
PASSPHRASE=<пароль для сервера, если он установлен>
SSH_KEY=<ваш приватный SSH ключ (для получения команда: cat ~/.ssh/id_rsa)>

TELEGRAM_TO=<ID чата, в который придет сообщение>
TELEGRAM_TOKEN=<токен вашего бота>
```

Workflow состоит из четырех шагов:

- tests: Проверка кода на соответствие стандарту PEP8
- build_and_push_to_docker_hub: Сборка и доставка образа на Docker Hub
- deploy: Автоматический деплой на удаленный сервер
- send_message: Отправка отчета в Telegram-чат о результате выполнения задач workflow

### Соберем и запустим контейнеры на сервере

```
docker compose up -d --build
```

### В контейнере web на сервере выполняем миграции

```
docker compose exec web python manage.py migrate
```

### В контейнере web создаем суперпользователя

```
docker compose exec web python manage.py createsuperuser
```

### В контейнере web соберем статику

```
docker compose exec web python manage.py collectstatic --no-input
```

### Проект Yamdb доступен по ссылке

http://158.160.27.206/api/v1/

### Документация API Yamdb доступна по ссылке

http://158.160.27.206/redoc/

Автор: Шлемин Сергей Александрович
