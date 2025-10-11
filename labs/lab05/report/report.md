---
## Front matter
title: "Лабораторная работа № 5"
subtitle: "Отчёт"
author: "Ермишина Мария Кирилловна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Целью данной лабораторной работы является получение навыков управления системными службами операционной системы посредством systemd.

# Выполнение лабораторной работы

1. Управление сервисами
Для начала получите права администратора. После проверьте статус службы Very Secure FTP: (рис. [-@fig:001])
  - systemctl status vsftpd

Установите службу Very Secure FTP: (рис. [-@fig:001])
  - dnf -y install vsftpd
  
Запустите службу Very Secure FTP и проверьте ее статус:
  - systemctl start vsftpd
  - systemctl status vsftpd
  
![Служба VSFTP](image/image1.jpg){#fig:001 width=70%}
 
Добавьте службу Very Secure FTP в автозапуск при загрузке операционной системы, используя команду systemctl enable. Удалите службу из автозапуска, используя команду systemctl disable, в обоих случаях проверяем статус. (рис. [-@fig:002]) 

Выведите на экран символические ссылки, ответственные за запуск различных сервисов:
  - ls /etc/systemd/system/multi-user.target.wants
  
Снова добавьте службу Very Secure FTP в автозапуск:
  - systemctl enable vsftpd
  
Снова проверьте статус службы Very Secure FTP:
  - systemctl status vsftpd
  
Выведите на экран список зависимостей юнита:
  - systemctl list-dependencies vsftpd
  
Выведите на экран список юнитов, которые зависят от данного юнита:
  - systemctl list-dependencies vsftpd --reverse
  
![Автозапуск](image/image2.jpg){#fig:002 width=70%}

Получите полномочия администратора. Установите iptables: 
  - dnf -y install iptables\*

Проверьте статус firewalld и iptables: 
  - systemctl status firewalld
  - systemctl status iptables
  
Попробуйте запустить firewalld и iptables:
  - systemctl start firewalld
  - systemctl start iptables

Введите для проверки настроек юнита: (рис. [-@fig:003])
  - cat /usr/lib/systemd/system/firewalld.service
  - cat /usr/lib/systemd/system/iptables.service

![Настройки юнитов](image/image3.jpg){#fig:003 width=70%}

Выгрузите службу iptables ( - systemctl stop iptables), и загрузите службу firewalld ( - systemctl start firewalld) (рис. [-@fig:004])
Заблокируйте запуск iptables, введя:
  - systemctl mask iptables
  
Попробуйте запустить iptables:
  - systemctl start iptables
  
Попробуйте добавить iptables в автозапуск:
  - systemctl enable iptables

![iptables](image/image4.jpg){#fig:004 width=70%}

2. Изолируемые цели 
Чтобы получить список всех активных загруженных целей, введите: (рис. [-@fig:004])
  - systemctl --type=target
  
Чтобы получить список всех целей, введите: (рис. [-@fig:004])
  - systemctl --type=target --all

Получите полномочия администратора. Перейдите в каталог systemd и найдите список всех целей, которые можно изолировать: (рис. [-@fig:005])
  - cd /usr/lib/systemd/system
  - grep Isolate *.target

Переключите операционную систему в режим восстановления:
  - systemctl isolate rescue.target
  
Перезапустите операционную систему следующим образом:
  - systemctl isolate reboot.target

![Настройки ОС](image/image5.jpg){#fig:005 width=70%}

3. Цель по умолчанию

Получите полномочия администратора. Выведите на экран цель, установленную по умолчанию:
  - systemctl get-default
  
Для запуска по умолчанию текстового режима введите
  - systemctl set-default multi-user.target
  
Перегрузите систему командой reboot. Убедитесь, что система загрузилась в текстовом режиме. Получите полномочия администратора. Для запуска по умолчанию графического режима введите
  - systemctl set-default graphical.target
  
Вновь перегрузите систему командой reboot. Убедитесь, что система загрузилась в графическом режиме.

![Изменения виды ОС](image/image6.jpg){#fig:006 width=70%}

# Контрольные вопросы
1. Unit – объект, которым может управлять система
2. systemctl is-enable “имя_юнита” (пример: systemctl is-enable vsftpd.service)
3. system list-units
4. Нужно внести всю необходимую информацию в переменную “Wants”, которая находится в файле имя_сервиса.service
5. systemctl set-default rescue.target
6. Изолируя цель, мы запускаем эту цель со всеми её зависимостями. Не все цели могут быть изолированы (в случае, если цель является неотъемлемой частью system)
7. systemctl list-dependencies “имя_юнита” --reverse (пример: systemctl list-dependencies firewalld.service --reverse)

# Выводы

В ходе данной лабораторной работы получены навыков управления системными службами операционной системы посредством systemd.

# Список литературы{.unnumbered}

::: {#refs}
:::
