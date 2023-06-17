# Foodgram - продуктовый помощник

![Github actions](https://github.com/FroSDD/foodgram-project-react/actions/workflows/foodgram_workflow.yml/badge.svg)


Foodgram - продуктовый помощник это приложение, в котором авторизированные пользователи могут публиковать рецепты, подписываться на рецепты других пользователей, добавлять чужие рецепты в список «Избранное». Благодаря данному сервису, перед походом в магазин можно скачать список продуктов, необходимых для приготовления одного или нескольких выбранных блюд.

### Проект доступен по ссылке:
http://158.160.103.137

### Админка для входа:
Логин: admin
Почта: admin@admin.ru
пароль: 432165qQ

### Для запуска проекта необходимо:

Установить на сервере docker и docker-compose. Скопировать на сервер файлы docker-compose.yaml и default.conf:

```
scp docker-compose.yml <логин_на_сервере>@<IP_сервера>:/home/<логин_на_сервере>/docker-compose.yml
scp nginx.conf <логин_на_сервере>@<IP_сервера>:/home/<логин_на_сервере>/nginx.conf

```

Добавить в Secrets на Github следующие данные:

```
DB_ENGINE=django.db.backends.postgresql # указать, что проект работает с postgresql
DB_NAME=postgres # имя базы данных
POSTGRES_USER=postgres # логин для подключения к базе данных
POSTGRES_PASSWORD=postgres # пароль для подключения к БД
DB_HOST=db # название сервиса БД (контейнера) 
DB_PORT=5432 # порт для подключения к БД
DOCKER_PASSWORD= # Пароль от аккаунта на DockerHub
DOCKER_USERNAME= # Username в аккаунте на DockerHub
HOST= # IP удалённого сервера
USER= # Логин на удалённом сервере
SSH_KEY= # SSH-key компьютера, с которого будет происходить подключение к удалённому серверу
PASSPHRASE= #Если для ssh используется фраза-пароль
TELEGRAM_TO= #ID пользователя в Telegram
TELEGRAM_TOKEN= #ID бота в Telegram

```

Выполнить команды:

*   git add .
*   git commit -m "Ваш комментарий"
*   git push

После этого будут запущены процессы workflow:

*   проверка кода на соответствие стандарту PEP8 (с помощью пакета flake8) и запуск pytest
*   сборка и доставка докер-образа для контейнера web на Docker Hub
*   автоматический деплой проекта на боевой сервер
*   отправка уведомления в Telegram о том, что процесс деплоя успешно завершился

После успешного завершения процессов workflow на боевом сервере должны будут выполнены следующие команды:

```
sudo docker-compose exec web python manage.py migrate

```


```
sudo docker-compose exec web python manage.py collectstatic --no-input 
```

Затем необходимо будет создать суперпользователя и загрузить в базу данных информацию об ингредиентах:

```
sudo docker-compose exec web python manage.py createsuperuser

```

```
sudo docker-compose exec web python manage.py load_data_csv --path <путь_к_файлу> --model_name <имя_модели> --app_name <название_приложения>

```

### Как запустить проект локально в контейнерах:

Клонировать репозиторий и перейти в него в командной строке:

``` git@github.com:FroSDD/foodgram-project-react.git ``` 
``` cd foodgram-project-react ``` 

Запустить docker-compose:

```
docker-compose up

```

После окончания сборки контейнеров выполнить миграции:

```
docker-compose exec web python manage.py migrate

```

Создать суперпользователя:

```
docker-compose exec web python manage.py createsuperuser

```

Загрузить статику:

```
docker-compose exec web python manage.py collectstatic --no-input 

```

Проверить работу проекта по ссылке:

```
http://localhost/
```


### Как запустить проект локально:

Клонировать репозиторий и перейти в него в командной строке:

``` git@github.com:FroSDD/foodgram-project-react.git ``` 
``` cd foodgram-project-react ``` 

Создать и активировать виртуальное окружение:

``` python3 -m venv venv ``` 

* Если у вас Linux/macOS:
    ``` source venv/bin/activate ``` 

* Если у вас Windows:
    ``` source venv/Scripts/activate ```
    
``` python3 -m pip install --upgrade pip ``` 

Установить зависимости из файла requirements:

``` pip install -r requirements.txt ``` 

Выполнить миграции:

``` python3 manage.py migrate ``` 

Запустить проект:

``` python3 manage.py runserver ``` 

### Технологии:

Python 3.9, Django 3.2, DRF 3.13, Nginx, Docker, Docker-compose, Postgresql, Github Actions.  