---
title: C# mono¶
url: https://docs.amvera.ru/applications/environments/csharp-mono.html
path: applications/environments/csharp-mono
prev: Go
next: С# Microsoft(.NET)
---

# C# mono¶

## Содержание

- C# mono
- Секция meta
- Секция build
- Секция run
- Рецепты
  - Минимальный файл amvera.yml

---

Back to top

[ View this page ](<../../_sources/applications/environments/csharp-mono.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# C# mono

Данная конфигурация подходит, если проект собирается и запускается при помощи Mono. В этом случае проект может быть написан на таких языках как C#.

Написать yaml файл можно как самостоятельно, используя инструкцию ниже, так и воспользоваться нашим генератором yaml, перейдя по [ссылке](<https://manifest.amvera.ru/>), либо заполнить в разделе «Конфигурация» личного кабинета.

## Секция meta

Секция ``meta`` файла ``amvera.yml`` будет выглядеть следующим образом:
[code] 
    ```
    meta:
      environment: csharp
      toolchain:
        name: mono
        version: latest
    
    ```
    
[/code]

Из параметров, которые можно здесь менять это ``meta.toolchain.version``. Логически это версия образа Mono, который нужно использовать для сборки. Технически значение ``version`` подставляется в имя образа Docker, который будет использован.

Для фазы сборки и запуска это ``mono:${meta.toolchain.version}``. Допустимые значения можно увидеть на странице [докер хаба](<https://hub.docker.com/>).

> **⚠️ Предупреждение** > > Важно Значение meta.toolchain.version должно быть допустимым как для фазы сборки, так и для фазы запуска. Лучше всего подходит простой номер LTS версии Mono. 

## Секция build

В секции ``build`` могут быть указаны следующие параметры:
* ``image``
* ``mainFile``

Параметр ``image`` позволяет использовать другой образ для сборки, а не тот, который предлагается Amvera. Образ должен удовлетворять следующим требованиям:
* исходный код для сборки ожидается в папке /app (или образу без разницы, где будет находиться исходный код);

Параметр ``mainFile`` является ОБЯЗАТЕЛЬНЫМ. В него необходимо передать название файла с методом Main c расширением. Пример:
[code] 
    ```
    build:
      mainFile: Program.cs 
    
    ```
    
[/code]

## Секция run

В секции ``run`` могут быть использованы следующие параметры:
* ``image``
* ``persistenceMount``
* ``containerPort``

Параметр ``image`` позволяет использовать другой образ для сборки, а не тот, который предлагается Amvera. Образ должен удовлетворять следующим требованиям:
* результат сборки ожидается в папке /app (или образу без разницы, где будет находиться результат сборки);

Параметр ``persistenceMount`` позволяет указать, в какую директорию будет примонтирована папка с [постоянным хранилищем](<../storage.html#data>). По умолчанию имеет значение ``/data``.

Параметр ``containerPort`` позволяет указать какой порт слушает приложение. По умолчанию имеет значение ``80``.

## Рецепты

### Минимальный файл amvera.yml
[code] 
    ```
    meta:
      environment: csharp
      toolchain:
        name: mono
        version: latest
    
    build:
      mainFile: Program.cs
    
    ```
    
[/code]

[ Next С# Microsoft(.NET) ](<csharp-dotnet.html>) [ Previous Go ](<golang-go.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Go](https://docs.amvera.ru/golang-go.html)

→ [С# Microsoft(.NET)](https://docs.amvera.ru/csharp-dotnet.html)
