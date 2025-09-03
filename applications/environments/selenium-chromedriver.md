---
title: Selenium и Chromedriver¶
url: https://docs.amvera.ru/applications/environments/selenium-chromedriver.html
path: applications/environments/selenium-chromedriver
prev: ffmpeg
next: Перенос проекта из Heroku
---

# Selenium и Chromedriver¶

## Содержание

- Selenium и Chromedriver
- Использование готового образа
- Создание своего образа с ChromeDriver
- Пример
- Видеопример депдлоя Python-приложения с Selenium

---

Back to top

[ View this page ](<../../_sources/applications/environments/selenium-chromedriver.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Selenium и Chromedriver

Облако Амвера использует под капотом контейнеры. Чтобы было возможно использовать selenium в контейнере, надо поставить необходимые библиотеки и указать порт дисплея в этом контейнере.

## Использование готового образа

В [этом образе](<https://github.com/joyzoursky/docker-python-chromedriver>) уже имеется нужный нам ChromeDriver и Chrome

**Шаги установки** :
1. Создаем Dockerfile в директории проекта.
2. В Dockerfile указываем образ c
[code] ```
         FROM joyzoursky/python-chromedriver:3.9
         
         ```
         
[/code]
3. Назначаем WORKDIR и указываем файлы приложения, которые будут включены в итоговый образ (все файлы в папке проекта):
[code] ```
         WORKDIR  /app
         COPY  .  /app
         
         ```
         
[/code]
4. Добавляем команду на установку зависимостей _(укажем их чуть позже)_ :
[code] ```
         RUN  pip  install  -r  requirements.txt
         
         ```
         
[/code]
5. Задаём команду для запуска приложения:
[code] ```
         CMD ["python", "main.py"]
         
         ```
         
[/code]

**Получившийся Dockerfile:**
[code] 
    ```
    FROM  joyzoursky/python-chromedriver:3.9
    WORKDIR  /app
    COPY  .  /app
    RUN  pip  install  -r  requirements.txt
    CMD  ["python",  "main.py"]
    
    ```
    
[/code]

Примечание

Вместо версии 3.9 можно указать другую версию Python, которая вам нужна и поддерживается автором образа.

## Создание своего образа с ChromeDriver

В случае, если требуемая версия Python не поддерживается, можно создать свой образ с нужной версией, установив в него необходимые драйвера.

В приведенном примере за базовый образ взят [образ python:3.8](<https://hub.docker.com/_/python>) который можно заменить на подходящий.
[code] 
    ```
    # задаем базовый образ (вместо 3.8 можно указать другую версию Python)
    FROM python:3.8
    
    # устанавливаем google-chrome
    RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
    RUN sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
    RUN apt-get -y update
    RUN apt-get install -y google-chrome-stable
    
    # выставляем нужный порт дисплея
    ENV DISPLAY=:99
    
    ```
    
[/code]

## Пример

Пример будет выводить Title страницы www.python.org

**Добавим в корень проекта следующеие файлы:**
* requirements.txt
[code] ```
        selenium==4.17.2
        
        ```
        
[/code]
* amvera.yml
[code] ```
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
          containerPort: 80
        
        ```
        
[/code]
* Dockerfile
[code] ```
        FROM  joyzoursky/python-chromedriver:3.9
        WORKDIR  /app
        COPY  .  /app
        RUN  pip  install  -r  requirements.txt
        CMD  ["python",  "main.py"]
        
        ```
        
[/code]
* main.py
[code] ```
        from selenium import webdriver
        from selenium.webdriver.common.keys import Keys
        from selenium.webdriver.chrome.options import Options
        
        chrome_options = Options()
        chrome_options.add_argument('--headless')
        chrome_options.add_argument('--no-sandbox')
        chrome_options.add_argument('--disable-dev-shm-usage')
        
        driver = webdriver.Chrome(options=chrome_options)
        driver.get("https://www.python.org")
        print(driver.title)
        
        driver.close()
        
        ```
        
[/code]

В случае правильной работы программы в консоль выведется текст: **Welcome to Python.org**

## Видеопример депдлоя Python-приложения с Selenium

Видео в VK Video для просмотра без VPN доступно по [ссылке](<https://vkvideo.ru/video-167699755_456239037>).

Видео в YouTube:

[ Next Перенос проекта из Heroku ](../configuration/heroku-migration.md) [ Previous ffmpeg ](ffmpeg-pip.md)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [ffmpeg](ffmpeg-pip.md)

→ [Перенос проекта из Heroku](configuration/heroku-migration.md)
