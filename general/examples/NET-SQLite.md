---
title: Web-приложение на C# Microsoft (.NET) с подключением к SQLite¶
url: https://docs.amvera.ru/general/examples/NET-SQLite.html
path: general/examples/NET-SQLite
prev: Ruby on Rails c базой данных SQLite
next: Быстрый старт
---

# Web-приложение на C# Microsoft (.NET) с подключением к SQLite¶

## Содержание

- Web-приложение на C# Microsoft (.NET) с подключением к SQLite
- Напишем простое веб-приложение, которое при открытии сайта выводит всю информацию из базы данных. Будем использовать веб-фреймворк ASP.NET и базу данных SQLite.
- Создание проекта
- База данных (SQLite)
- Конфигурация
- amvera.yml
- Dockerfile
  - Шаги:
- Создание проекта в Amvera
- Проверка работоспособности
- Видеопример
- Если у вас не получается развернуть проект

---

Back to top

[ View this page ](<../../_sources/general/examples/NET-SQLite.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Web-приложение на C# Microsoft (.NET) с подключением к SQLite

**Чтобы развернуть приложение в Amvera, нужно выполнить следующие простые шаги:**
1. Открываем страницу https://cloud.amvera.ru/projects
2. Нажимаем кнопку _Создать_ и выбираем _тип сервиса_ **приложение**.
3. Выгружаем все файлы (можно через git, а можно через интерфейс). Не забудьте добавить конфигурационный файл (amvera.yml) и/или Dockerfile.
4. После этого начнется [сборка](<applications/build.md>) и [развертывание](<applications/run.md>) приложения. Дождитесь появления статуса «Успешно развернуто».

Рассмотрим процесс подробнее.

## Напишем простое веб-приложение, которое при открытии сайта выводит всю информацию из базы данных. Будем использовать веб-фреймворк ASP.NET и базу данных SQLite.

## Создание проекта

Для начала создадим новый проект (веб-приложение) с именем «WebService»:
[code] 
    ```
    $ dotnet new web -n WebService
    
    ```
    
[/code]

Создалась директория _WebService_ , перейдем в нее:
[code] 
    ```
    $ cd WebService
    
    ```
    
[/code]

Так как мы будем использовать SQLite, то необходимо его добавить в проект:
[code] 
    ```
    $ dotnet add package System.Data.SQLite
    
    ```
    
[/code]

## База данных (SQLite)

В файле _Program.cs_ (код доступен в конце документации) реализовано взаимодействие с базой данных. Для использования SQLite в Amvera отдельное приложение/контейнер не требуется. Достаточно в коде указать путь для хранения файлов этой БД в постоянное хранилище. Подробнее можно прочитать [здесь](<databases/sqlite.md>).

> **Важно** : обратите внимание на указанный путь к базе данных. Сохранять изменяемые файлы (базы данных и т.д.) нужно именно в постоянное хранилище /data. Это позволит избежать их потери при пересборке. Папка data в коде и постоянное хранилище /data - разные директории.

## Конфигурация

## amvera.yml

Написать yaml файл можно как самостоятельно, так и воспользоваться нашим генератором yaml, перейдя по [ссылке](<https://manifest.amvera.ru/>), либо заполнить в разделе «Конфигурация» личного кабинета.

**Пример файла amvera.yml:**
[code] 
    ```
    meta:
      environment: csharp
      toolchain:
        name: dotnet
        version: 8.0
    build:
      image: mcr.microsoft.com/dotnet/sdk:8.0
    run:
      image: mcr.microsoft.com/dotnet/sdk:8.0
      buildFileName: bin/WebService
      persistenceMount: /data
      containerPort: 8080
    
    ```
    
[/code]

> **Важно:** Поле buildFileName должно быть в формате bin/имя_файла, либо bin/publish/имя_файла если вы делаете через публикацию.

> **Важно:** Обратите внимание на _containerPort_. При создании приложения в _Program.cs_ порт явно не указывается, поэтому используется значение по умолчанию (8080). Если в файле _amvera.yml_ указан неправильный порт, то возникнет ошибка «Service Unavailable 503». Amvera по умолчанию слушает порт 80.

Рассмотрим альтернативный способ задания конфигурации - через _Dockerfile_.

## Dockerfile

> Замечание: если вы используете Dockerfile, то конфигурационный файл amvera.yml в большинстве случаев можно не добавлять.

### Шаги:
1. Создаем Dockerfile в директории с проектом.
2. В Dockerfile указываем базовый образ для этапа сборки:

[code] 
    ```
    FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
    
    ```
    
[/code]

> вместо 8.0 можете указать любую другую версию, которая вам нужна.
3. Устанавливаем рабочий каталог внутри контейнера:

[code] 
    ```
    WORKDIR /app
    
    ```
    
[/code]
4. Копируем файл _WebService.csproj_ в контейнер:

[code] 
    ```
    COPY WebService.csproj ./
    
    ```
    
[/code]
5. Восстанавливаем зависимости:

[code] 
    ```
    RUN dotnet restore
    
    ```
    
[/code]
6. Копируем все файлы проекта в контейнер:

[code] 
    ```
    COPY . ./
    
    ```
    
[/code]
7. Публикуем приложение для релиза:

[code] 
    ```
    RUN dotnet publish -c Release -o WebService
    
    ```
    
[/code]
8. Указываем базовый образ для этапа запуска:

[code] 
    ```
    FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
    
    ```
    
[/code]
9. Устанавливаем рабочий каталог внутри контейнера:

[code] 
    ```
    WORKDIR /app 
    
    ```
    
[/code]
10. Копируем опубликованный результат из этапа сборки:

[code] 
    ```
    COPY --from=build /app/WebService ./ 
    
    ```
    
[/code]
11. Устанавливаем точку входа для контейнера:

[code] 
    ```
    ENTRYPOINT ["dotnet", "WebService.dll"]
    
    ```
    
[/code]

**Получившийся Dockerfile:**
[code] 
    ```
    FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
    WORKDIR /app
    COPY WebService.csproj ./
    RUN dotnet restore
    COPY . ./
    RUN dotnet publish -c Release -o WebService
    
    FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
    WORKDIR /app
    COPY --from=build /app/WebService ./
    ENTRYPOINT ["dotnet", "WebService.dll"]
    
    ```
    
[/code]

**Пример файла amvera.yml при использовании вместе с Dockerfile:**
[code] 
    ```
    meta:
      environment: docker
      toolchain:
        name: docker
        version: latest
    build:
      dockerfile: Dockerfile
      skip: false
    run:
      persistenceMount: /data
      containerPort: 8080
    
    ```
    
[/code]

## Создание проекта в Amvera

Последний шаг - развернуть приложение. В файле _Program.cs_ содержится основной код и выполняется подключение к базе данных:

**Program.cs**
[code] 
    ```
    using Microsoft.AspNetCore.Builder;
    using Microsoft.AspNetCore.Http;
    using Microsoft.Extensions.DependencyInjection;
    using System;
    using System.Data.SQLite;
    
    var builder = WebApplication.CreateBuilder(args);
    var app = builder.Build();
    
    // Указываем путь к базе данных:
    var connectionString = "Data Source=../data/people.db;";
    
    using var connection = new SQLiteConnection(connectionString);
    connection.Open();
    var createTableCommand = connection.CreateCommand();
    createTableCommand.CommandText = @"
        CREATE TABLE IF NOT EXISTS People (
            Id INTEGER PRIMARY KEY,
            Name TEXT,
            Age INTEGER,
            Occupation TEXT
        );
    ";
    createTableCommand.ExecuteNonQuery();
    
    var insertDataCommand = connection.CreateCommand();
    insertDataCommand.CommandText = @"
        INSERT OR IGNORE INTO People (Name, Age, Occupation) VALUES
        ('John Doe', 30, 'Engineer'),
        ('Jane Smith', 25, 'Teacher'),
        ('Michael Johnson', 35, 'Doctor');
    ";
    insertDataCommand.ExecuteNonQuery();
    
    app.MapGet("/", async () =>
    {
        using var selectCommand = connection.CreateCommand();
        selectCommand.CommandText = "SELECT * FROM People;";
        using var reader = await selectCommand.ExecuteReaderAsync();
        
        string result = "People Database:\n";
        while (reader.Read())
        {
            result += $"ID: {reader.GetInt32(0)}, Name: {reader.GetString(1)}, Age: {reader.GetInt32(2)}, Occupation: {reader.GetString(3)}\n";
        }
            
        return result;
    });
    
    app.Run();
    
    ```
    
[/code]

> При необходимости можете изменить путь к базе данных.
> > Если не хотите использовать стандартный порт (8080), то можно указать другой. Но тогда важно учесть это изменение в конфигурационном файле.

## Проверка работоспособности
1. Переходим в настройки проекта и активируем доменное имя.
2. Теперь можно перейти по нему и откроется наш сайт с записями из базы данных.

Если что-то не работает, рекомендуем ознакомиться с логами Сборки и Приложения.

## Видеопример

Видео в VK Video для просмотра без VPN доступно по [ссылке](<https://vkvideo.ru/video-167699755_456239045>).

Видео в YouTube:

> **⚠️ Предупреждение** > > Важно Сохраняйте файлы БД и иные изменяемые данные в постоянное хранилище, чтобы избежать их потери при обновлении проекта, когда производится «откат» папки код до состояния обновления репозитория. Папка data в корне проекта и директория /data, это разные директории. Проверить, что сохранение идет в /data, можно зайдя в папку «data» на странице «Репозиторий». 

> **⚠️ Предупреждение** > > Важно Чтобы избежать ошибки 502, измените в вашем коде host 127.0.0.1 (или подобный localhost) на 0.0.0.0, и пропишите в конфигурации порт, который слушает ваше приложение (пример - 8080). 

Поздравляем, вы успешно создали свое первое приложение в Amvera!

## Если у вас не получается развернуть проект

Напишите наблюдаемую вами симптоматику на support@amvera.ru с указанием вашего имени пользователя и названия проекта, мы постараемся вам помочь.

[ Next Быстрый старт ](../../applications/quick-start.md) [ Previous Ruby on Rails c базой данных SQLite ](Ruby-SQLite.md)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Ruby on Rails c базой данных SQLite](Ruby-SQLite.md)

→ [Быстрый старт](applications/quick-start.md)
