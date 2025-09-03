---
title: Keycloak¶
url: https://docs.amvera.ru/marketplace/Keycloack.html
path: marketplace/Keycloack
prev: n8n
next: RabbitMQ
---

# Keycloak¶

## Содержание

- Keycloak
- Запуск Keycloak
  - 1. Создать проект.
  - 2. На этапе «Конфигурация» настроить переменные окружения (envvars)
  - 3.В разделе «Настройки» приложения активироватьбесплатное доменное имяилидобавить своё
  - 4. Добавить переменную KC_HOSTNAME ваше доменное имя
  - 5. Перезапустить проект
- Подробная инструкция по настройке Keycloack и подключению к проектам в Amvera

---

Back to top

[ View this page ](<../_sources/marketplace/Keycloack.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Keycloak

Для того, чтобы развернуть Keycloack нам дополнительно понадобиться одна из следующих баз данных:
* MySQL
* PostgreSQL (рекомендуемая нами БД)

Инструкции как развернуть [MySQL](<databases/mysql.md>) и [PostgreSQL](<databases/postgreSQL.md>) на платформе Amvera
* * *

## Запуск Keycloak

Для развертывания Keycloak Вам потребуется выполнить следующие шаги:

### 1\. Создать проект.

Выбираем:
* Тип: Преднастроенное приложение из маркетплейса
* Параметры сервиса: Авторизация
* Тип сервиса: Keycloak

![Изображение](images/keycloack-create.jpg)

Стабильная работа возможна на тарифах не ниже «Начальный».

### 2\. На этапе «Конфигурация» настроить переменные окружения (envvars)
* KC_BOOTSTRAP_ADMIN_USERNAME — имя login администратора
* KC_BOOTSTRAP_ADMIN_PASSWORD — временный пароль администратора
* KC_DB — тип базы данных. Возможные значения: \- mariadb \- postgres
* KC_DB_URL_HOST — host базы данных
* KC_DB_URL_PORT — порт базы данных
* KC_DB_URL_DATABASE - имя базы данных
* KC_DB_USERNAME — имя пользователя базы данных
* KC_DB_PASSWORD — пароль базы данных

![keycloack-envvars](images/keycloack-envvars.png)

### 3.В разделе «Настройки» приложения активировать [бесплатное доменное имя](<applications/configuration/network.md>) или [добавить своё](<applications/configuration/network.md>)

![keycloack-domain](images/keycloack-domain.png)

### 4\. Добавить переменную KC_HOSTNAME ваше доменное имя

![keycloack-hostname](images/keycloack-hostname.png)

### 5\. Перезапустить проект

![keyckloack-restart-project](images/keyckloack-restart-project.jpg)

## Подробная инструкция по настройке Keycloack и подключению к проектам в Amvera

Подробную инструкцию по настройке авторизации и подключению к проектам, вы можете найти в нашей статье на [Хабр.](<https://habr.com/ru/companies/amvera/articles/907990/>)

[ Next RabbitMQ ](<rabbitmq.html>) [ Previous n8n ](<n8n.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [n8n](https://docs.amvera.ru/n8n.html)

→ [RabbitMQ](https://docs.amvera.ru/rabbitmq.html)
