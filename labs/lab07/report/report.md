---
## Front matter
title: "Лабораторная работа № 7"
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

Целью данной лабораторной работы является получение навыков работы с журналами мониторинга различных событий в системе.

# Выполнение лабораторной работы

1. Мониторинг журнала системных событий в реальном времени
Запустите три вкладки терминала и в каждом из них получите полномочия админи-
стратора. 
На второй вкладке терминала запустите мониторинг системных событий в реальном времени: (рис. [-@fig:001])
  - tail -f /var/log/messages
В третьей вкладке терминала вернитесь к учётной записи своего пользователя и попробуйте получить полномочия администратора, но введите неправильный пароль. Обратите внимание, что во второй вкладке терминала с мониторингом событий или ничего не отобразится, или появится сообщение «FAILED SU (to root) username ...». (рис. [-@fig:001])
В третьей вкладке терминала из оболочки пользователя введите logger hello - во второй вкладке терминала с мониторингом событий вы увидите сообщение. (рис. [-@fig:001])

![Изменения на второй вкладке](image/image1.jpg){#fig:001 width=70%}

Во второй вкладке терминала с мониторингом остановите трассировку файла сообщений мониторинга реального времени, используя Ctrl + c. Затем запустите мониторинг сообщений безопасности: (рис. [-@fig:002])
  - tail -n 20 /var/log/secure
  
![Мониторинг сообщений безопасности](image/image2.jpg){#fig:002 width=70%}

2. Изменение правил rsyslog.conf
В первой вкладке терминала установите Apache, если он не был ранее инсталлирован: (рис. [-@fig:003])
  - dnf -y install httpd
После окончания процесса установки запустите веб-службу: (рис. [-@fig:003])
  - systemctl start httpd
  - systemctl enable httpd

![Установка Apache](image/image3.jpg){#fig:003 width=70%}

Во второй вкладке терминала посмотрите журнал сообщений об ошибках веб-службы: (рис. [-@fig:004])
  - tail -f /var/log/httpd/error_log

![Журнал соо. об ошибках](image/image4.jpg){#fig:004 width=70%}

В каталоге /etc/rsyslog.d создайте файл мониторинга событий веб-службы: 
  - cd /etc/rsyslog.d
  - touch httpd.conf
Открыв его на редактирование, пропишите в нём: (рис. [-@fig:005])
  - local1.* -/var/log/httpd-error.log

![Редактирование файла](image/image5.jpg){#fig:005 width=70%}

Перейдите в первую вкладку терминала и перезагрузите конфигурацию rsyslogd и веб-службу:
  - systemctl restart rsyslog.service
  - systemctl restart httpd

В третьей вкладке терминала создайте отдельный файл конфигурации для мониторинга отладочной информации: (рис. [-@fig:006])
  - cd /etc/rsyslog.d
  - touch debug.conf

В этом же терминале введите: (рис. [-@fig:006])
  - echo "*.debug /var/log/messages-debug" > /etc/rsyslog.d/debug.conf

В первой вкладке терминала снова перезапустите rsyslogd:
  - systemctl restart rsyslog.service
  
Во второй вкладке терминала запустите мониторинг отладочной информации:
  - tail -f /var/log/messages-debug
  
В третьей вкладке терминала введите: (рис. [-@fig:006])
  - logger -p daemon.debug "Daemon Debug Message"

В терминале с мониторингом посмотрите сообщение отладки.

![Работа с третьим окном](image/image6.jpg){#fig:006 width=70%}

3. Использование journalctl
Во второй вкладке терминала посмотрите содержимое журнала с событиями с момента последнего запуска системы: (рис. [-@fig:007])
  - journalctl
Просмотр содержимого журнала без использования пейджера: (рис. [-@fig:008])
  - journalctl --no-pager
Режим просмотра журнала в реальном времени: (рис. [-@fig:008])
  - journalctl -f
  
![Журнал с событиями с момента запуска](image/image7.jpg){#fig:007 width=70%}

![Журналы без пейджера и в реальном времени](image/image8.jpg){#fig:008 width=70%}

Для использования фильтрации просмотра конкретных параметров журнала введите
  - journalctl 
и дважды нажмите клавишу Tab (рис. [-@fig:009])

![Фильтрация](image/image9.jpg){#fig:009 width=70%}

Просмотрите события для UID0:
  - journalctl _UID=0
Для отображения последних 20 строк журнала введите:
  - journalctl -n 20
Для просмотра только сообщений об ошибках введите: (рис. [-@fig:010])
  - journalctl -p err
Для просмотра всех сообщений со вчерашнего дня введите:
  - journalctl --since yesterday

Если вы хотите показать все сообщения с ошибкой приоритета, которые были зафиксированы со вчерашнего дня, то используйте:
  - journalctl --since yesterday -p err
Если вам нужна детальная информация, то используйте:
  - journalctl -o verbose
Для просмотра дополнительной информации о модуле sshd введите:
  - journalctl _SYSTEMD_UNIT=sshd.service
  
![Сообщения об ошибках](image/image10.jpg){#fig:010 width=70%}

4. Постоянный журнал journald
Запустите терминал и получите полномочия администратора. 
Создайте каталог для хранения записей журнала: (рис. [-@fig:011])
  - mkdir -p /var/log/journal
Скорректируйте права доступа для каталога /var/log/journal, чтобы journald смог записывать в него информацию: (рис. [-@fig:0111])
  - chown root:systemd-journal /var/log/journal
  - chmod 2755 /var/log/journal
Для принятия изменений необходимо или перезагрузить систему (перезапустить службу systemd-journald недостаточно), или использовать команду: (рис. [-@fig:011])
  - killall -USR1 systemd-journald
  
![Каталог для хранения записей журнала](image/image11.jpg){#fig:011 width=70%}

Если вы хотите видеть сообщения журнала с момента последней перезагрузки, используйте: (рис. [-@fig:012])
  - journalctl -b
  
![Журнал с момента посл. перезагрузки](image/image12.jpg){#fig:012 width=70%}

# Контрольные вопросы

1. /etc/rsyslog.conf
2. /var/log/secure
3. Неделя
4. info.* - /var/log/messages.info
5. tail -f /var/log/messages
6. journalctl _PID=1 -since “2025-02-01 09:00:00” –until “2025-02-01 15:00:00”
7. journalctl - b
8. Запустите терминал и получите полномочия администратора: su –
Создайте каталог для хранения записей журнала: 
  - mkdir -p /var/log/journal
Скорректируйте права доступа для каталога /var/log/journal, чтобы journald смог записывать в него информацию:
  - chown root:systemd-journal /var/log/journal
  - chmod 2755 /var/log/journal
Для принятия изменений необходимо или перезагрузить систему (перезапустить службу systemd-journald недостаточно), или использовать команду: 
  - killall -USR1 systemd-journald

# Выводы

Получены навыки работы с журналами мониторинга различных событий в системе.

