---
title: ffmpeg¶
url: https://docs.amvera.ru/applications/environments/ffmpeg-pip.html
path: applications/environments/ffmpeg-pip
prev: Ruby (Rails)
next: Selenium и Chromedriver
---

# ffmpeg¶

## Содержание

- ffmpeg
- Установка через Dockerfile
- Пример
- Возможные ошибки
- Видеопример - Деплой приложения с FFmpeg

---

Back to top

[ View this page ](<../../_sources/applications/environments/ffmpeg-pip.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# ffmpeg

Для развертывания приложения с использованием ffmpeg (в нашем случае Python), нам нужно установить этот набор программ в сам контейнер, а не только библиотеку для python, которая ставится через менеджер пакетов pip.

## Установка через Dockerfile

Используем Docker для установки FFMpeg.

**Шаги:**
1. Создаем Dockerfile в директории с проектом.
2. В Dockerfile указываем базовый образ Python
[code] ```
         FROM python:3.8-slim
         
         ```
         
[/code]
3. Выполняем команду установки FFMpeg:
[code] ```
         RUN apt-get update && apt-get install -y ffmpeg
         
         ```
         
[/code]
4. Копируем проект в собираемый образ и назначаем WORKDIR:
[code] ```
         COPY . /usr/src/app
         WORKDIR /usr/src/app
         
         ```
         
[/code]
5. Добавляем команду для запуска приложения:
[code] ```
         CMD ["python", "main.py"]
         
         ```
         
[/code]

**Получившийся Dockerfile:**
[code] 
    ```
    FROM python:3.9
    
    RUN apt-get update && apt-get install -y ffmpeg
    
    COPY . /usr/src/app
    WORKDIR /usr/src/app
    RUN pip install -r requirements.txt
    
    COPY . /usr/src/app
    
    CMD ["python", "bot.py"]
    
    ```
    
[/code]

Созданный файл необходимо поместить в корень проекта.

Примечание
* Вместо 3.8-slim вы можете указать любую другую версию Python, которая вам нужна.
* Если вы используете другой язык программирования, вам нужно будет изменить команду установки FFMpeg в соответствии с вашим языком.

**Примечание:**
* Вместо ``3.8-slim`` возможно указать другую версию Python, которая требуется приложению.
* Если используется другой язык программирования, необходимо выбрать другой базовый образ с нужным языком и установить библиотеку для этого языка.

## Пример

Создадим telegram бота на python с использованием библиотек telebot и ffmpeg-python, который будет принимать пользовательские видео и добавлять туда watermark’и.

> **⚠️ Предупреждение** > > Примечание Код является демонстрационным примером и мы настоятельно не рекомендуем хранить токены ботов или подобные данные в коде. Используйте переменные окружения (секреты)! 
[code] 
    ```
    import os
    
    import telebot
    import ffmpeg
    
    bot = telebot.TeleBot('YOUR_BOT_TOKEN')
    
    @bot.message_handler(commands=['start'])
    def handle_start(message):
        bot.send_message(message.chat.id, "Привет! Отправь мне видео, и я добавлю водяной знак с помощью Ffmpeg.")
    
    @bot.message_handler(content_types=['video'])
    def handle_video(message):
        video = message.video
        file_info = bot.get_file(video.file_id)
        downloaded_file = bot.download_file(file_info.file_path)
    
        with open('video.mp4', 'wb') as new_file:
            new_file.write(downloaded_file)
    
        in_file = ffmpeg.input('video.mp4')
        overlay_file = ffmpeg.input('watermark.png')
        (
            ffmpeg
            .concat(
                in_file.trim(start_frame=10, end_frame=20),
                in_file.trim(start_frame=30, end_frame=40),
            )
            .overlay(overlay_file.hflip())
            .drawbox(50, 50, 120, 120, color='red', thickness=5)
            .output('out.mp4')
            .run()
        )
    
        with open('out.mp4', 'rb') as video:
            bot.send_video(message.chat.id, video)
    
        os.remove('video.mp4')
        os.remove('out.mp4')  
    
    bot.polling()
    
    ```
    
[/code]

**Добавим в корень проекта следующие файлы:**
* конфигурационный yaml-файл:
[code] ```
        meta:
          environment: docker
          toolchain:
            name: docker
            version: latest
        build:
          dockerfile: Dockerfile
        run:
          persistenceMount: /data
          containerPort: 80
        
        ```
        
[/code]
* Dockerfile:
[code] ```
        FROM python:3.9
        
        RUN apt-get update && apt-get install -y ffmpeg
        
        COPY . /usr/src/app
        WORKDIR /usr/src/app
        RUN pip install -r requirements.txt
        
        COPY . /usr/src/app
        
        CMD ["python", "bot.py"]
        
        ```
        
[/code]
* Файл с зависимостями requirements.txt. В нашем случае это pyTelegramBotAPI (официальное название telebot) и ffmpeg-python.
[code] ```
        pyTelegramBotAPI==4.15.4
        ffmpeg-python==0.2.0
        
        ```
        
[/code]

## Возможные ошибки
* Python не находит файл .py – проверьте, изменили ли вы название **main.py** в параметре ``CMD`` **Dockerfile** ’а или в параметре ``command`` в **amvera.yml** на название вашего скрипта или путь до него.
* ``AttributeError: module 'ffmpeg' has no attribute 'input'`` \- проверьте, что вы установили **ffmpeg-python** , а не **python-ffmpeg**.

## Видеопример - Деплой приложения с FFmpeg

Описание: В этом видео подробно рассматривается процесс развертывания приложения при помощи Dockerfile на Amvera Cloud.

Видео в VK Video для просмотра без VPN доступно по [ссылке](<https://vkvideo.ru/video-167699755_456239040>).

Видео в YouTube:
* 00:00 Интро
* 00:29 Dockerfile
* 01:57 bot.py
* 03:21 requirements.txt
* 05:55 amvera.yaml
* 06:52 Git
* 08:55 Переменные окружения
* 09:17 Структура проекта (/data)
* 09:58 Тестируем бота

[ Next Selenium и Chromedriver ](<selenium-chromedriver.html>) [ Previous Ruby (Rails) ](<ruby-bundle.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Ruby (Rails)](ruby-bundle.md)

→ [Selenium и Chromedriver](selenium-chromedriver.md)
