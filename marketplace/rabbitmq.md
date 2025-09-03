---
title: RabbitMQ¶
url: https://docs.amvera.ru/marketplace/rabbitmq.html
path: marketplace/rabbitmq
prev: Keycloak
next: Joomla
---

# RabbitMQ¶

## Содержание

- RabbitMQ
- Создание RabbitMQ
- Подключение к RabbitMQ

---

Back to top

[ View this page ](<../_sources/marketplace/rabbitmq.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# RabbitMQ

Облако Amvera поддерживает создание преднастроенного сервиса RabbitMQ.

## Создание RabbitMQ

Для создания RabbitMQ его следует выбрать в разделе «Преднастроенные сервисы» и задать следующие переменные
* RABBITMQ_DEFAULT_USER: имя пользователя (по умолчанию guest)
* RABBITMQ_DEFAULT_PASS: пароль (по умолчанию guest)
* RABBITMQ_DEFAULT_VHOST: виртуальный host (по умолчанию vhost). Нужен, чтобы подключаться к очереди
* RABBITMQ_SERVER_ADDITIONAL_ERL_ARGS Переменная опциональна и устанавливает уровень логирования и т. д.

## Подключение к RabbitMQ

Для подключения к RabbitMQ из приложения следует использовать следующий шаблон, заменив соответствующие значения на ваши параметры.

Вместо host указывается внутреннее доменное имя проекта c RabbitMQ, которое можно найти во вкладке «Домены».
[code] 
    ```
    amqp://<login>:<password>@<host>:5672/vhost
    
    
    ```
    
[/code]

При возникновении вопросов пишите на почту support@amvera.ru

[ Next Joomla ](joomla.md) [ Previous Keycloak ](Keycloack.md)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Keycloak](Keycloack.md)

→ [Joomla](joomla.md)
