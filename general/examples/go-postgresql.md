---
title: Fullstack-приложение на Go c подключением к PostgreSQL¶
url: https://docs.amvera.ru/general/examples/go-postgresql.html
path: general/examples/go-postgresql
prev: Spring Boot с встраиваемой базой данных H2 и PostgreSQL
next: Fullstack на Go
---

# Fullstack-приложение на Go c подключением к PostgreSQL¶

## Содержание

- Fullstack-приложение на Go c подключением к PostgreSQL
- Видеопример деплоя Go-приложения
- Создадим простое web приложение на языке программирования Go, где можно будет оставлять и читать цитаты. Для их хранения будем использовать СУБД PostgreSQL.
- Конфигурация
- Amvera.yml
- Dockerfile
  - Шаги:
- Зависимости (go.mod и sum.go)
- Развертывание СУБД (PostgreSQL)
- Создание проекта в Amvera
- Проверка работоспособности
- Код статических файлов из примера
- Если у вас не получается развернуть проект

---

Back to top

[ View this page ](<../../_sources/general/examples/go-postgresql.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Fullstack-приложение на Go c подключением к PostgreSQL

**Чтобы развернуть основное приложение в Amvera, нужно выполнить следующие простые шаги:**
1. Открываем страницу https://cloud.amvera.ru/projects
2. Нажимаем кнопку _Создать_ и выбираем _тип сервиса_ **приложение**
3. Выгружаем все файлы (можно через git, а можно через интерфейс). Убедитесь, что вы выгрузили все нужные файлы. Для проекта из примера это:
* go.mod (обязательно)
* go.sum (обязательно)
* main.go (обязательно)
* Dockerfile (обязательно)
* static/index.html (если используете)
* static/script.js (если используете)
* static/styles.css (если используете)
* amvera.yml (необязательно)
4. После этого начнется [сборка](<applications/build.md>) и [развертывание](<applications/run.md>) приложения. Дождитесь появления статуса «Успешно развернуто».

Рассмотрим процесс подробнее.

## Видеопример деплоя Go-приложения

Видео в VK Video для просмотра без VPN доступно по [ссылке](<https://vkvideo.ru/video-167699755_456239034>).

Видео в YouTube:

## Создадим простое web приложение на языке программирования Go, где можно будет оставлять и читать цитаты. Для их хранения будем использовать СУБД PostgreSQL.

Директория приложения имеет следующую структуру:
[code] 
    ```
    └─ code/
    ├── static
    │   ├── styles.css
    │   ├── script.js
    │   └── index.html
    ├── amvera.yml
    ├── main.go
    ├── go.sum
    ├── go.mod
    └── Dockerfile 
    
    ```
    
[/code]

> Код статических файлов доступен в конце страницы.

## Конфигурация

## Amvera.yml

Написать yaml файл можно как самостоятельно, так и воспользоваться нашим генератором yaml, перейдя по [ссылке](<https://manifest.amvera.ru/>), либо заполнить в разделе «Конфигурация» личного кабинета.

**Пример файла amvera.yml при использовании только amvera.yml:**
[code] 
    ```
    meta:
      environment: golang
      toolchain:
        name: go
        version: 1.22
    build:
      image: golang:1.22
    run:
      image: golang:1.22
      persistenceMount: /data
      containerPort: 80
    
    ```
    
[/code]

**Пример файла amvera.yml при использовании вместе с Dockerfile:**
[code] 
    ```
    meta:
      environment: docker
      toolchain: docker
    build:
      dockerfile: Dockerfile
      skip: false
    run:
      persistenceMount: /data
      containerPort: "80"
    
    ```
    
[/code]

> Важно: сохранять изменяемые файлы (база данных и т.д.) именно в постоянное хранилище /data. Это позволит избежать их потери при пересборке. Папка Data в коде и постоянное хранилище /data - разные директории.

Рассмотрим альтернативный способ задания конфигурации - через Dockerfile.

## Dockerfile

> Замечание: если вы используете Dockerfile, то конфигурационный файл amvera.yml в большинстве случаев можно не добавлять.

### Шаги:
1. Создаем Dockerfile в директории с проектом.
2. В Dockerfile указываем базовый образ с названием _builder_ : ``FROM golang:1.21.1 AS builder``

> вместо 1.21.1 можете указать любую другую версию, которая вам нужна.
3. Устанавливаем рабочую директорию: ``WORKDIR /app``
4. Копируем файлы _main.go_ , _go.mod_ и _go.sum_ в текущую директорию рабочего каталога: ``COPY main.go go.mod go.sum ./``
5. Выполняем сборку проекта и создаем исполняемый файл _server_ : ``RUN CGO_ENABLED=0 go build -a -installsuffix cgo -o server``
6. Используем образ Alpine Linux (это уменьшит размер конечного образа и улучшит его производительность): ``FROM alpine:latest``
7. Копируем исполняемый файл _server_ , собранный в предыдущем образе, в текущую директорию рабочего каталога: ``COPY --from=builder /app/server ./``
8. Копируем статические файлы внутрь контейнера: ``COPY static/ ./static/``
9. Открываем порт 80 для внешних подключений: ``EXPOSE 80``
10. Добавляем команду для запуска приложения: ``CMD ["./server", "--port", "80"]``

**Получившийся Dockerfile:**
[code] 
    ```
    FROM golang:1.21.1 AS builder
    
    WORKDIR /app
    
    COPY main.go go.mod go.sum ./
    
    RUN CGO_ENABLED=0 go build -a -installsuffix cgo -o server
    
    FROM alpine:latest
    
    COPY --from=builder /app/server ./
    
    COPY static/ ./static/
    
    EXPOSE 80
    
    CMD ["./server", "--port", "80"]
    
    ```
    
[/code]

## Зависимости (go.mod и sum.go)

Инициализируем новый модуль для управления зависимостями. Для этого выполняем следующую команду, которая создаст файл _go.mod_ :
[code] 
    ```
    $ go mod init main
    
    ```
    
[/code]

> Замечание: вместо main можно написать произвольную строку

Остаётся выполнить ещё одну команду, которая добавит по две записи на каждую зависимость и создаст файл _go.sum_ :
[code] 
    ```
    go mod tidy
    
    ```
    
[/code]

## Развертывание СУБД (PostgreSQL)

Базу данных нужно развернуть как отдельное приложение, а затем можно будет подключаться к ней из основного приложения. Подробная инструкция доступна по [ссылке](<databases/postgreSQL.md>).

## Создание проекта в Amvera

Последний шаг - развернуть само приложение. В файле _main.go_ содержится основной код и выполняется подключение к базе данных. Не забудьте поменять параметры для подключения к базе данных на те, которые вы использовали в прошлом шаге при создании базы данных на Amvera:
* **user** = Имя пользователя
* **password** = Пароль пользователя
* **dbname** = Имя создаваемой БД
* параметр **host** можно найти на странице _Инфо_ вашего PostgreSQL проекта (например, amvera-username-cnpg-appname-rw)

**main.go:**
[code] 
    ```
    package main
    
    import (
    	"database/sql"
    	"encoding/json"
    	"log"
    	"net/http"
    	"strconv"
    
    	_ "github.com/lib/pq"
    )
    
    // Укажите те значения, которые задавали при создании БД на Amvera Cloud
    const (
    	host     = "amvera-nskripko-cnpg-godb-rw"
    	port     = 5432
    	user     = "nick"
    	password = "href239"
    	dbname   = "godb"
    )
    
    func main() {
    	portStr := strconv.Itoa(port)
    	dbinfo := "host=" + host + " port=" + portStr + " user=" + user + " password=" + password + " dbname=" + dbname + " sslmode=disable"
    
    	db, err := sql.Open("postgres", dbinfo)
    	if err != nil {
    		log.Fatal(err)
    	}
    	defer db.Close()
    
    	_, err = db.Exec(`CREATE TABLE IF NOT EXISTS quotes (
                            id SERIAL PRIMARY KEY,
                            quote TEXT NOT NULL
                        )`)
    	if err != nil {
    		log.Fatal(err)
    	}
    	
    	http.HandleFunc("/quotes", func(w http.ResponseWriter, r *http.Request) {
    		if r.Method != http.MethodGet {
    			http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
    			return
    		}
    
    		rows, err := db.Query("SELECT quote FROM quotes")
    		if err != nil {
    			log.Println("Error querying database:", err)
    			http.Error(w, "Internal server error", http.StatusInternalServerError)
    			return
    		}
    		defer rows.Close()
    
    		var quotes []string
    		for rows.Next() {
    			var quote string
    			if err := rows.Scan(&quote); err != nil {
    				log.Println("Error scanning rows:", err)
    				continue
    			}
    			quotes = append(quotes, quote)
    		}
    
    		json.NewEncoder(w).Encode(quotes)
    	})
    
    	http.HandleFunc("/addquote", func(w http.ResponseWriter, r *http.Request) {
    		if r.Method != http.MethodPost {
    			http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
    			return
    		}
    
    		var data struct {
    			Quote string `json:"quote"`
    		}
    		if err := json.NewDecoder(r.Body).Decode(&data); err != nil {
    			http.Error(w, "Bad request", http.StatusBadRequest)
    			return
    		}
    
    		_, err := db.Exec("INSERT INTO quotes (quote) VALUES ($1)", data.Quote)
    		if err != nil {
    			log.Println("Error inserting quote into database:", err)
    			http.Error(w, "Internal server error", http.StatusInternalServerError)
    			return
    		}
    
    		w.WriteHeader(http.StatusCreated)
    	})
    
    	fs := http.FileServer(http.Dir("static"))
    	http.Handle("/", fs)
    
    	log.Fatal(http.ListenAndServe(":80", nil))
    }
    
    ```
    
[/code]

> :warning: Код является демонстрационным примером и мы настоятельно не рекомендуем указывать логин и пароль для подключения к базе данных в коде. Используйте переменные окружения (секреты)!

## Проверка работоспособности
1. Переходим в настройки проекта и активируем доменное имя.
2. Теперь можно перейти по данному URL и откроется наше приложение.

Если что-то не работает, рекомендуем ознакомиться с логами Сборки и Приложения.

Поздравляем, вы успешно создали свое первое приложение в Amvera!

## Код статических файлов из примера

**static/index.html:**
[code] 
    ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Quote Board</title>
        <link rel="stylesheet" href="styles.css">
    </head>
    <body>
        <div class="container">
            <h1>Quote Board</h1>
            <form id="quoteForm">
                <input type="text" id="quoteInput" placeholder="Enter your quote" required>
                <button type="submit">Submit</button>
            </form>
            <div id="quoteList"></div>
        </div>
        <script src="script.js"></script>
    </body>
    </html>
    
    ```
    
[/code]

**static/styles.css:**
[code] 
    ```
    body {
        font-family: Arial, sans-serif;
    }
    
    .container {
        max-width: 600px;
        margin: 50px auto;
        padding: 0 20px;
    }
    
    input[type="text"] {
        width: calc(100% - 80px);
        padding: 10px;
        margin-right: 10px;
    }
    
    button {
        padding: 10px 20px;
        background-color: #007bff;
        color: #fff;
        border: none;
        cursor: pointer;
    }
    
    button:hover {
        background-color: #0056b3;
    }
    
    #quoteList {
        margin-top: 20px;
    }
    
    ```
    
[/code]

**static/script.js:**
[code] 
    ```
    document.addEventListener('DOMContentLoaded', () => {
        const quoteForm = document.getElementById('quoteForm');
        const quoteInput = document.getElementById('quoteInput');
        const quoteList = document.getElementById('quoteList');
    
        // Function to fetch quotes from the server and display them
        const fetchQuotes = async () => {
            try {
                const response = await fetch('/quotes');
                const quotes = await response.json();
    
                // Clear previous quotes
                quoteList.innerHTML = '';
    
                // Append new quotes to the list
                quotes.forEach(quote => {
                    const quoteItem = document.createElement('div');
                    quoteItem.textContent = quote;
                    quoteList.appendChild(quoteItem);
                });
            } catch (error) {
                console.error('Error fetching quotes:', error);
            }
        };
    
        // Fetch initial quotes when the page loads
        fetchQuotes();
    
        // Submit quote form
        quoteForm.addEventListener('submit', async event => {
            event.preventDefault();
            const newQuote = quoteInput.value.trim();
    
            if (newQuote === '') {
                alert('Please enter a quote.');
                return;
            }
    
            try {
                // Send the new quote to the server
                await fetch('/addquote', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({ quote: newQuote })
                });
    
                // Clear the input field
                quoteInput.value = '';
    
                // Fetch and display updated quotes
                fetchQuotes();
            } catch (error) {
                console.error('Error adding quote:', error);
            }
        });
    });
    
    ```
    
[/code]

> **⚠️ Предупреждение** > > Важно Сохраняйте файлы БД и иные изменяемые данные в постоянное хранилище, чтобы избежать их потери при обновлении проекта, когда производится «откат» папки код до состояния обновления репозитория. Папка data в корне проекта и директория /data, это разные директории. Проверить, что сохранение идет в /data, можно зайдя в папку «data» на странице «Репозиторий». 

> **⚠️ Предупреждение** > > Важно Чтобы избежать ошибки 502, измените в вашем коде host 127.0.0.1 (или подобный localhost) на 0.0.0.0, и пропишите в конфигурации порт, который слушает ваше приложение (пример - 8080). 

## Если у вас не получается развернуть проект

Напишите наблюдаемую вами симптоматику на support@amvera.ru с указанием вашего имени пользователя и названия проекта, мы постараемся вам помочь.

[ Next Fullstack на Go ](<go_full.html>) [ Previous Spring Boot с встраиваемой базой данных H2 и PostgreSQL ](<java-springboot.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Spring Boot с встраиваемой базой данных H2 и PostgreSQL](https://docs.amvera.ru/java-springboot.html)

→ [Fullstack на Go](https://docs.amvera.ru/go_full.html)
