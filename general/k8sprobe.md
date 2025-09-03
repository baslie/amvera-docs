---
title: Поддержка проб Kubernetes¶
url: https://docs.amvera.ru/general/k8sprobe.html
path: general/k8sprobe
prev: Уведомления о сбоях
next: CLI. Управление через командную строку
---

# Поддержка проб Kubernetes¶

## Содержание

- Поддержка проб Kubernetes
- Как настраивать?
- Пример
- На основе чего срабатывает уведомление

---

Back to top

[ View this page ](<../_sources/general/k8sprobe.md.txt> "View this page")

Toggle Light / Dark / Auto color theme

Toggle table of contents sidebar

__

# Поддержка проб Kubernetes

## Как настраивать?

В форму нужно заполнять настройки в формате yaml, нативно формату, который используется самим k8s(см. [здесь](<https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/>)). Конфигурационный файл, подставляется в ваш Deployment, который загружается с ним в кластер. **Это означает, что если вы загрузили неработающую настройку, или в неправильном формате, ваш проект упадет на этапе сборки.**

## Пример
[code] 
    ```
    livenessProbe:
          tcpSocket:
            port: 8080
          initialDelaySeconds: 15
          periodSeconds: 10
    
    ```
    
[/code]

Внимание! То что указано в примере, необязательно будет работать на вашем проекте.

## На основе чего срабатывает уведомление

Пробы k8s не предполагают функционала для уведомления по какому-либо критерию, но наша система принимает решение на основе ивентов Kubernetes. Уведомление отправляется в результате получения ивента с информацией о негативной пробе. Например, если readiness probe оказалась негативной, Kubernetes дает ивент с сообщением, которое начинается «Readiness probe failed…»

[ Next CLI. Управление через командную строку ](<cli.html>) [ Previous Уведомления о сбоях ](<notifications.html>)

Copyright © 2024, Amvera 

Made with [Sphinx](<https://www.sphinx-doc.org/>) and [@pradyunsg](<https://pradyunsg.me>)'s [Furo](<https://github.com/pradyunsg/furo>)


---

### Навигация

← [Уведомления о сбоях](notifications.md)

→ [CLI. Управление через командную строку](cli.md)
