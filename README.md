# SkipUFO_microservices
SkipUFO Microservices repository

# Table of content
- [Homework-12: Docker-1](#homework-12-docker-1)
- [Homework-13: Docker-2](#homework-13-docker-2)
- [Homework-14: Docker-3](#homework-14-docker-3)

# Homework-12: Docker-1
## 1.1 Что было сделано
- Установлен docker (sudo apt install docker.io)
- Добавлены права для теущего пользователя (sudo usermod -a -G docker $USER)
- Запущен образ ubuntu-16.04 в него добавлен файл /tmp/file
- Собранный образ добавлен в локальный репозиторий образов

## 1.2 В задании со *
- Сравнены выводы команды inspect на образе и контейнере

# Homework-13: Docker-2
## 1.1 Что было сделано
- Настроена работа для работы с докером в облаке при помощи docker-machine
```
docker-machine create --driver google --google-machine-image https://www.googleapis.com/compute/v1/projects/ubuntu-os-cloud/global/images/family/ubuntu-1604-lts --google-machine-type n1-standard-1 --google-zone europe-west1-b docker-host
```
- Запущен образ tehbilly/htop сравнен вывод запуска контейнера с параметром --pid host и без него (в случае указания --pid host htop отображает процессы хоста, а не контейнера, как без --pid host)

# Homework-14: Docker-3
## 1.1 Что было сделано
- Развернуты исходники из предоставленного архива. Добавлены файлы сборки образов Docker. Ответ на вопрос, почему ui собирается не с первого шага - потому что Dockerfile образа comment содержит промежуточный слой с 
```
FROM ruby:2.2
RUN apt-get update -qq && apt-get install -y build-essential

ENV APP_HOME /app
RUN mkdir $APP_HOME
WORKDIR $APP_HOME

ADD Gemfile* $APP_HOME/
RUN bundle install
ADD . $APP_HOME
```
- Изучено использование volume (например, для хранения файлов БД). Добавлен volume в gc docker volume create reddit_db, использование - docker run  -v reddit_db:/data/db, где /data/db - абсолютный путь в volume

## 1.2 В задании со *
- Была сформирована команда с установкой переменных окружения
```
docker network create reddit
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=reddit --network-alias=post -e POST_DATABASE_HOST=post_db2 -e POST_DATABASE=post2 skipufo/post:1.0
docker run -d --network=reddit --network-alias=comment -e COMMENT_DATABASE_HOST=comment_db2 -e COMMENT_DATABASE=comments2 skipufo/comment:1.0
docker run -d --network=reddit -p 9292:9292 -e POST_SERVICE_HOST=post2 -e COMMENT_SERVICE_HOST=comments2 skipufo/ui:1.0
```
-- Создан образ ui на основе Alpine Linux. У Alpine свой псевдо пакетный менеджер, "правильные" имена пакетов взяты из репо: http://nl.alpinelinux.org/alpine/v3.7/main/x86_64/
```
FROM alpine:3.7

RUN apk add --update ruby ruby-dev build-base \
    && gem install bundler --no-ri --no-rdoc

ENV APP_HOME /app
RUN mkdir $APP_HOME

WORKDIR $APP_HOME
ADD Gemfile* $APP_HOME/
RUN bundle install
ADD . $APP_HOME

ENV POST_SERVICE_HOST post
ENV POST_SERVICE_PORT 5000
ENV COMMENT_SERVICE_HOST comment
ENV COMMENT_SERVICE_PORT 9292

CMD ["puma"]

```
