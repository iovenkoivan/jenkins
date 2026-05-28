# Звіт про виконану роботу з Jenkins

## Мета роботи

Виконати налаштування Jenkins та реалізувати CI/CD процес між двома машинами:

1. Запустити Jenkins на окремій машині.
2. Налаштувати SSH-обмін ключами між Jenkins-сервером та сервером з вебсервісом.
3. Створити pipeline у Jenkins для автоматичного оновлення контенту на вебсервері.
4. Створити другий pipeline зі stage-ами для демонстрації роботи Jenkins Pipeline.

---

## 1. Запуск Jenkins

Було перевірено стан служби Jenkins через `systemctl status jenkins`.

![Jenkins service](<Screenshots/Screenshot 2026-05-27 214828.png>)

На скріншоті видно:

* служба `jenkins.service` активна;
* Jenkins успішно запущений;
* сервіс працює на Java.

---

## Відкриття вебінтерфейсу Jenkins

Після запуску Jenkins вебінтерфейс став доступний через браузер за адресою:

```bash
http://10.0.2.15:8080
```

![Jenkins UI](<Screenshots/Screenshot 2026-05-27 215025.png>)

---

## 2. Налаштування SSH-обміну ключами

Для організації безпарольного підключення між Jenkins та сервером вебсервера було створено SSH-ключі.

Команда:

```bash
ssh-keygen
```

![SSH key generation](<Screenshots/Screenshot 2026-05-27 215111.png>)

Після генерації ключі були передані на сервер з вебсервісом.

---

## Перевірка SSH-підключення

Було виконано SSH-підключення до другої машини.

![SSH connection](<Screenshots/Screenshot 2026-05-27 215704.png>)

Підключення виконано успішно.

---

## Налаштування прав для вебкаталогу

Для можливості деплою Jenkins отримав доступ до каталогу вебсервера `/var/www`.

Використані команди:

```bash
sudo chgrp -R strk /var/www
sudo chmod -R g+rw /var/www
```

---

## 3. Створення pipeline для деплою контенту

Було створено Jenkins Job для автоматичного оновлення контенту вебсервера.

У Build Steps використано shell-команди:

```bash
echo "Go go go"
echo "<html><body><h1>Hello World</h1></body></html>" > index.html
```

![Build step](<Screenshots/Screenshot 2026-05-27 215228.png>)

---

## Перевірка виконання Job

Після запуску Job Jenkins успішно виконав build.

![Console output](<Screenshots/Screenshot 2026-05-27 215321.png>)

У консолі видно:

* виконання shell-команд;
* створення файлу `index.html`;
* статус `SUCCESS`.

---

## Встановлення Publish Over SSH

Для передачі контенту на вебсервер було встановлено плагін:

* `Publish Over SSH`

![Plugin installation](<Screenshots/Screenshot 2026-05-27 215453.png>)

---

## Результат деплою

Після запуску pipeline Jenkins виконав передачу файлу на сервер через SSH.

![Deploy success](<Screenshots/Screenshot 2026-05-27 215743.png>)

У логах видно:

* SSH-підключення;
* передачу файлів;
* успішне завершення build.

---

## Перевірка роботи вебсервера

Після деплою сторінка стала доступною через браузер.

![Hello World page](<Screenshots/Screenshot 2026-05-27 215807.png>)

На сторінці відображається:

```html
Hello World
```

---

## 4. Створення pipeline зі stage-ами

Було створено другий Jenkins Pipeline для демонстрації stage-based pipeline.

Pipeline складався зі stage-ів:

* Clone
* Build
* Test
* Deploy
* Post Actions

---

## Відображення проходження stage-ів

![Pipeline stages](<Screenshots/Screenshot 2026-05-27 221016.png>)

На графіку видно:

* послідовне проходження всіх stage-ів;
* успішне завершення pipeline;
* статус `Pipeline finished successfully`.

---

## Висновок

У ході роботи було:

* встановлено та запущено Jenkins;
* налаштовано SSH-обмін ключами між двома машинами;
* реалізовано автоматичний деплой контенту на вебсервер;
* встановлено та налаштовано Publish Over SSH;
* створено Jenkins Pipeline зі stage-ами;
* перевірено успішне проходження всіх етапів CI/CD процесу.

Отримано практичні навички роботи з Jenkins, SSH та автоматизацією процесу деплою.
