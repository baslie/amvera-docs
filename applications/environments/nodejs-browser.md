---
title: Node.JS Browser¶
url: https://docs.amvera.ru/applications/environments/nodejs-browser.html
path: applications/environments/nodejs-browser
prev: JVM Maven
next: Node.JS Server
---

# Node.JS Browser¶

## Содержание

- Node.JS Browser
- Секция meta
- Секция build
- Секция run
- Рецепты
  - React
  - Vue
  - Angular

---

Back to top

[ View this page ](<../../_sources/applications/environments/nodejs-browser.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Node.JS Browser

Данная конфигурация подходит если проект использует окружение node.js и пакетный менеджер npm для сборки приложения для работы в браузере. В этом случае проект может быть написан на таких языках как JavaScript или TypeScript. Данная конфигурация предназначена для браузерных приложений. Для серверных приложений см. [Node.JS Server](<https://docs.amvera.ru/books/amvera/page/nodejs-server>).

Написать yaml файл можно как самостоятельно, используя инструкцию ниже, так и воспользоваться нашим генератором yaml, перейдя по [ссылке](<https://manifest.amvera.ru/>).

## Секция meta

Секция ``meta`` файла ``amvera.yml`` будет выглядеть следующим образом:
[code] 
    ```
    meta:
      environment: node
      toolchain:
        name: browser
        version: 18
    
    ```
    
[/code]

Из параметров, которые можно здесь менять это ``meta.toolchain.version``. Логически это версия node.js, которую нужно использовать для работы. Технически значение ``version`` подставляется в имя образа Docker, который будет использован.

Для фазы сборки используется образ ``node:${meta.toolchain.version}``. Так как для фазы запуска node.js вообще не используется, ``meta.toolchain.version`` может быть любым тегом этого образа на [докер хабе](<https://hub.docker.com/_/node/tags>).

## Секция build

Секция ``build`` может содержать следующие необязательные параметры:
* ``additionalCommands``
* ``artifacts``

Во время сборки скрипт выполняет команду ``npm install && npm run build``. Если будет задан параметр ``additionalCommands``, то будет выполнена команда ``npm install && ${build.additionalCommands}``. Это может пригодиться, если в проекте команда сборки называется не ``build``.
[code] 
    ```
    build:
      additionalCommands: npm run amvera:build
    
    ```
    
[/code]

Параметр ``artifacts`` позволяет указать какие файлы должны попасть в итоговое приложение. По умолчанию будут скопированы все файлы из папки ``build`` в корень папки приложения.

Параметр ``artifacts`` в отличие от других параметров это не строка, а словарь. Ключ в нем это маска файлов источника копирования, а значение: папка, в которую будут скопированы файлы.

Так, значение ``artifacts`` по умолчанию:
[code] 
    ```
    build:
      artifacts:
        "build/*": /
    
    ```
    
[/code]

Мы используем следующие правила копирования:
* в качестве источника указываются только относительные пути (без / в начале);
* если под маску попал файл, файл будет скопирован в папку назначения без исходного пути;
* если под маску попала папка, она будет скопирована целиком в папку назначения вместе со всем содержимым.

## Секция run

Для запуска используется модифицированный образ ``nginx:1.23.2`` для поддержки маршрутизации в SPA приложениях.

В секции ``run`` нет настраиваемых параметров.

## Рецепты

### React
[code] 
    ```
    meta:
      environment: node
      toolchain:
        name: browser
        version: 18
    
    ```
    
[/code]

### Vue
[code] 
    ```
    meta:
      environment: node
      toolchain:
        name: browser
        version: 18
    
    build:
      artifacts:
        "dist/*": /
    
    ```
    
[/code]

### Angular
[code] 
    ```
    meta:
      environment: node
      toolchain:
        name: browser
        version: 18
    
    build:
      artifacts:
        "dist/my-app/*": /
    
    ```
    
[/code]

[ Next Node.JS Server ](<nodejs-server.html>) [ Previous JVM Maven ](<jvm-maven.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [JVM Maven](jvm-maven.md)

→ [Node.JS Server](nodejs-server.md)
