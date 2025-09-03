# 📚 Amvera Cloud Documentation

> Полная документация по платформе Amvera Cloud для развертывания и управления приложениями.

[![Версия документации](https://img.shields.io/badge/docs-2025--09--04-blue.svg)](https://github.com/your-username/amvera-docs)
[![Статус парсинга](https://img.shields.io/badge/parsed-74%20pages-green.svg)](parsing_report.txt)

## 🌟 О документации

Эта документация была извлечена из официального сайта [Amvera Cloud](https://amvera.ru/) с помощью автоматизированного парсера. Она содержит актуальную информацию о развертывании, конфигурации и управлении приложениями на платформе Amvera.

**Дата последнего обновления:** 2025-09-04

## 🚀 Быстрый старт

1. **Начните с основ:** [Руководство Amvera Cloud](index.md)
2. **Выберите ваше окружение:** [Поддерживаемые среды разработки](#-поддерживаемые-среды-разработки)
3. **Настройте развертывание:** [Конфигурация приложений](#-конфигурация-приложений)

## 📁 Структура документации

### 🔧 Приложения (`applications/`)
- **[Создание и развертывание](applications/build.md)** - Процесс сборки и деплоя
- **[Резервное копирование](applications/backups.md)** - Управление бэкапами
- **[Конфигурация](applications/configuration.md)** - Настройка приложений
- **[Git интеграция](applications/git/)** - Работа с репозиториями

### 🗄️ Базы данных (`databases/`)
- **PostgreSQL, MySQL, MongoDB** - Настройка и управление
- **Резервное копирование БД** - Стратегии бэкапа
- **Миграции** - Работа с изменениями схемы

### ⚙️ Общие настройки (`general/`)
- **Домены и SSL** - Настройка доменов
- **Логирование** - Мониторинг приложений
- **Безопасность** - Best practices

### 🏪 Маркетплейс (`marketplace/`)
- **Готовые шаблоны** - Быстрый старт с шаблонами
- **Интеграции** - Подключение сервисов

## 🛠️ Поддерживаемые среды разработки

| Язык/Фреймворк | Руководство | Конфигурация |
|----------------|-------------|--------------|
| **Node.js** | [Server](applications/environments/nodejs-server.md) \| [Browser](applications/environments/nodejs-browser.md) | Package.json |
| **Python** | [Python + pip](applications/environments/python-pip.md) | requirements.txt |
| **Java** | [Maven](applications/environments/jvm-maven.md) \| [Gradle](applications/environments/jvm-gradle.md) | pom.xml / build.gradle |
| **C#** | [.NET](applications/environments/csharp-dotnet.md) \| [Mono](applications/environments/csharp-mono.md) | *.csproj |
| **Go** | [Golang](applications/environments/golang-go.md) | go.mod |
| **Ruby** | [Ruby + Bundle](applications/environments/ruby-bundle.md) | Gemfile |

## 🔧 Конфигурация приложений

- **[Переменные окружения](applications/configuration/variables.md)** - Настройка env переменных
- **[Сетевые настройки](applications/configuration/network.md)** - Порты и соединения
- **[Docker конфигурация](applications/configuration/docker.md)** - Кастомизация контейнеров
- **[Файлы конфигурации](applications/configuration/config-file.md)** - amvera.yml и другие

## 📊 Статистика парсинга

- 📄 **Всего страниц:** 74
- ✅ **Успешно обработано:** 74  
- ❌ **Ошибок:** 3
- 🖼️ **Изображений:** 100
- 📁 **Разделов:** 6

## 🖼️ Медиафайлы

Все изображения сохранены локально в папке `images/`. Ссылки в документации автоматически обновлены для работы без интернета.

## 🔍 Поиск по документации

Для быстрого поиска используйте:
```bash
# Поиск по содержимому файлов
grep -r "ваш запрос" . --include="*.md"

# Поиск файлов по названию
find . -name "*keyword*.md"
```

## 📝 Известные ограничения

Следующие страницы не удалось обработать при парсинге:
- Docker конфигурация (решение проблем)
- Конфигурационные ошибки  
- Node.js сервер (дополнительные настройки)

Подробности в файле [parsing_report.txt](parsing_report.txt).

## 🤝 Вклад в документацию

1. Создайте issue для предложения улучшений
2. Отправьте pull request с исправлениями
3. Обновите документацию при изменениях в Amvera Cloud

## 📞 Поддержка

- 🌐 **Официальный сайт:** [amvera.ru](https://amvera.ru)
- 📖 **Официальная документация:** [docs.amvera.ru](https://docs.amvera.ru)
- 💬 **Техподдержка:** Через панель управления Amvera

## ⚖️ Лицензия

Документация предоставляется для ознакомления. Права на содержание принадлежат Amvera Cloud.

---

*📅 Создано автоматическим парсером • 2025-09-04 01:23:16*  
*🔄 Структурировано для удобства использования*
