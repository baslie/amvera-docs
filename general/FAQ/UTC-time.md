---
title: Используется другой часовой пояс (на серверах - UTC)¶
url: https://docs.amvera.ru/general/FAQ/UTC-time.html
path: general/FAQ/UTC-time
prev: Не найден файл (no such file or directory …)
next: Ошибки при работе с git
---

# Используется другой часовой пояс (на серверах - UTC)¶

## Содержание

- Используется другой часовой пояс (на серверах - UTC)
- Возможные причины:
- Решение:

---

Back to top

[ View this page ](<../../_sources/general/FAQ/UTC-time.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Используется другой часовой пояс (на серверах - UTC)

## Возможные причины:

В коде приложения не учтено, что на серверах Amvera время - UTC. Это -3 часа от Московского времени.

> Время на сервере - UTC (-3 часа от Москвы)

## Решение:

Учтите в коде приложения, что на серверах время UTC (-3 часа от МСК) и может не совпадать с временем в вашем регионе. Изменить время на сервере нельзя, нужно учесть данный момент на стороне кода приложения.

[ Next Ошибки при работе с git ](../../applications/git/freq-errors.md) [ Previous Не найден файл (no such file or directory …) ](not-found-file.md)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Не найден файл (no such file or directory …)](not-found-file.md)

→ [Ошибки при работе с git](applications/git/freq-errors.md)
