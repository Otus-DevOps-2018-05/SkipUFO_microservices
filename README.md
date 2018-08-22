# SkipUFO_microservices
SkipUFO Microservices repository

# Table of content
- [Homework-12: Docker-1](#homework-12-docker-1)
- [Homework-13: Docker-2](#homework-13-docker-2)

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
