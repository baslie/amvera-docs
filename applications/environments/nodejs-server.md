---
title: Node.JS Server¶
url: https://docs.amvera.ru/applications/environments/nodejs-server.html
path: applications/environments/nodejs-server
prev: Node.JS Browser
next: Go
---

# Node.JS Server¶

## Содержание

- Node.JS Server
- Секция meta
- Секция build
- Секция run
- Рецепты
  - Минимальный файл amvera.yml
  - Приложение на Express.JS
  - Приложение с фазой сборки (Webpack, tsc и т.п.)

---

Back to top

[ View this page ](<../../_sources/applications/environments/nodejs-server.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Node.JS Server

Данная конфигурация подходит если проект использует окружение node.js и пакетный менеджер npm. В этом случае проект может быть написан на таких языках как JavaScript или TypeScript. Данная конфигурация предназначена для серверных приложений. Для браузерных приложений см. [Node.JS Browser](<nodejs-browser.html>).

Написать yaml файл можно как самостоятельно, используя инструкцию ниже, так и воспользоваться нашим генератором yaml, перейдя по [ссылке](<https://manifest.amvera.ru/>).

## Секция meta

Секция ``meta`` файла ``amvera.yml`` будет выглядеть следующим образом:
[code] 
    ```
    meta:
      environment: node
      toolchain:
        name: npm
        version: 18
    
    ```
    
[/code]

Из параметров, которые можно здесь менять это ``meta.toolchain.version``. Логически это версия node.js, которую нужно использовать для работы. Технически значение ``version`` подставляется в имя образа Docker, который будет использован.

Как для фазы сборки, так и для фазы запуска используется образ ``node:${meta.toolchain.version}``. Так как для обеих фаз используется один и тот же образ, ``meta.toolchain.version`` может быть любым тегом этого образа на [докер хабе](<https://hub.docker.com/_/node/tags>).

## Секция build

Секция ``build`` может содержать следующие необязательные параметры:
* ``skip``
* ``additionalCommands``
* ``artifacts``

Параметр ``skip`` может использоваться если у проекта нет зависимостей и/или отсутствует файл ``package.json``. Указание ``skip: yes`` будет означать, что фаза сборки будет целиком пропущена.

Во время сборки скрипт выполняет команду ``npm install``. Если нужно выполнить дополнительные команды (например, ``npm run build``), используется параметр ``additionalCommands``:
[code] 
    ```
    build:
      additionalCommands: npm run build
    
    ```
    
[/code]

В этом случае во время сборки будет выполнена команда ``npm install && npm run build``.

Параметр ``artifacts`` позволяет указать какие файлы должны попасть в итоговое приложение. По умолчанию будут скопированы все файлы в корень папки приложения.

Параметр ``artifacts`` в отличие от других параметров это не строка, а словарь. Ключ в нем это маска файлов источника копирования, а значение: папка, в которую будут скопированы файлы.

Так, значение ``artifacts`` по умолчанию:
[code] 
    ```
    build:
      artifacts:
        "*": /
    
    ```
    
[/code]

Мы используем следующие правила копирования:
* в качестве источника указываются только относительные пути (без / в начале);
* если под маску попал файл, файл будет скопирован в папку назначения без исходного пути;
* если под маску попала папка, она будет скопирована целиком в папку назначения вместе со всем содержимым.

## Секция run

Секция ``run`` может содержать следующие параметры:
* ``scriptName``
* ``scriptArguments``
* ``nodeArguments``
* ``command``
* ``persistenceMount``
* ``persistenceMount``

Параметр ``scriptName`` является обязательным.

Для запуска используется команда ``node ${run.nodeArguments} ${run.scriptName} ${run.scriptArguments}``.

Параметр ``scriptName`` это путь до скрипта, который нужно запустить, относительно корня репозитория.
[code] 
    ```
    run:
      scriptName: index.js
    
    ```
    
[/code]

Если нужно передать дополнительные аргументы ``node`` для запуска, их нужно указать в параметре ``nodeArguments``.

Если нужно передать дополнительные аргументы сприкту для запуска, их нужно указать в параметре ``scriptArguments``.

Параметр ``command`` это альтернативный способ запустить приложение. При использовании этого параметра игнорируются параметры ``scriptName``, ``scriptArguments`` и ``nodeArguments``. Он должен содержать полную команду запуска:
[code] 
    ```
    run:
      command: npm run start
    
    ```
    
[/code]

Параметр ``persistenceMount`` позволяет указать, в какую директорию будет примонтирована папка с [постоянным хранилищем](<../storage.html#data>). По умолчанию имеет значение ``/data``.

Параметр ``containerPort`` позволяет указать какой порт слушает приложение. По умолчанию имеет значение ``80``.

## Рецепты

### Минимальный файл amvera.yml
[code] 
    ```
    meta:
      environment: node
      toolchain:
        name: npm
        version: 18
    
    run:
      scriptName: index.js
    
    ```
    
[/code]

### Приложение на Express.JS
[code] 
    ```
    meta:
      environment: node
      toolchain:
        name: npm
        version: 18
    
    run:
      scriptName: index.js
      containerPort: 3000
    
    ```
    
[/code]

### Приложение с фазой сборки (Webpack, tsc и т.п.)

Данный файл предполагает, что скрипт запуска сборки прописан в задаче ``build``, а также скрипт запуска приложения прописан в задаче ``start`` в файле ``package.json``
[code] 
    ```
    meta:
      environment: node
      toolchain:
        name: npm
        version: 18
    
    build:
      additionalCommands: npm run build
    
    run:
      command: npm run start
      containerPort: 3000
    
    ```
    
[/code]

[ Next Go ](<golang-go.html>) [ Previous Node.JS Browser ](<nodejs-browser.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Node.JS Browser](https://docs.amvera.ru/nodejs-browser.html)

→ [Go](https://docs.amvera.ru/golang-go.html)
