---
title: JVM Maven¶
url: https://docs.amvera.ru/applications/environments/jvm-maven.html
path: applications/environments/jvm-maven
prev: JVM Gradle
next: Node.JS Browser
---

# JVM Maven¶

## Содержание

- JVM Maven
- Секция meta
- Секция build
- Секция run
- Рецепты
  - Минимальный файл amvera.yml
  - Spring Boot

---

Back to top

[ View this page ](<../../_sources/applications/environments/jvm-maven.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# JVM Maven

Данная конфигурация подходит, если проект собирается при помощи Maven и запускается на JVM. В этом случае проект может быть написан на таких языках как Java или Kotlin.

Написать yaml файл можно как самостоятельно, используя инструкцию ниже, так и воспользоваться нашим генератором yaml, перейдя по [ссылке](<https://manifest.amvera.ru/>), либо заполнить в разделе «Конфигурация» личного кабинета.

## Секция meta

Секция ``meta`` файла ``amvera.yml`` будет выглядеть следующим образом:
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: maven
        version: 11
    
    ```
    
[/code]

Из параметров, которые можно здесь менять это ``meta.toolchain.version``. Логически это версия JDK, который нужно использовать для сборки. Технически значение ``version`` подставляется в имя образа Docker, который будет использован.

Для фазы сборки это ``maven:3-openjdk-${meta.toolchain.version}``. Допустимые значения можно увидеть на странице [докер хаба](<https://hub.docker.com/_/maven/tags?page=1&amp;name=3-openjdk>).

Для фазы запуска это ``bellsoft/liberica-openjre-debian:${meta.toolchain.version}``. Допустимые значения можно увидеть на странице [докер хаба](<https://hub.docker.com/r/bellsoft/liberica-openjre-debian/tags>).

> **⚠️ Предупреждение** > > Важно Значение meta.toolchain.version должно быть допустимым как для фазы сборки, так и для фазы запуска. Лучше всего подходит простой номер LTS версии JDK. 

## Секция build

В секции ``build`` могут быть указаны следующие параметры:
* ``image``
* ``args``
* ``artifacts``

Параметр ``image`` позволяет использовать другой образ для сборки, а не тот, который предлагается Amvera. Образ должен удовлетворять следующим требованиям:
* исходный код для сборки ожидается в папке /app (или образу без разницы, где будет находиться исходный код);
* в образе присутствует команда ``mvn``, которую можно вызвать по имени (без указания абсолютного пути) с параметрами ``clean package``.

Параметр ``args`` позволяет указать дополнительные параметры команде ``mvn clean package ${build.args}``. Например, если в проекте используется Spring Boot, может потребоваться следующий параметр:
[code] 
    ```
    build:
      args: spring-boot:repackage
    
    ```
    
[/code]

В этом случае для сборки будет выполнена команда ``mvn clean package spring-boot:repackage``.

Указывать можно несколько параметров, все они будут подставлены в команду. Например, если нужно указать еще и профиль мавен:
[code] 
    ```
    build:
      args: spring-boot:repackage -Pproduction
    
    ```
    
[/code]

Параметр ``artifacts`` позволяет указать какие файлы должны попасть в итоговое приложение. По умолчанию будут скопированы все файлы с расширением ``jar`` из папки ``target`` в корень папки приложения.

Параметр ``artifacts`` в отличие от других параметров это не строка, а словарь. Ключ в нем это маска файлов источника копирования, а значение: папка, в которую будут скопированы файлы.

Так, значение ``artifacts`` по умолчанию:
[code] 
    ```
    build:
      artifacts:
        "target/*.jar": /
    
    ```
    
[/code]

Мы используем следующие правила копирования:
* в качестве источника указываются только относительные пути (без / в начале);
* если под маску попал файл, файл будет скопирован в папку назначения без исходного пути;
* если под маску попала папка, она будет скопирована целиком в папку назначения вместе со всем содержимым.

## Секция run

В секции ``run`` могут быть использованы следующие параметры:
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
* в образе присутствует команда ``java``, которую можно вызвать по имени (без указания абсолютного пути) с параметрами ``-jar имя_файла``.

Параметр ``persistenceMount`` позволяет указать, в какую директорию будет примонтирована папка с [постоянным хранилищем](<../storage.html#data>). По умолчанию имеет значение ``/data``.

Параметр ``containerPort`` позволяет указать какой порт слушает приложение. По умолчанию имеет значение ``80``.

## Рецепты

### Минимальный файл amvera.yml
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: maven
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
        name: maven
        version: 17
    
    build:
      args: spring-boot:repackage -Pproduction
    
    run:
      jarName: bag-end-1.0.0.jar
      containerPort: 8080
    
    ```
    
[/code]

[ Next Node.JS Browser ](<nodejs-browser.html>) [ Previous JVM Gradle ](<jvm-gradle.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [JVM Gradle](https://docs.amvera.ru/jvm-gradle.html)

→ [Node.JS Browser](https://docs.amvera.ru/nodejs-browser.html)
