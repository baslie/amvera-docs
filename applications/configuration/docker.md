---
title: Docker¶
url: https://docs.amvera.ru/applications/configuration/docker.html
path: applications/configuration/docker
prev: Файл конфигурации
next: Поддерживаемые окружения
---

# Docker¶

## Содержание

- Docker
- Конфигурационный файл
  - Секция meta
  - Секция build
  - Секция run
- Рецепты
  - Свое приложение с нестандартным портом
  - Свое приложение с нестандартным расположением Dockerfile
  - Готовый образ Docker
  - Приложение с нестандартным ENTRYPOINT

---

Back to top

[ View this page ](<../../_sources/applications/configuration/docker.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Docker

Для сборки и развертывания своего приложения в контейнере Docker можно вообще не писать [файл конфигурации](<config-file.html>) ``amvera.yaml``. Достаточно написать ``Dockerfile``. Однако для более точной настройки файл ``amvera.yaml`` может понадобиться.

![settings](images/docker_proc.png)

При использовании Dockerfile сначала инициализируется процесс сборки образа, запускаемый в отдельном контейнере с доступом к папке Code. По завершению сборки образ загружается в хранилище (registry). После успешной загрузки образа инициализируется процесс разворачивания приложения, в процессе которого создается контейнер из полученного ранее образа.

> **⚠️ Предупреждение** > > Важно docker-compose.yml не поддерживается. Возможно использование только классического Dockerfile. 

## Конфигурационный файл

Для более точной настройки следует дополнять Dockerfile файлом конфигурации amvera.yaml. Использование файла конфигураци дает возможность пропустить сборку и запускать приложение с использованием готового общедоступного образа из сторонних registry. Так-же можно указать папку монтирования постоянного хранилища.

### Секция meta

Для окружения Docker секцию ``meta`` можно не писать вовсе. По умолчанию, она подразумевается следующей:
[code] 
    ```
    meta:
      environment: docker
      toolchain:
        name: docker
    
    ```
    
[/code]

### Секция build

Секция ``build`` поддерживает следующие параметры:
* ``dockerfile``: путь до файла ``Dockerfile`` относительно папки с исходным кодом (без слэша в начале); это необязательный параметр: если его не указать, то ``Dockerfile`` будет искаться в следующих местах:
* ``amvera/Dockerfile``
* ``Dockerfile``
* ``docker/Dockerfile``
* ``deploy/Dockerfile``
* ``deployment/Dockerfile``
* ``skip``: пропуск сборки образа; применяется при запуске готовых образов Docker.

### Секция run

Секция ``run`` поддерживает следующие параметры:
* ``image``: образ для запуска вместо собранного; обычно используется в сочетании с ``build.skip: yes``;
* ``command``: команда для запуска в указанном образе; полезно в сочетании с ``run.image``; в этом параметре указывается то, что указывается в параметре ``ENTRYPOINT`` докерфайла или в параметре ``command`` для пода Kubernetes: обычно это имя команды без параметров;
* ``args``: параметры команды, указанной в ``run.command``; в этом параметре указвается то, что указывается в параметре ``CMD`` докерфайла или в параметре ``args`` для пода Kubernetes с тем отличием, что здесь параметры указываются обычной строкой, а не массивом;
* ``persistenceMount``: абсолютный путь в файловой системе контейнера, куда должна быть примонтирована [папка с постоянным хранилищем](<../storage.html#data>); по умолчанию равен ``/data``
* ``containerPort``: номер порта TCP, который слушает приложение в контейнере; по умолчанию равен ``80``.

## Рецепты

### Свое приложение с нестандартным портом

Если ваше приложение работает по протоколу HTTP, но использует номер порта, отличный от 80, это можно настроить следующим файлом ``amvera.yml``:
[code] 
    ```
    run:
      containerPort: 3000
    
    ```
    
[/code]

### Свое приложение с нестандартным расположением Dockerfile

Допустим, Dockerfile находится по пути ``myapp/amvera.dockerfile``:
[code] 
    ```
    build:
      dockerfile: myapp/amvera.dockerfile
    
    ```
    
[/code]

### Готовый образ Docker

Если вам нужно запустить готовый образ Docker, который либо работает по протоколу HTTP, либо не принимает входящих соединений (например, бот), можно пропустить фазу сборки и указать имя образа напрямую.

В качестве примера рассмотрим развертывание Dokuwiki (<https://hub.docker.com/r/linuxserver/dokuwiki>). Dokuwiki слушает порт 80, поэтому эту настройку нам менять не нужно. А вот папку с данными нужно примонтировать по пути ``/config``. Получается следующий файл ``amvera.yml``:
[code] 
    ```
    build:
      skip: yes
    
    run:
      image: lscr.io/linuxserver/dokuwiki:latest
      persistenceMount: /config
    
    ```
    
[/code]

Так как кроме файла ``amvera.yml`` вам ничего не нужно, это единственный файл, который нужно отправить в репозиторий git, созданный для проекта.

### Приложение с нестандартным ENTRYPOINT

Если вы написали приложение и не указали в ``Dockerfile`` ни ``ENTRYPOINT``, ни ``CMD``, либо используете готовый образ и хотите использовать иное приложение из него, нежели было предусмотрено разработчиком, вам пригодятся параметры ``run.command`` и ``run.args``.

Для примера рассмотрим приложение на языке Go:
[code] 
    ```
    package main
    
    import (
    	"fmt"
    	"net/http"
    	"os"
    )
    
    func main() {
    	var port string
    	if (len(os.Args) > 2) && (os.Args[1] == "--port") {
    		port = fmt.Sprintf(":%v", os.Args[2])
    	} else {
    		port = ":80"
    	}
    
    	http.HandleFunc("/", HelloServer)
    	http.ListenAndServe(port, nil)
    }
    
    func HelloServer(w http.ResponseWriter, r *http.Request) {
    	fmt.Fprintf(w, "Hello, %s!", r.URL.Path[1:])
    }
    
    ```
    
[/code]

``Dockerfile`` для него:
[code] 
    ```
    FROM golang:1.19
    
    WORKDIR /app
    
    COPY server.go go.mod ./
    
    RUN CGO_ENABLED=0 go build -a -installsuffix cgo -o server
    
    FROM alpine:latest
    
    WORKDIR /app
    
    COPY --from=0 /app/server ./
    
    ```
    
[/code]

Чтобы запустить сервер, нужно вызвать команду ``/app/server --port 8080``, но в ``Dockerfile`` по тем или иным причинам это не написано. Укажем параметры запуска в ``amvera.yml``:
[code] 
    ```
    run:
      command: /app/server
      args: --port 8080
      containerPort: 8080
    
    ```
    
[/code]

[ Next Поддерживаемые окружения ](<../supported-env.html>) [ Previous Файл конфигурации ](<config-file.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Файл конфигурации](https://docs.amvera.ru/config-file.html)

→ [Поддерживаемые окружения](https://docs.amvera.ru/supported-env.html)
