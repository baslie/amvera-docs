---
title: Перенос проекта из Heroku¶
url: https://docs.amvera.ru/applications/configuration/heroku-migration.html
path: applications/configuration/heroku-migration
prev: Selenium и Chromedriver
next: Переменные и секреты
---

# Перенос проекта из Heroku¶

## Содержание

- Перенос проекта из Heroku
- Сравнение процессов Heroku и Amvera
  - Процесс развертывания
  - Окружение
  - Управление процессами
- Heroku проект с Procfile
- Heroku проект с heroku.yml
- Heroku Review Apps
- Возможные ошибки
- Решение проблем

---

Back to top

[ View this page ](<../../_sources/applications/configuration/heroku-migration.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Перенос проекта из Heroku

В данном разделе будет предложен вариант миграции приложения из Heroku в облако Amvera.

## Сравнение процессов Heroku и Amvera

Несмотря на то, что Amvera вдохновлялся идеями Heroku, это не 100% клон и некоторые вещи делаются по-разному. Важно понимать различия между этими двумя сервисами при переносе своего проекта.

В этом видео подробно сравниваются два сервиса для развертывания приложений: Heroku и Amvera, а также показан пример переноса проекта с Heroku на Amvera.

### Процесс развертывания

Heroku использует трехступенчатый процесс развертывания приложения:
1. Сборка (``build``)
2. Подготовка релиза (``release``)
3. Запуск (``run``)

На данный момент в Amvera используется двухтупенчатый процесс:
1. Сборка (``build``)
2. Запуск (``run``)

В большинстве сценариев процесс ``release`` может быть включен в фазу сборки. В будущем фаза ``release`` может быть добавлена и в Amvera. Если вам нужна эта фаза, [напишите нам](<mailto:support%40amvera.ru>): мы передвинем эту задачу в более раннюю версию Amvera.

### Окружение

Heroku пытается определить окружение (инструменты сборки, зависимости, команды) самостоятельно и, если это не получается, или Heroku делает это неправильно (сборка приложения проваливается), то пользователь может указать окружение самостоятельно. Такие окружения в Heroku называется ``buildpack``. Управление ``buildpack`` осуществляется инструментом командной строки ``heroku``.

Amvera позволяет указать окружение в [файле конфигурации](<config-file.html>) ``amvera.yml``. Если окружение не указано, подразумевается окружение ``docker`` (аналог стека ``Container`` в Heroku).

Написать yaml файл можно как самостоятельно, используя инструкцию ниже, так и воспользоваться нашим генератором yaml, перейдя по [ссылке](<https://manifest.amvera.ru/>).

### Управление процессами

Heroku позволяет запустить процессы нескольких типов из одного проекта: например, основной процесс, слушающий веб интерфейс (``web``), и вспомогательные рабочие процессы (``worker``). Такие процессы могут масштабироваться индивидуально.

Amvera придерживается принципа «один проект» == «один тип процесса». Каждый проект может также масштабироваться индивидуально. Так, если у вас монорепозиторий, в котором находится код как основного процесса, так и код рабочих процессов, вам придется отправить его в два разных проекта на Amvera. В идеале: разделить код на несколько репозиториев.

Если вам нужна поддержка процессов разных типов в одном проекте, [напишите нам](<mailto:support%40amvera.ru>), мы готовы внедрить такой подход при достаточном интересе со стороны пользователей.

## Heroku проект с Procfile

Простой вариант: один тип процесса, нет процесса ``release``.

Python

``Procfile``:
[code] 
    ```
    web: python3 bagend.py
    
    ```
    
[/code]

Создайте в корне репозитория файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: python
      toolchain:
        name: pip
    
    run:
      scriptName: bagend.py
    
    ```
    
[/code]

[Подробнее о том, что это значит](<../environments/python-pip.html>)

JavaScript (node.js)

``Procfile``:
[code] 
    ```
    web: node bagend.js
    
    ```
    
[/code]

Создайте в корне репозитория файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: node
      toolchain:
        name: npm
        version: 18
    
    run:
      scriptName: bagend.js
    
    ```
    
[/code]

[Подробнее о том, что это значит](<../environments/nodejs-server.html>)

JVM (Maven)

``Procfile``:
[code] 
    ```
    web: java -jar bagend.jar
    
    ```
    
[/code]

Создайте в корне репозитория файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: maven
        version: 17
    
    run:
      jarName: bagend.jar
    
    ```
    
[/code]

[Подробнее о том, что это значит](<../environments/jvm-maven.html>)

JVM (Gradle)

``Procfile``:
[code] 
    ```
    web: java -jar bagend.jar
    
    ```
    
[/code]

Создайте в корне репозитория файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: gradle
        version: 17
    
    run:
      jarName: bagend.jar
    
    ```
    
[/code]

[Подробнее о том, что это значит](<../environments/jvm-gradle.html>)

Что-то другое

Скорее всего, мы пока не поддерживаем сборку вашего окружения нативно и вместо этого нужно использовать окружение ``docker``. Однако список поддерживаемых окружений со временем обновляется, актуальную информацию о поддерживаемых окружениях можно найти [здесь](<../supported-env.html>).

Чтобы не оставлять вас совсем без информации как это делается, рассмотрим пример приложения на Go:

``Dockerfile``:
[code] 
    ```
    FROM golang:1.19
    
    WORKDIR /app
    
    COPY bagend.go go.mod ./
    
    RUN CGO_ENABLED=0 go build -a -installsuffix cgo -o bagend
    
    FROM alpine:latest
    
    WORKDIR /app
    
    COPY --from=0 /app/bagend ./
    
    ```
    
[/code]

``amvera.yml``:
[code] 
    ```
    run:
      command: /app/bagend
      args: --port 8080
      containerPort: 8080
    
    ```
    
[/code]

[Подробнее о том, что это значит](<docker.html>)

Вариант с одним типом процесса, есть процесс ``release``.

Python

``Procfile``:
[code] 
    ```
    release: python3 manage.py migrate
    web: python3 manage.py runserver 0.0.0.0:8000
    
    ```
    
[/code]

Создайте в корне репозитория файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: python
      toolchain:
        name: pip
    
    run:
      command: python3 manage.py migrate && python3 manage.py runserver 0.0.0.0:8000
      containerPort: 8000
    
    ```
    
[/code]

[Подробнее о том, что это значит](<../environments/python-pip.html>)

JavaScript (node.js)

``Procfile``:
[code] 
    ```
    release: npm run migrate
    web: node bagend.js
    
    ```
    
[/code]

Создайте в корне репозитория файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: node
      toolchain:
        name: npm
        version: 18
    
    build:
      additionalCommands: npm run migrate
    
    run:
      scriptName: bagend.js
    
    ```
    
[/code]

[Подробнее о том, что это значит](<../environments/nodejs-server.html>)

Что-то другое

Скорее всего, мы пока не поддерживаем сборку вашего окружения нативно или для вашего стека технологий пока нет возможности запускать произвольные команды и вместо этого нужно использовать окружение ``docker``. Однако список поддерживаемых окружений со временем обновляется, актуальную информацию о поддерживаемых окружениях можно найти [здесь](<../supported-env.html>).

Чтобы не оставлять вас совсем без информации как это делается, рассмотрим пример приложения на Go:

``Dockerfile``:
[code] 
    ```
    FROM golang:1.19
    
    WORKDIR /app
    
    COPY bagend.go go.mod ./
    
    RUN CGO_ENABLED=0 go build -a -installsuffix cgo -o bagend
    
    FROM alpine:latest
    
    WORKDIR /app
    
    COPY --from=0 /app/bagend ./
    
    ```
    
[/code]

``amvera.yml``:
[code] 
    ```
    run:
      command: bash
      args: -c "./migrate.sh && /app/bagend"
      containerPort: 8080
    
    ```
    
[/code]

[Подробнее о том, что это значит](<docker.html>)

Вариант с несколькими типами процессов.

Python

``Procfile``:
[code] 
    ```
    web: python3 bagend.py
    worker: python3 erebor.py
    
    ```
    
[/code]

Разделите проект на два. В первом (будет аналогом процесса ``web``) в корне создайте файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: python
      toolchain:
        name: pip
    
    run:
      scriptName: bagend.py
    
    ```
    
[/code]

Во втором проекте (будет аналогом процесса ``worker``) в корне создайте файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: python
      toolchain:
        name: pip
    
    run:
      scriptName: erebor.py
    
    ```
    
[/code]

[Подробнее о том, что это значит](<../environments/python-pip.html>)

JavaScript (node.js)

``Procfile``:
[code] 
    ```
    web: node bagend.js
    worker: node erebor.js
    
    ```
    
[/code]

Разделите проект на два. В первом (будет аналогом процесса ``web``) в корне создайте файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: node
      toolchain:
        name: npm
        version: 18
    
    run:
      scriptName: bagend.js
    
    ```
    
[/code]

Во втором проекте (будет аналогом процесса ``worker``) в корне создайте файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: node
      toolchain:
        name: npm
        version: 18
    
    run:
      scriptName: erebor.js
    
    ```
    
[/code]

[Подробнее о том, что это значит](<../environments/nodejs-server.html>)

JVM (Maven)

``Procfile``:
[code] 
    ```
    web: java -jar bagend.jar
    worker: java -jar erebor.jar
    
    ```
    
[/code]

Разделите проект на два. В первом (будет аналогом процесса ``web``) в корне создайте файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: maven
        version: 17
    
    run:
      jarName: bagend.jar
    
    ```
    
[/code]

Во втором проекте (будет аналогом процесса ``worker``) в корне создайте файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: maven
        version: 17
    
    run:
      jarName: erebor.jar
    
    ```
    
[/code]

[Подробнее о том, что это значит](<../environments/jvm-maven.html>)

JVM (Gradle)

``Procfile``:
[code] 
    ```
    web: java -jar bagend.jar
    worker: java -jar erebor.jar
    
    ```
    
[/code]

Разделите проект на два. В первом (будет аналогом процесса ``web``) в корне создайте файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: gradle
        version: 17
    
    run:
      jarName: bagend.jar
    
    ```
    
[/code]

Во втором проекте (будет аналогом процесса ``worker``) в корне создайте файл ``amvera.yml``:
[code] 
    ```
    meta:
      environment: jvm
      toolchain:
        name: gradle
        version: 17
    
    run:
      jarName: erebor.jar
    
    ```
    
[/code]

[Подробнее о том, что это значит](<../environments/jvm-gradle.html>)

Что-то другое

Скорее всего, мы пока не поддерживаем сборку вашего окружения нативно и вместо этого нужно использовать окружение ``docker``. Однако список поддерживаемых окружений со временем обновляется, актуальную информацию о поддерживаемых окружениях можно найти [здесь](<../supported-env.html>).

Общая рекомендация: разделите ваш репозиторий на столько проектов, сколько у вас процессов в ``Procfile``. Для каждого создайте файлы ``Dockerfile`` и при необходимости ``amvera.yml``.

Чтобы не оставлять вас совсем без информации как это делается, рассмотрим пример приложения на Go:

``Dockerfile``:
[code] 
    ```
    FROM golang:1.19
    
    WORKDIR /app
    
    COPY bagend.go go.mod ./
    
    RUN CGO_ENABLED=0 go build -a -installsuffix cgo -o bagend
    
    FROM alpine:latest
    
    WORKDIR /app
    
    COPY --from=0 /app/bagend ./
    
    ```
    
[/code]

``amvera.yml``:
[code] 
    ```
    run:
      command: /app/bagend
      args: --port 8080
      containerPort: 8080
    
    ```
    
[/code]

[Подробнее о том, что это значит](<docker.html>)

## Heroku проект с heroku.yml

Файл ``heroku.yml`` позволяет указать как собирать образ Docker из вашего приложения. Для схожих целей применяется ``amvera.yml``.

Секция ``setup`` файла ``heroku.yml`` на данный момент не имеет аналогов из-за того, что в Amvera пока не реализованы адд-оны (но мы работаем над этим), а также не реализован проброс переменных окружения (мы так же работаем над этим, но из-за того, что переменные окружения могут содержать конфиденциальную информацию, такую как ключи доступа и пароли, доступ к ним будет осуществляться через веб-интерфейс и интерфейс командной строки).

Секция ``release`` файла ``heroku.yml`` также на данный момент не поддерживается из-за отсутствия фазы ``release`` в Amvera. Действия, описанные в этой фазе, на данный момент лучше указать в ``Dockerfile``.

Секция ``build`` файла ``heroku.yml`` позволяет указать местоположение ``Dockerfile``, а также переменные окружения для фазы сборки. Так как проброс переменных окружения еще не реализован, эта возможность не поддерживается.

``Dockerfile`` в Amvera ищется по предпопределенным путям:
* ``amvera/Dockerfile``
* ``Dockerfile``
* ``docker/Dockerfile``
* ``deploy/Dockerfile``
* ``deployment/Dockerfile``

Если у вас ``Dockerfile`` расположен по одному из этих путей, он подхватится автоматически. Если нет – путь до него можно указать в ``amvera.yml``:

``heroku.yml``
[code] 
    ```
    build:
      docker:
        web: web/Dockerfile
    
    ```
    
[/code]

``amvera.yml``
[code] 
    ```
    build:
      dockerfile: web/Dockerfile
    
    ```
    
[/code]

Секция ``run`` файла ``heroku.yml`` позволяет указать команду в докер образе для запуска. Если команда указана в самом ``Dockerfile``, она не указывается. Аналогично делается и в ``amvera.yml``, но с несколько иным синтаксисом.

``heroku.yml``
[code] 
    ```
    build:
      docker:
        web: Dockerfile
    
    run:
      web: /app/bagend --port $PORT
    
    ```
    
[/code]

``amvera.yml``
[code] 
    ```
    run:
      command: /app/bagend
      args: --port 8080
      containerPort: 8080
    
    ```
    
[/code]

Обратите внимание: мы указали конкретный порт для приложения. Если ваше приложение опирается на наличие переменной окружения ``PORT``, она тоже задается и по умолчанию равна ``80`` или тому значению ``run.containerPort``, которое вы указали в ``amvera.yml``.

[Подробнее о развертывании образов Docker](<docker.html>)

## Heroku Review Apps

На данный момент Review Apps в том виде, в котором они реализованы в Heroku, не поддерживаются.

## Возможные ошибки

Если у вас при клонировании или пуше в репозиторий Amvera возникает ошибка 404, но вы уверены, что прописали адрес репозитория верно (например, скопировали) скорее всего клиент git пытается авторизоваться с запомненными учетными данными другого репозитория (GitHub, Heroku, etc). Для того, чтобы выполнить вход с учетными данными Amvera, необходимо «забыть» старые учетные данные.

Для Windows

Control Panel -> Credential Manager

В разделе Generic Credentials найдите учетные данные git (обычно начинаются с ``git:``), разверните их и нажмите кнопку ``Remove``).

После этого клиент git снова запросит данные для входа.

Для Mac OS

В командной строке выполните команду:
[code] 
    ```
    git credential-osxkeychain erase
    
    ```
    
[/code]

Команда ничего не выведет. Напечатайте в командную строку следующее:
[code] 
    ```
    host=git.amvera.ru
    protocol=https
    
    ```
    
[/code]

После этого нажмите клавишу ``<Return>`` два раза. Команда завершит работу. После этого клиент git снова запросит данные для входа.

Для Linux

Откройте файл ``$HOME/.git-credentials`` в текстовом редакторе и удалите нужные записи. После этого клиент git снова запросит данные для входа.

Альтернативным вариантом является выполнение команды
[code] 
    ```
    git config --global --unset user.password
    
    ```
    
[/code]

## Решение проблем

Если вы столкнулись с проблемами в развертывании вашего Heroku-приложения в Amvera или вам не хватает какой-то функциональности, напишите в [техническую поддержку](<mailto:support%40amvera.ru>), мы проконсультируем вас по вопросам развертывания и рассмотрим ваши предложения по функциональности.

[ Next Переменные и секреты ](<variables.html>) [ Previous Selenium и Chromedriver ](<../environments/selenium-chromedriver.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Selenium и Chromedriver](environments/selenium-chromedriver.md)

→ [Переменные и секреты](variables.md)
