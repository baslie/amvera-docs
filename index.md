---
title: Руководство Amvera Cloud¶
url: https://docs.amvera.ru/index.html
path: index
next: Примеры
---

# Руководство Amvera Cloud¶

## Содержание

- Руководство Amvera Cloud

---

Back to top

[ View this page ](<_sources/index.rst.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Руководство Amvera Cloud

Amvera Cloud - сервис для развертывания IT-приложений, СУБД и других программ в облаке.

Развертывание
* [Примеры](general/examples.md)
* [Backend с использованием Flask](general/examples/python-flask.md)
* [Телеграм бот на Python](general/examples/python-tgbot.md)
* [Fullstack на Django](general/examples/python-django.md)
* [Mini Apps со stage и prod, привязкой GitHub, БД и связкой Backend и Frontend, развернутых в разных проектах](general/examples/miniappex.md)
* [Spring Boot с встраиваемой базой данных H2 и PostgreSQL](general/examples/java-springboot.md)
* [Fullstack-приложение на Go c подключением к PostgreSQL](general/examples/go-postgresql.md)
* [Fullstack на Go](general/examples/go_full.md)
* [Ruby on Rails c базой данных SQLite](general/examples/Ruby-SQLite.md)
* [Web-приложение на C# Microsoft (.NET) с подключением к SQLite](general/examples/NET-SQLite.md)
* [Быстрый старт](applications/quick-start.md)
* [Процесс создания приложения](applications/quick-start.md#id2)
* [Частые ошибки](general/faq.md)
* [Ошибки в логах и их причины](general/FAQ/errors-in-logs.md)
* [Не получается обновить проект](general/FAQ/update.md)
* [Не сохраняются данные при перезапуске/пересборке приложения/базы данных](general/FAQ/data-saving.md)
* [502/503 ошибка при статусе «Приложение запущено»](general/FAQ/502-503-error.md)
* [Бесконечный запуск, сборка или отсутствие логов](general/FAQ/infinite-build-run.md)
* [Не работает телеграм-бот](general/FAQ/tgbot.md)
* [Ошибка venv в Python при сборке или запуске приложения](general/FAQ/enverror.md)
* [Не найден файл (no such file or directory …)](general/FAQ/not-found-file.md)
* [Используется другой часовой пояс (на серверах - UTC)](general/FAQ/UTC-time.md)
* [Ошибки при работе с git](applications/git/freq-errors.md)
* [Проблема с оплатой](general/FAQ/payments.md)

Приложения
* [Git](applications/git.md)
* [Использование Amvera как основной репозиторий](applications/git/main-origin.md)
* [Подключение к существующему репозиторию](applications/git/secondary-origin.md)
* [Подключить GitHub, GitLab, Bitbucket](applications/git/webhooks.md)
* [Ошибки при работе с git](applications/git/freq-errors.md)
* [Основные концепции](applications/git.md#id1)
* [Хранилище и пути](applications/storage.md)
* [Code](applications/storage.md#code)
* [Artifacts (DEPRECATED)](applications/storage.md#artifacts-deprecated)
* [Data](applications/storage.md#data)
* [Пример](applications/storage.md#id2)
* [Работа с файлами через интерфейс](applications/storage.md#id3)
* [Сборка](applications/build.md)
* [Процесс сборки с ``amvera.yaml``](applications/build.md#amvera-yaml)
* [Инициализация сборки](applications/build.md#id2)
* [Лог сборки](applications/build.md#id5)
* [Запуск](applications/run.md)
* [Процесс запуска с ``amvera.yaml``](applications/run.md#amvera-yaml)
* [Жизненный цикл](applications/run.md#id2)
* [Лог приложения](applications/run.md#id5)
* [Бэкапы](applications/backups.md)
* [Бэкапы /data](applications/backups.md#data)
* [Контроль версий](applications/version-control.md)
* [Как использовать](applications/version-control.md#id2)
* [Конфигурация](applications/configuration.md)
* [Файл конфигурации](applications/configuration/config-file.md)
* [Docker](applications/configuration/docker.md)
* [Поддерживаемые окружения](applications/supported-env.md)
* [Перенос проекта из Heroku](applications/configuration/heroku-migration.md)
* [Переменные и секреты](applications/configuration/variables.md)
* [Добавление переменной или секрета](applications/configuration/variables.md#id2)
* [Доступ к переменным](applications/configuration/variables.md#id3)
* [Переменная AMVERA](applications/configuration/variables.md#amvera)
* [Отличие секретов от переменных окружения](applications/configuration/variables.md#id4)
* [Сетевое взаимодействие](applications/configuration/network.md)
* [Доступ из других приложений](applications/configuration/network.md#id2)
* [Доступ из сети Интернет](applications/configuration/network.md#id3)
* [Пример настройки записей в ЛК reg.ru](applications/configuration/network.md#reg-ru)
* [Профилактика ошибок](applications/configuration/network.md#id6)

Базы Данных
* [PostgreSQL](databases/postgreSQL.md)
* [SQLite](databases/sqlite.md)
* [MongoDB](databases/mongodb.md)
* [MySQL](databases/mysql.md)
* [Redis](databases/redis.md)
* [Memcached](databases/memcached.md)
* [Qdrant](databases/Qdrant.md)

Преднастроенные сервисы
* [n8n](marketplace/n8n.md)
* [Keycloak](marketplace/Keycloack.md)
* [RabbitMQ](marketplace/rabbitmq.md)
* [Joomla](marketplace/joomla.md)
* [Drupal](marketplace/drupal.md)
* [WordPress](marketplace/wordpress.md)

LLM
* [Amvera LLM Inference](LLM/doc-inference-ru.md)

Общее
* [Пополнение счета](general/topup.md)
* [Тарифные планы](general/price.md)
* [Смена тарифного плана](general/tarifs.md)
* [Масштабирование ресурса](general/scaling.md)
* [Уведомления о сбоях](general/notifications.md)
* [Поддержка проб Kubernetes](general/k8sprobe.md)
* [CLI. Управление через командную строку](general/cli.md)
* [SLA](general/sla.md)
* [Удаление ресурса](general/disposion.md)

[ Next Примеры ](general/examples.md)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

→ [Примеры](general/examples.md)
