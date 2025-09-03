---
title: Joomla¶
url: https://docs.amvera.ru/marketplace/joomla.html
path: marketplace/joomla
prev: RabbitMQ
next: Drupal
---

# Joomla¶

## Содержание

- Joomla
- Заполнить переменные окружения
- Открыть ссылку во вкладке «Домены»
- Пример

---

Back to top

[ View this page ](<../_sources/marketplace/joomla.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Joomla

На главной странице выбрать «Преднастроенные сервисы» и нажать кнопку «Создать преднастроенный сервис».

Выбираем:
* Задайте параметры сервиса: low-code
* Задайте тип сервиса: Joomla

![joompla_create](images/joomla_create.png)

## Заполнить переменные окружения

**JOOMLA_DB_TYPE** \- тип базы данных (pgsql - PostgreSQL, mysqli - MySQL) **JOOMLA_DB_HOST** \- url базы данных **JOOMLA_DB_USER** \- имя пользователя базы данных **JOOMLA_DB_PASSWORD** \- пароль базы данных **JOOMLA_DB_NAME** \- имя базы данных **JOOMLA_SITE_NAME** \- название сайта **JOOMLA_ADMIN_USER** \- имя администратора сайта **JOOMLA_ADMIN_USERNAME** \- логин администратора сайта **JOOMLA_ADMIN_PASSWORD** \- пароль администратора сайта **JOOMLA_ADMIN_EMAIL** \- email администратора сайта

![joomla_envvars](images/joomla_envvars.png)

## Открыть ссылку во вкладке «Домены»

![joomla_domain](images/joomla_domain.png)

## Пример

![joomla_login_page](images/joomla_login_page.png)

Нажать на «Open Administrator»

![joomla_admin](images/joomla_admin.png)

Перейти Menus -> All Menus Item -> New

![joomla_menus](images/joomla_menus.png)

Заполнить поля и нажать «Save»

![joomla_add_content](images/joomla_add_content.png)

Возвращаемся на сайт, видим, добавленный контент ![joomla_result](marketplace/assets/joomla_result.png)

[ Next Drupal ](<drupal.html>) [ Previous RabbitMQ ](<rabbitmq.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [RabbitMQ](rabbitmq.md)

→ [Drupal](drupal.md)
