---
title: SQLite¶
url: https://docs.amvera.ru/databases/sqlite.html
path: databases/sqlite
prev: PostgreSQL
next: MongoDB
---

# SQLite¶

## Содержание

- SQLite
- Использование в Amvera
- Примеры подключения
  - Python
  - Java
  - JavaScript (Node.js)
  - Видеопример

---

Back to top

[ View this page ](<../_sources/databases/sqlite.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# SQLite

SQLite — это компактная встраиваемая реляционная база данных. Она не требует отдельного сервера для работы, что делает её идеальным выбором для легковесных приложений, для разработки, тестирования и производства. SQLite используют в тех случаях, когда не требуется сложная обработка транзакций или большая параллельная нагрузка.

SQLite обеспечивает поддержку основных функций SQL, включая транзакции, индексы, триггеры и большинство стандартных типов данных. Файлы баз данных SQLite, как правило, имеют расширение ``.db``, ``.sqlite`` или ``.sqlite3``.

## Использование в Amvera

Для использования SQLite в Amvera отдельное приложение/контейнер не требуется. Достаточно в коде указать путь для хранения файлов этой БД в постоянное хранилище. Если библиотека для вашего языка требует ручного создания файла БД или необходимо инициализировать БД уже имеющимся файлом, то его необходимо загрузить через интерфейс, выбрав папку Data вкладки «Репозиторий».

![data_folder](images/data_folder.png)

> **⚠️ Предупреждение** > > Важно Сохранять данные в папку постоянного хранилища, указанного в конфигурации (по умолчанию /data). Папка data в корне репозитория и /data это разные директории. 

> **ATTENTION** > > Внимание Файлы БД, сохраненные в папке Artifacts (откуда запускается код) могу быть затерты и потеряны при пересборке проекта. 

Чтобы проверить, куда сохраняются данные, папку Data в разделе «Репозиторий». Если в папке отсутствует требуемый файл, вероятно, сохранение данных идет в папку Artifacts (её можно посмотреть так-же).

Корректный путь (при условии дефолтной папки монтирования ``/data``) выглядит следующим образом (не забудьте про / перед data):

``/data/sqlite_database.db``

> **HINT** > > Подсказка Для удобства локального тестирования и проверки путей вы можете создать директорию /data на локальном компьютере. Это позволит тестировать локально, используя верные пути при отправке приложения в облако. 

## Примеры подключения

В приведенных примерах предполагается, что папка постоянного хранилища примонтирована в дефолтную папку ``/data``.

### Python

В Python для работы с SQLite часто используется встроенная библиотека sqlite3.
[code] 
    ```
    import sqlite3
    
    # Подключение к базе данных (или создание, если она не существует)
    conn = sqlite3.connect('/data/example.db')
    
    # Создание объекта cursor для выполнения SQL-запросов
    cursor = conn.cursor()
    
    # Выполнение запроса
    # cursor.execute('''CREATE TABLE stocks
    #              (date text, trans text, symbol text, qty real, price real)''')
    
    # Закрытие соединения
    conn.close()
    
    ```
    
[/code]

### Java

В Java для подключения к SQLite можно использовать JDBC драйвер, например, SQLite JDBC от xerial.
[code] 
    ```
    import java.sql.Connection;
    import java.sql.DriverManager;
    import java.sql.SQLException;
    
    public class Main {
        public static void main(String[] args) {
            Connection connection = null;
            try {
                // Подключение к БД
                String url = "jdbc:sqlite:/data/database.db";
                connection = DriverManager.getConnection(url);
                
                System.out.println("Подключение к SQLite выполнено успешно");
                
                // Здесь могут быть запросы и обработка данных
                
            } catch (SQLException e) {
                System.out.println(e.getMessage());
            } finally {
                try {
                    if (connection != null) {
                        connection.close();
                    }
                } catch (SQLException ex) {
                    System.out.println(ex.getMessage());
                }
            }
        }
    }
    
    ```
    
[/code]

### JavaScript (Node.js)

В Node.js с SQLite можно работать через пакет sqlite3, который нужно установить через npm.
[code] 
    ```
    const sqlite3 = require('sqlite3').verbose();
    
    // Подключение к БД
    let db = new sqlite3.Database('/data/example.db', sqlite3.OPEN_READWRITE | sqlite3.OPEN_CREATE, (err) => {
        if (err) {
            return console.error(err.message);
        }
        console.log('Подключение к SQLite базе данных выполнено успешно.');
    });
    
    // Закрытие подключения
    db.close((err) => {
        if (err) {
            return console.error(err.message);
        }
        console.log('Соединение с базой данных закрыто.');
    });
    
    ```
    
[/code]

### Видеопример

Видео в VK Video для просмотра без VPN доступно по [ссылке](<https://vkvideo.ru/video-167699755_456239049>).

Видео в YouTube:

[ Next MongoDB ](<mongodb.html>) [ Previous PostgreSQL ](<postgreSQL.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [PostgreSQL](postgreSQL.md)

→ [MongoDB](mongodb.md)
