---
title: Go¶
url: https://docs.amvera.ru/applications/environments/golang-go.html
path: applications/environments/golang-go
prev: Node.JS Server
next: C# mono
---

# Go¶

## Содержание

- Go
- Секция meta
- Секция build
- Секция run
- Рецепты
  - Минимальный файл amvera.yml

---

Back to top

[ View this page ](<../../_sources/applications/environments/golang-go.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Go

Данная конфигурация подходит, если проект сделан на Go.

Написать yaml файл можно как самостоятельно, используя инструкцию ниже, так и воспользоваться нашим генератором yaml, перейдя по [ссылке](<https://manifest.amvera.ru/>), либо заполнить в разделе «Конфигурация» личного кабинета.

## Секция meta

Секция ``meta`` файла ``amvera.yml`` будет выглядеть следующим образом:
[code] 
    ```
    meta:
      environment: golang
      toolchain:
        name: go
        version: 1.22
    
    ```
    
[/code]

Из параметров, которые можно здесь менять это ``meta.toolchain.version``. Логически это версия компилятора Go, который нужно использовать для сборки. Технически значение ``version`` подставляется в имя образа Docker, который будет использован. Вы можете использовать более высокую версию компилятора, к примеру: дома на компьютере вы используете 1.21, если вы попробуйте собрать ваш проект в амвере с образом 1.22 - у вас все сработает.

Для фазы сборки и запуска - это ``golang:${meta.toolchain.version}``. Допустимые значения можно увидеть на странице [докер хаба](<https://hub.docker.com/>).

> **⚠️ Предупреждение** > > Важно Значение meta.toolchain.version должно быть допустимым как для фазы сборки, так и для фазы запуска. Лучше всего подходит простой номер LTS версии. 

## Секция build

В секции ``build`` могут быть указаны следующие параметры:
* ``image``

Параметр ``image`` позволяет использовать другой образ для сборки, а не тот, который предлагается Amvera. Образ должен удовлетворять следующим требованиям:
* исходный код для сборки ожидается в папке /app (или образу без разницы, где будет находиться исходный код);

## Секция run

В секции ``run`` могут быть использованы следующие параметры:
* ``image``
* ``persistenceMount``
* ``containerPort``

Параметр ``image`` позволяет использовать другой образ для сборки, а не тот, который предлагается Amvera. Образ должен удовлетворять следующим требованиям:
* результат сборки ожидается в папке /app (или образу без разницы, где будет находиться результат сборки);

Параметр ``persistenceMount`` позволяет указать, в какую директорию будет примонтирована папка с [постоянным хранилищем](../storage.md#data). По умолчанию имеет значение ``/data``.

Параметр ``containerPort`` позволяет указать какой порт слушает приложение. По умолчанию имеет значение ``80``.

## Рецепты

### Минимальный файл amvera.yml
[code] 
    ```
    meta:
      environment: golang
      toolchain:
        name: go
        version: 1.22
    
    ```
    
[/code]

[ Next C# mono ](csharp-mono.md) [ Previous Node.JS Server ](nodejs-server.md)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Node.JS Server](nodejs-server.md)

→ [C# mono](csharp-mono.md)
