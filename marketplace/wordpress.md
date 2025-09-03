---
title: WordPress¶
url: https://docs.amvera.ru/marketplace/wordpress.html
path: marketplace/wordpress
prev: Drupal
next: Amvera LLM Inference
---

# WordPress¶

## Содержание

- WordPress
- Пример

---

Back to top

[ View this page ](<../_sources/marketplace/wordpress.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# WordPress

Предварительно нужно создать базу данных MySQL

На главной странице выбрать «Преднастроенные сервисы» и нажать кнопку «Создать преднастроенный сервис».

Выбираем:
* Задайте параметры сервиса: low-code
* Задайте тип сервиса: WordPress

![wordpress_create](images/wordpress_create.png)

Заполнить переменные окружения

WORDPRESS_DB_HOST - host базы данных WORDPRESS_DB_NAME - имя базы данных WORDPRESS_DB_USER - имя пользователя базы данных WORDPRESS_DB_PASSWORD - пароль базы данных

![wordpress_envvars](images/wordpress_envvars.png)

Открыть ссылку во вкладке «Домены»

![wordpress_domain](images/wordpress_domain.png)

Выбрать язык

![wordpress_language](images/wordpress_language.png)

Ввести
* название сайта
* логинн администратора
* пароль администратора
* email администратора

![wordpress_site_name](images/wordpress_site_name.png)

## Пример

Войти под учётными данными администратора

![wordpress_login](images/wordpress_login.png)

![wordpress_admin_panel](images/wordpress_admin_panel.png)

![wordpress_add_content1](images/wordpress_add_content1.png)

Нажать «Save»

![wordpress_add_content2](images/wordpress_add_content2.png)

[ Next Amvera LLM Inference ](../LLM/doc-inference-ru.md) [ Previous Drupal ](drupal.md)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Drupal](drupal.md)

→ [Amvera LLM Inference](LLM/doc-inference-ru.md)
