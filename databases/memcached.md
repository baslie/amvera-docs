---
title: Memcached¶
url: https://docs.amvera.ru/databases/memcached.html
path: databases/memcached
prev: Redis
next: Qdrant
---

# Memcached¶

## Содержание

- Memcached
- Подключение Memcached

---

Back to top

[ View this page ](<../_sources/databases/memcached.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Memcached

Облако Amvera поддерживает создание преднастроенного сервиса Memcached.

Создание Memcached осуществляется в разделе «Преднастроенные сервисы» и не требует ввода переменных.

## Подключение Memcached

Пример подключения из приложения на Python
[code] 
    ```
        ```
    from pymemcache.client import base 
    client = base.Client((<host>, 11211))
        ```
    
    ```
    
[/code]

Вместо host следует указать внутреннее доменное имя проекта Memcached, которое можно найти во вкладке «Домены».

[ Next Qdrant ](<Qdrant.html>) [ Previous Redis ](<redis.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Redis](redis.md)

→ [Qdrant](Qdrant.md)
