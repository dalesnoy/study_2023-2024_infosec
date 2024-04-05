---
## Front matter
lang: ru-RU
title: Индивидуальный проект №3
subtitle: Основы информационной безопасности
author:
  - Барсегян В.Л.
institute:
  - Российский университет дружбы народов им. Патриса Лумумбы, Москва, Россия

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'

## Fonts
mainfont: Arial
romanfont: Arial
sansfont: Arial
monofont: Arial
---

## Докладчик


  * Барсегян Вардан Левонович
  * НПИбд-01-22
  * Российский университет дружбы народов
  * [1132222005@pfur.ru]
  * <https://github.com/VARdamn/study_2023-2024_infosec/tree/master/project-personal>
  
# Вводная часть

## Цели и задачи

Знакомство с Hydra для подбора или взлома имени пользователя и пароля.

# Выполнение лабораторной работы

## Открываю в браузере страницу http://127.0.0.1/DVWA/login.php и вхожу в DVWA

![Вход в DVWA](image/1.png){ #fig:001 width=70% }

## Открываю страницу http://127.0.0.1/DVWA/security.php и выставляю уровень безопасности на низкий

![Низкий уровень безопасности](image/2.png){ #fig:002 width=70% }

## Создаю файл с паролями, в него ввожу самые распространенные пароли 

![Файл с паролями](image/3.png){ #fig:003 width=70% }

## Перехожу во вкладку Brute Force, где можно подобрать комбинацию логина и пароля и проверить, верна ли она. Во вкладке Network консоли разработчика смотрю запрос для валидации логина и пароля - это GET-запрос, отправляющий логин, пароль в качестве параметров 

![Запрос для проверки комбинации](image/4.png){ #fig:004 width=70% }

## Куки GET-запроса - уровень безопасности и id сессии 

![Куки запроса](image/5.png){ #fig:005 width=70% }

## Ввожу команду для hydra:

'''
hydra -l admin -P ~/passwords.txt 127.0.0.1 http-get-form "/DVWA/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:H=Cookie\:PHPSESSID=1pu2bih9masffp4i16iap5dglc;security=low:F=Username and/or password incorrect"
'''

ключ -l –	логин для входа
ключ -P –	пароль для входа, берутся все возможные из файла ~/passwords.txt
http-get-form – тип запроса (GET)
дополнительный параметр (длинная строка) - полный путь, параметры, куки, и сообщение при ошибке

## В результате, hydra подобрала верную комбинацию: логин admin и пароль password

![Команда для подбора пароля](image/6.png){ #fig:006 width=70% }

## Создаю файл с возможными логинами

![Файл с логинами](image/7.png){ #fig:007 width=70% }

## Изменяю команду для hydra: указываю ключ -L ~/usernames.txt, чтобы логины ткаже перебирались из файла 

'''
hydra -L ~/usernames.txt -P ~/passwords.txt 127.0.0.1 http-get-form "/DVWA/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:H=Cookie\:PHPSESSID=1pu2bih9masffp4i16iap5dglc;security=low:F=Username and/or password incorrect"
'''

## Измененная команда

![Измененная команда](image/8.png){ #fig:008 width=70% }

## Вывод

Я познакомился с hydra, научился подбирать логины и пароли с помощью нее, отправляя запросы.
