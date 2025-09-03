---
title: JVM Gradle¶
url: https://docs.amvera.ru/applications/environments/jvm-gradle.html
path: applications/environments/jvm-gradle
prev: Python Pip
next: JVM Maven
---

# JVM Gradle¶

## Содержание

- JVM Gradle
- Секция meta
- Секция build
- Секция run
- Рецепты
  - Минимальный файл amvera.yml
  - Spring Boot

---

Back to top

[ View this page ](<../../_sources/applications/environments/jvm-gradle.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# JVM Gradle

Данная конфигурация подходит, если проект собирается при помощи Gradle и запускается на JVM. В этом случае проект может быть написан на таких языках как Java или Kotlin.

Написать yaml файл можно как самостоятельно, используя инструкцию ниже, так и воспользоваться нашим генератором yaml, перейдя по [ссылке](<https://manifest.amvera.ru/>).

## Секция meta

Секция ``meta`` файла ``amvera.yml`` будет выглядеть следующим образом:
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: gradle
        version: 11
    
    ```
    
[/code]

Из параметров, которые можно здесь менять это ``meta.toolchain.version``. Логически это версия JDK, который нужно использовать для сборки. Технически значение ``version`` подставляется в имя образа Docker, который будет использован.

Для фазы сборки это ``gradle:jdk${meta.toolchain.version}``. Допустимые значения можно увидеть на странице [докер хаба](<https://hub.docker.com/_/gradle/tags?page=1&amp;name=jdk>).

Для фазы запуска это ``bellsoft/liberica-openjre-debian:${meta.toolchain.version}``. Допустимые значения можно увидеть на странице [докер хаба](<https://hub.docker.com/r/bellsoft/liberica-openjre-debian/tags>).

> **⚠️ Предупреждение** > > Важно Значение meta.toolchain.version должно быть допустимым как для фазы сборки, так и для фазы запуска. Лучше всего подходит простой номер LTS версии JDK. 

## Секция build

В секции ``build`` могут быть указаны следующие параметры:
* ``image``
* ``args``
* ``artifacts``

Параметр ``image`` позволяет использовать другой образ для сборки, а не тот, который предлагается Amvera. Образ должен удовлетворять следующим требованиям:
* исходный код для сборки ожидается в папке /app (или образу без разницы, где будет находиться исходный код);
* в образе присутствует команда ``gradle``, которую можно вызвать по имени (без указания абсолютного пути) с параметром ``build``.

Параметр ``args`` позволяет указать дополнительные параметры команде ``gradle build ${build.args}``. Например, чтобы задать свойство ``server.port``:
[code] 
    ```
    build:
      args: -Dserver.port=80
    
    ```
    
[/code]

В этом случае для сборки будет выполнена команда ``gradle build -Dserver.port=80``.

Параметр ``artifacts`` позволяет указать какие файлы должны попасть в итоговое приложение. По умолчанию будут скопированы все файлы с расширением ``jar`` из папки ``build/libs`` в корень папки приложения.

Параметр ``artifacts`` в отличие от других параметров это не строка, а словарь. Ключ в нем это маска файлов источника копирования, а значение: папка, в которую будут скопированы файлы.

Так, значение ``artifacts`` по умолчанию:
[code] 
    ```
    build:
      artifacts:
        "build/libs/*.jar": /
    
    ```
    
[/code]

Мы используем следующие правила копирования:
* в качестве источника указываются только относительные пути (без / в начале);
* если под маску попал файл, файл будет скопирован в папку назначения без исходного пути;
* если под маску попала папка, она будет скопирована целиком в папку назначения вместе со всем содержимым.

## Секция run

В сети ``run`` могут быть использованы следующие параметры:
* ``jarName``
* ``image``
* ``persistenceMount``
* ``containerPort``

Параметр ``jarName`` является обязательным. Он указывает путь до файла с расширением ``jar``, который нужно запустить командой ``java -jar ${run.jarName}``. Пример:
[code] 
    ```
    run:
      jarName: bin/bag-end.jar
    
    ```
    
[/code]

Параметр ``image`` позволяет использовать другой образ для сборки, а не тот, который предлагается Amvera. Образ должен удовлетворять следующим требованиям:
* результат сборки ожидается в папке /app (или образу без разницы, где будет находиться результат сборки);
* в образе присутствует команда ``java``, которую можно вызвать по имени (без указания абсолютного пути) с параметрами ``-jar имя_образа``.

Параметр ``persistenceMount`` позволяет указать, в какую директорию будет примонтирована папка с [постоянным хранилищем](<../storage.html#data>). По умолчанию имеет значение ``/data``.

Параметр ``containerPort`` позволяет указать какой порт слушает приложение. По умолчанию имеет значение ``80``.

## Рецепты

### Минимальный файл amvera.yml
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: gradle
        version: 11
    
    run:
      jarName: bag-end.jar
    
    ```
    
[/code]

### Spring Boot
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: gradle
        version: 11
    
    run:
      jarName: bag-end-1.0.0.jar
      containerPort: 8080
    
    ```
    
[/code]

[ Next JVM Maven ](<jvm-maven.html>) [ Previous Python Pip ](<python-pip.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Python Pip](https://docs.amvera.ru/python-pip.html)

→ [JVM Maven](https://docs.amvera.ru/jvm-maven.html)
