University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Введение в веб технологии](https://itmo-ict-faculty.github.io/introduction-in-web-tech/)
Year: 2025/2026
Group: U4125
Author: Barvinok Vsevolod Vladimirovich
Lab: Lab2
Date of create: 03.03.2026
Date of finished: 10.03.2026

Ход работы:
Настройка CI/CD пайплайна с GitHub Actions:

Подготовка проекта:

Скопировать файлы из первой лабораторной (app.py, requirements.txt, Dockerfile) в новый репозиторий
Создать аккаунт на Docker Hub — не было
Создать новый репозиторий на Docker Hub для вашего образа (фото 1 и 2)
Настройка GitHub Actions:

Создать папку .github/workflows/ в корне проекта : https://github.com/b1r3jnok-ship-it/devops-lab-barvinok/blob/main/.github/workflows/docker-build.yml
Создать файл docker-build.yml с пайплайном, который должен:
Запускаться при пуше в main ветку
Использовать Ubuntu как runner
Выполнять checkout кода
Настраивать Docker Buildx
Логиниться в Docker Hub используя секреты
Собирать и пушить образ с тегом username/my-flask-app:latest
Добавлять шаг деплоя (можно просто echo сообщение)
Настройка секретов: (фото 3)

В настройках GitHub репозитория добавить секреты:
DOCKER_USERNAME - ваш логин на Docker Hub
DOCKER_PASSWORD - ваш пароль или токен доступа Docker Hub
Тестирование пайплайна: (фото 4) 

Сделать коммит и пуш в main ветку (фото 5)
Проверить выполнение пайплайна в разделе Actions
Убедиться, что образ появился в Docker Hub (фото 6)
Проверить логи выполнения каждого шага (фото 7-10)
