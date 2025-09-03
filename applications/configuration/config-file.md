---
title: Файл конфигурации¶
url: https://docs.amvera.ru/applications/configuration/config-file.html
path: applications/configuration/config-file
prev: Конфигурация
next: Docker
---

# Файл конфигурации¶

## Содержание

- Файл конфигурации
- Структура конфигурационного файла
  - meta
  - build
  - run
- Пример

---

Back to top

[ View this page ](<../../_sources/applications/configuration/config-file.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Файл конфигурации

Для настройки [сборки](<../build.html>) и [запуска](<../run.html>) проекта можно создать файл ``amvera.yml`` или ``amvera.yaml`` в корне репозитория. Альтернативой является использование [_Dockerfile_](<docker.html>).

Создать ``amvera.yaml`` лучше всего в интерфейсе приложения в разделе «Конфигурация» так он автоматически добавится в корень проекта создав при этом новый коммит в гит репозиторий. Возможно так-же воспользоваться нашим генератором yaml, перейдя по [ссылке](<https://manifest.amvera.ru/>), либо самостоятельно, используя инструкции ниже.

Если файл конфигурации отсутствует, то Amvera будет искать файл [_Dockerfile_](<docker.html>).

Если ``Dockerfile`` будет найден, он будет использоваться для сборки. Если ``Dockerfile`` также не будет найден, сборка завершится провалом.

В дальнейшем на файл ``amvera.yml`` или ``amvera.yaml`` для определенности будем ссылаться как ``amvera.yaml``.

## Структура конфигурационного файла

Файл ``amvera.yaml`` состоит из трех секций.

### meta

Секция ``meta`` определяет общую информацию о сборке, такую как окружение и инструменты сборки. Со списком поддерживаемых окружений и подробным их описанием можно ознакомиться на [этой странице](<../supported-env.html>).

Пример секции ``meta`` для JVM приложения, собираемого при помощи Maven:
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: maven
        version: 17
    
    ```
    
[/code]

Подробнее о сборке JVM приложений с помощью Maven в сервисе Amvera можно прочитать [здесь](<../environments/jvm-maven.html>).

### build

Секция ``build`` определяет параметры, необходимые для [сборки приложения](<../build.html>). Для разных окружений эти параметры разные. Про параметры для вашего окружения можно прочитать в соответствующем разделе данной документации.

Если указывать параметры сборки не нужно, то секцию ``build`` можно опустить.

В нашем примере сборки JVM приложения при помощи Maven все параметры build являются необязательными. Однако если нужно указать дополнительные параметры компиляции, сделать это можно следующим образом:
[code] 
    ```
    build:
      args: -Dserver.port=80 -Pproduction
    
    ```
    
[/code]

### run

Секция ``run`` определяет параметры, необходимые для [запуска приложения](<../run.html>). Аналогично сборке для разных окружения эти параметры разные и описаны в соответствующем разделе данной документации.

Так, параметр ``persistenceMount`` задает то, в какую папку будет примонтировано [постоянное хранилище](<../storage.html#data>). Иными словами, если там указано ``/data``, то именно по этому пути следует сохранять важные файлы во время работы приложения.

Если указывать параметры запуска не нужно, то секцию ``run`` можно опустить.

В нашем примере сборки JVM приложения при помощи Maven, в секции run необходимо как минимум указать путь до jar-файла относительно корня проекта:
[code] 
    ```
    run:
      jarName: bag-end.jar
    
    ```
    
[/code]

## Пример

Так, весь файл ``amvera.yaml`` для нашего примера выглядит следующим образом:
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: maven
        version: 17
    
    build:
      args: -Dserver.port=80 -Pproduction
    
    run:
      jarName: bag-end.jar
    
    ```
    
[/code]

Так как формат файла конфигурации YAML, секции можно указывать в любом порядке.

[ Next Docker ](<docker.html>) [ Previous Конфигурация ](<../configuration.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Конфигурация](https://docs.amvera.ru/configuration.html)

→ [Docker](https://docs.amvera.ru/docker.html)
