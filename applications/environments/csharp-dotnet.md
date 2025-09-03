---
title: С# Microsoft(.NET)¶
url: https://docs.amvera.ru/applications/environments/csharp-dotnet.html
path: applications/environments/csharp-dotnet
prev: C# mono
next: Ruby (Rails)
---

# С# Microsoft(.NET)¶

## Содержание

- С# Microsoft(.NET)
- Секция meta
- Секция build
- Секция run
- Рецепты
  - Минимальный файл amvera.yml

---

Back to top

[ View this page ](<../../_sources/applications/environments/csharp-dotnet.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# С# Microsoft(.NET)

Данная конфигурация подходит, если проект собирается при помощи Maven и запускается на .NET. В этом случае проект может быть написан на таких языках как C#, F# и все поддерживаемые языки для .NET.

Написать yaml файл можно как самостоятельно, используя инструкцию ниже, так и воспользоваться нашим генератором yaml, перейдя по [ссылке](<https://manifest.amvera.ru/>), либо заполнить в разделе «Конфигурация» личного кабинета.

## Секция meta

Секция ``meta`` файла ``amvera.yml`` будет выглядеть следующим образом:
[code] 
    ```
    meta:
      environment: csharp
      toolchain:
        name: dotnet
        version: 8.0
    
    ```
    
[/code]

Из параметров, которые можно здесь менять это ``meta.toolchain.version``. Логически это версия .NET, который нужно использовать для сборки. Технически значение ``version`` подставляется в имя образа Docker, который будет использован.

Для фазы сборки и запуска это ``mcr.microsoft.com/dotnet/sdk:${meta.toolchain.version}``. Допустимые значения можно увидеть на странице [докер хаба](<https://hub.docker.com/>).

> **⚠️ Предупреждение** > > Важно Значение meta.toolchain.version должно быть допустимым как для фазы сборки, так и для фазы запуска. Лучше всего подходит простой номер LTS версии .NET. 

## Секция build

В секции ``build`` могут быть указаны следующие параметры:
* ``image``
* ``args``
* ``artifacts``

Параметр ``image`` позволяет использовать другой образ для сборки, а не тот, который предлагается Amvera. Образ должен удовлетворять следующим требованиям:
* исходный код для сборки ожидается в папке /app (или образу без разницы, где будет находиться исходный код);

## Секция run

В секции ``run`` могут быть использованы следующие параметры:
* ``image``
* ``buildFileName``
* ``persistenceMount``
* ``containerPort``

Параметр ``image`` позволяет использовать другой образ для сборки, а не тот, который предлагается Amvera. Образ должен удовлетворять следующим требованиям:
* результат сборки ожидается в папке /app (или образу без разницы, где будет находиться результат сборки);

Параметр ``buildFileName`` \- это название вашего проекта.

Если вы не знаете, откуда взять параметр buildFileName. Введите у себя на компьютере такую команду в папке с проектом: ``dotnet build``. У вас появится папка ``bin/Debug/net*.0/${buildFileName}``. Вам необходимо взять название файла ``.exe`` без расширения. Пример можно найти в минимальном файле amvera.yml

Параметр ``persistenceMount`` позволяет указать, в какую директорию будет примонтирована папка с [постоянным хранилищем](../storage.md#data). По умолчанию имеет значение ``/data``.

Параметр ``containerPort`` позволяет указать какой порт слушает приложение. По умолчанию имеет значение ``80``.

## Рецепты

### Минимальный файл amvera.yml
[code] 
    ```
    meta:
      environment: csharp
      toolchain:
        name: dotnet
        version: 8.0
    
    run:
      buildFileName: bin/WebApplication
    
    ```
    
[/code]

> **Важно:** Поле buildFileName должно быть в формате bin/имя_файла, либо bin/publish/имя_файла если вы делаете через публикацию.

[ Next Ruby (Rails) ](ruby-bundle.md) [ Previous C# mono ](csharp-mono.md)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [C# mono](csharp-mono.md)

→ [Ruby (Rails)](ruby-bundle.md)
