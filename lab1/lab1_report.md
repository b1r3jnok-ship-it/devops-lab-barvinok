University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Введение в веб технологии](https://itmo-ict-faculty.github.io/introduction-in-web-tech/)
Year: 2025/2026
Group: U4125
Author: Barvinok Vsevolod Vladimirovich
Lab: Lab1
Date of create: 02.03.2026
Date of finished: 03.03.2026

Ход работы:

Установил Docker Desktop (для Windows) с проблемами, но все решил (фото 1)
Проверил установку командой docker --version (фото 2)
Запустил тестовый контейнер: docker run hello-world (фото 4)
Изучить базовые команды: docker images, docker ps, docker ps -a (фото 3)
Работа с готовыми образами:

Скачать образ Ubuntu: docker pull ubuntu:latest (фото 5)
Запустить интерактивный контейнер: docker run -it ubuntu bash (фото 6-7)
Внутри контейнера установить пакет (например, curl): apt update && apt install -y curl
Проверить установку: curl --version (фото 8)
Выйти из контейнера: exit
Запуск веб-сервера:

Запустить контейнер с nginx: docker run -d -p 8080:80 --name web-server nginx:alpine (фото 9)
Проверить работу в браузере: http://localhost:8080 (фото 10)
Посмотреть логи контейнера: docker logs web-server (фото 11)
Подключиться к контейнеру: docker exec -it web-server sh (фото 12, однако я не знал о команде ls, подсмотрел у студентов, связанные с IT)
Управление контейнерами:

Посмотреть запущенные контейнеры: docker ps (фото 13)
Посмотреть все контейнеры: docker ps -a
Остановить контейнер: docker stop web-server (фото 14)
Запустить остановленный контейнер: docker start web-server
Удалить контейнер: docker rm web-server
Удалить образ: docker rmi nginx:alpine
Работа с томами (volumes):

Создать том: docker volume create my-volume (фото 15)
Запустить контейнер с томом: docker run -it --name volume-test -d -v my-volume:/data ubuntu bash
Подключиться к контейнеру: docker exec -it volume-test bash
Создать файл в томе: echo "Hello from volume" > /data/test.txt
Удалить контейнер и создать новый с тем же томом
Проверить, что файл сохранился
