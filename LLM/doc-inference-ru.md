---
title: Amvera LLM Inference¶
url: https://docs.amvera.ru/LLM/doc-inference-ru.html
path: LLM/doc-inference-ru
prev: WordPress
next: Пополнение счета
---

# Amvera LLM Inference¶

## Содержание

- Amvera LLM Inference
- Аутентификация
- API
  - Доступные варианты инференса
  - Конечная точка модели
  - Пример запроса/ответа:

---

Back to top

[ View this page ](<../_sources/LLM/doc-inference-ru.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Amvera LLM Inference

Общая доступность LLM планируется 15 июля 2025 г.

Мы предоставляем доступ к нескольким большим языковым моделям через единый публичный API.

## Аутентификация

Чтобы использовать Amvera LLM Inference API, необходимо пройти аутентификацию:

Для этого в ЛК, нужно зайти в любую модель в разделе LLM, и скопировать токен доступа. Затем, при обращении к моделе, укажите токен доступа в заголовке ``X-Auth-Token`` в следующем формате: [``X-Auth-Token: Bearer <access_token>``]

## API

### Доступные варианты инференса

/llama
1. **``llama8b``** — модель с 8 миллиардами параметров, оптимизированная для эффективной обработки текста.
2. **``llama70b``** — модель с 70 миллиардами параметров, обеспечивающая более высокое качество генерации и обработки текста.

### Конечная точка модели

Для отправки запросов к моделям используйте следующий формат запроса к конечной точке: ``POST /models/<inference_name>``. Например: ``POST /models/llama``

Эта конечная точка позволяет отправлять запросы к выбранной модели. В зависимости от ваших нужд, в теле запроса нужно выбрать одну из доступных моделей (``llama8b`` или ``llama70b``).

### Пример запроса/ответа:

Request:
[code] 
    ```
    {
      "model": "llama8b", 
      "messages": [
        {
          "role": "user",
          "text": "Hi, how are you?"
        }
      ]
    }
    
    ```
    
[/code]

[ Next Пополнение счета ](<../general/topup.html>) [ Previous WordPress ](<../marketplace/wordpress.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [WordPress](marketplace/wordpress.md)

→ [Пополнение счета](general/topup.md)
