# Это <span style="color:darkred">Ya</span>mdb. Данный портал собирает отзывы пользователей на различные книги, фильмы и музыку.

## Технологии:
![DjangoREST](https://img.shields.io/badge/DJANGO-REST-ff1709?style=for-the-badge&logo=django&logoColor=white&color=ff1709&labelColor=gray)
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)
![Postman](https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)


![YAmDB](https://github.com/potapovjakov/yamdb_final/actions/workflows/yamdb_workflow.yml/badge.svg)
- Посмотреть запущенный проект можно по ссылке - [Yamdb](http://potapovjakov.ru/yamdb)

## Как запустить проект на своей машине:
Клонируйте репозиторий и перейдите в него в командной строке:

```
git clone git@github.com:potapovjakov/infra_sp2.git
```

```
cd infra_sp2
```

Создайте .env файл в каталоге infra/ и опишите следующие переменные:
- SECRET_KEY=токен
- DB_ENGINE=django.db.backends.postgresql
- DB_NAME=название БД
- POSTGRES_USER= имя пользователя БД
- POSTGRES_PASSWORD= пароль для доступа к БД
- DB_HOST=db
- DB_PORT=5432

Из папки infra/ соберите образ при помощи docker-compose
```
cd infra
```

```
docker-compose up -d --build
```
Примените миграции

```
docker-compose exec web python manage.py migrate
```

Соберите статику
```
docker-compose exec web python manage.py collectstatic --no-input
```

Создайте суперюзера
```
docker-compose exec web python manage.py createsuperuser
```
Теперь портал будет доступен по адресу http://127.0.0.1 на 80 порту
Админка находится по адресу http://127.0.0.1/admin


### Как работать с порталом:
Аутентификация происходит следующим образом:

> Отправляем POST запрос на эндпоинт ***/api/v1/auth/signup/*** в виде JSON свои логин и email.
Пример:
```
    {
        "username": "username",
        "email": "email@yamdb.com"
    }
```
- На указанную почту придет ответ с кодом подтверждения
- После получения кода необходимо отправить POST запрос в виде JSON на эндпоинт ***/api/v1/auth/token/*** с указанием логина и кода подтверждения. Пример:
```
    {
        "username": "username",
        "confirmation_code": "6g3 2208352adff8cbc627b0"
    }
```
- Токен вернётся в поле access.
- Если ваш токен утрачен, украден или каким-то иным образом скомпрометирован, вам необходимо повторить процедуру получения нового токена.
- Этот токен также надо будет передавать в заголовке каждого запроса, в поле Authorization. Перед самим токеном должно стоять ключевое слово Bearer и пробел.

### Примеры запросов:
```
Получить список произведений: GET запрос на эндпоинт 
/api/v1/titles/
```
```
Посмотреть отзывы к произведению: GET запрос на эндпоинт 
/api/v1/titles/{title_id}/reviews/
```
```
Посмотреть комментарии к отзыву: GET запрос на эндпоинт 
/api/v1/titles/{title_id}/reviews/{review_id}/comments/
```
```
Оставить отзыв к произведению: POST запрос с обязательными полями 
"text"(текст отзыва) и "score"(оценка произведения от 1 до 10)(не забываем про токен) на эндпоинт 
/api/v1/titles/{title_id}/reviews/
```
####Полный функционал API и примеры запросов можно посмотреть в браузере в документации по адресу http://127.0.0.1/redoc/

---
***Выполнил [Потапов Яков](https://github.com/potapovjakov) при поддержке [Яндекс Практикума](https://practicum.yandex.ru/)***
