---
title: n8n¶
url: https://docs.amvera.ru/marketplace/n8n.html
path: marketplace/n8n
prev: Qdrant
next: Keycloak
---

# n8n¶

## Содержание

- n8n
- Преднастроенный сервис n8n
- Запуск в виде приложения (альтернативный вариант)
- Https для n8n
- Восстановление пароля и использование почты
- Подключение таблиц Google
- Порты для webhook
- Обновление n8n
- Подключение к LLM Inference API от Amvera через n8n

---

Back to top

[ View this page ](<../_sources/marketplace/n8n.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# n8n

Подробная инструкция по развертыванию и настройке n8n есть в нашей [статье на Хабр](<https://habr.com/ru/companies/amvera/articles/890730/>).

Вы можете развернуть n8n как приложение через Dockerfile по инструкции выше, либо как преднастроенный сервис из маркетплейса.

## Преднастроенный сервис n8n

Для запуска преднастроенного сервиса создайте n8n из плитки на главной странице, либо из меню Преднастроенное приложение из маркетплейса - Машинное обучение и искусственный интеллект - n8n.

Сервис уже будет включать все базовые переменные и правильно заданный https-домен.

## Запуск в виде приложения (альтернативный вариант)

Подробная инструкция по развертыванию и настройке n8n есть в нашей [статье на Хабр](<https://habr.com/ru/companies/amvera/articles/890730/>).

На этапе создания переменных, создайте переменные со следующими значениями и названиями:

N8N_HOST: 0.0.0.0

N8N_PROTOCOL: https

GENERIC_TIMEZONE: Europe/Moscow или другая TZ, актуальная для вашего региона

N8N_DEFAULT_LOCALE: по умолчанию en

N8N_USER_FOLDER: /data (обязательно)

N8N_DATA: /data (обязательно)

## Https для n8n

Cоздаем бесплатный домен с типом подключения HTTPS во вкладке «Домены» проекта. Как тип домена выбираем «Бесплатный домен Amvera», если нет собственного.

## Восстановление пароля и использование почты

Восстановление пароля в n8n требует отправки email на привязанную почту, что невозможно без подключения smtp сервера.

Для подключения необходимо создать следующие переменные окружения (разобрано на примере gmail, для e.mail и других серверов SMPT_HOST может и будет отличаться):

N8N_SMTP_HOST`: smtp.gmail.com

N8N_SMTP_PORT`: 465

N8N_SMTP_USER`: адрес_админа@gmail.com

N8N_SMTP_PASS`: _пароль_приложения_

N8N_SMTP_SENDER`: адрес_админа@gmail.com

N8N_SMTP_SSL`: true

Здесь _пароль_приложения_ — специальный 16-значный пароль в формате ``abcd efgh ijkl mnop``, который генерируется гуглом (https://myaccount.google.com/u/1/apppasswords) если включена двухфакторная аутентификации. Если двухфакторная аутентификация не включена, необходимо вписать пароль от аккаунта.

## Подключение таблиц Google

Вы можете столкнуться с ошибкой «Приложение «amvera.io» не прошло проверку Google».

Такое бывает, когда вы используете инструменты, требующие Google авторизацию (например: гугл таблицы).

В таком случае вам необходимо:
1. Добавить собственный домен
2. Пройти проверку домена в Google (достаточно просто прописать выданные TXT-записи) по [инструкции](<https://support.google.com/a/answer/16018515?hl=ru>)

## Порты для webhook

В преднастроенном сервисе по умолчанию открыты порты 80 и 5678

Во вкладке «Домены» необходимо раскрыть созданный вами домен и добавить следующий маршрут: / 5678

![n8n](images/n8n.png)

Копируем ссылку на приложение, и во вкладке «Переменные» добавляем переменную WEBHOOK_URL, в значение которой вставляем домен. Примерно он будет выглядеть так: https://имя_проекта-ник.amvera.io/ (это бесплатный https домен, который необходимо создать во вкладке Домены).

## Обновление n8n

Для обновления n8n развернутого как преднастроенный сервис, перейдите в раздел «Конфигурация».

Найдите по официальной [ссылке](<https://docs.n8n.io/release-notes/#semantic-versioning-in-n8n>) актуальную версию. Введите версию в формате 1.100.1 и нажмите «Сохранить и перезапустить».

## Подключение к LLM Inference API от Amvera через n8n

Для подключения к инференсу, предоставляемому Amvera, используйте ноду HTTP Request, где укажите:
* Method: POST
* URL: API URL отсюда: https://lllm-swagger-amvera-services.amvera.io/#/LLaMA/post_models_llama
* Authentication: None (нужно указать имя заголовка вручную)
* Send Headers: true
* * Using Fields Below или JSON, как удобнее:
* *       * Name: ``X-Auth-Token``
* *       * Value: ``Bearer <ваш токен>`` (Желательно использовать переменные окружения)
* Send Body: true
* Body Content Type: JSON
* Specify Body: Using JSON
* JSON:

[code] 
    ```
    {
      "model": "llama70b",
      "messages": [
        {
          "role": "user",
          "text": "Привет!"
        }
      ]
    }
    
    ```
    
[/code]

Вместо ``"model": "llama70b"``, можно использовать любую другую модель, доступную в Amvera Inference API

[ Next Keycloak ](<Keycloack.html>) [ Previous Qdrant ](<../databases/Qdrant.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Qdrant](https://docs.amvera.ru/databases/Qdrant.html)

→ [Keycloak](https://docs.amvera.ru/Keycloack.html)
