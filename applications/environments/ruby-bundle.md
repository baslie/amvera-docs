---
title: Ruby (Rails)¶
url: https://docs.amvera.ru/applications/environments/ruby-bundle.html
path: applications/environments/ruby-bundle
prev: С# Microsoft(.NET)
next: ffmpeg
---

# Ruby (Rails)¶

## Содержание

- Ruby (Rails)
- Секция meta
- Секция build
- Секция run
- Важно
- Рецепты
  - Минимальный файл amvera.yml

---

Back to top

[ View this page ](<../../_sources/applications/environments/ruby-bundle.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Ruby (Rails)

Данная конфигурация подходит, если проект собирается при помощи Ruby.

Написать yaml файл можно как самостоятельно, используя инструкцию ниже, так и воспользоваться нашим генератором yaml, перейдя по [ссылке](<https://manifest.amvera.ru/>), либо заполнить в разделе «Конфигурация» личного кабинета.

## Секция meta

Секция ``meta`` файла ``amvera.yml`` будет выглядеть следующим образом:
[code] 
    ```
    meta:
      environment: ruby
      toolchain:
        name: bundle
        version: 3.0
    
    ```
    
[/code]

Из параметров, которые можно здесь менять это ``meta.toolchain.version``. Логически это версия Ruby, которую нужно использовать для сборки. Технически значение ``version`` подставляется в имя образа Docker, который будет использован.

Для фазы сборки и запуска это ``ruby:${meta.toolchain.version}``. Допустимые значения можно увидеть на странице [докер хаба](<https://hub.docker.com/>).

> Важно
> > Значение ``meta.toolchain.version`` должно быть допустимым как для фазы сборки, так и для фазы запуска. Лучше всего подходит простой номер LTS версии Ruby.

## Секция build

В секции ``build`` могут быть указаны следующие параметры:
* ``image``

Параметр ``image`` позволяет использовать другой образ для сборки, а не тот, который предлагается Amvera. Образ должен удовлетворять следующим требованиям:
* исходный код для сборки ожидается в папке /app (или образу без разницы, где будет находиться исходный код);

## Секция run

В секции ``run`` могут быть использованы следующие параметры:
* ``image``
* ``mainScript``
* ``persistenceMount``
* ``containerPort``

Параметр ``image`` позволяет использовать другой образ для сборки, а не тот, который предлагается Amvera. Образ должен удовлетворять следующим требованиям:
* результат сборки ожидается в папке /app (или образу без разницы, где будет находиться результат сборки);

Параметр ``mainScript`` позволяет указать главный скрипт в проекте, с расширением.

Параметр ``persistenceMount`` позволяет указать, в какую директорию будет примонтирована папка с [постоянным хранилищем](<../storage.html#data>). По умолчанию имеет значение ``/data``.

Параметр ``containerPort`` позволяет указать какой порт слушает приложение. По умолчанию имеет значение ``80``.

## Важно

Если вы используете обычный Ruby, без каких либо фреймворков. Необходимо добавить файл в ваш проект ``Gemfile``, в который добавить минимальную конфигурацию. Содержимое ``Gemfile``:
[code] 
    ```
    source "https://rubygems.org"
    
    ```
    
[/code]

## Рецепты

### Минимальный файл amvera.yml
[code] 
    ```
    meta:
      environment: ruby
      toolchain:
        name: bundle
        version: 3.0
    
    run:
      mainScript: WebService.rb
    
    ```
    
[/code]

[ Next ffmpeg ](<ffmpeg-pip.html>) [ Previous С# Microsoft(.NET) ](<csharp-dotnet.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [С# Microsoft(.NET)](https://docs.amvera.ru/csharp-dotnet.html)

→ [ffmpeg](https://docs.amvera.ru/ffmpeg-pip.html)
