[![Django-app workflow](https://github.com/fluid1408/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)](https://github.com/fluid1408/yamdb_final/actions/workflows/yamdb_workflow.yml)
Этот проект направлен на изучение Dockera, для того чтоб было проще работать на удаленных серверах

Стек: django-filter, djangorestframework==3.12.4djangorestframework-simplejwt, PyJWT, pytest-django, pytest-pythonpath, asgiref, Django, gunicorn, sqlparse



- EMAIL_HOST_PASSWORD= пароль от почты с которой будет отправляться код для подтверждения 
- EMAIL_HOST_USER= логин почты
- DB_ENGINE= испольщзуемая база данных
- DB_NAME= имя базы
- POSTGRES_USER= имя для захода в базу
- POSTGRES_PASSWORD= пароль от базы
- DB_HOST=db
- DB_PORT= порт
# Описание команд для запуска приложения в контейнерах
- 
docker-compose up -d --build
 - для того чтоб забилдить и контейнеры (без логов -d)
- 
docker-compose exec web python manage.py migrate
 (миграции)
- 
docker-compose exec web python manage.py createsuperuser
 (создание суперюзера)
- 
docker-compose exec web python manage.py collectstatic --no-input
 (сбор статических файлов)

Ссылка на REDOC: http://158.160.53.121:5000

Автор: Шлемин Сергей Александрович