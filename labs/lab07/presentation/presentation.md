---
## Front matter
lang: ru-RU
title: Лабораторная работа №7
subtitle: Презентация
author:
  - Ермишина М. К.
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 17 октября 2025

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

## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
---

# Информация

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

  * Ермишина Мария Кирилловна
  * студент группы НПИбд-01-24
  * Российский университет дружбы народов
  * [1132230166@pfur.ru](mailto:1132230166@pfur.ru)
  * <https://github.com/ErmiMash>

:::
::: {.column width="30%"}

:::
::::::::::::::


# Элементы презентации

## Цели и задачи

Целью данной лабораторной работы является получение навыков работы с журналами мониторинга различных событий в системе.

# Выполнение лабораторной работы

## Мониторинг журнала системных событий в реальном времени
Получив полномочия администратора на второй вкладке терминала запустите мониторинг системных событий в реальном времени:
  - tail -f /var/log/messages
В третьей вкладке терминала вернитесь к учётной записи своего пользователя и попробуйте получить полномочия администратора, но введите неправильный пароль. После введите "logger hello"
![Изменения на второй вкладке](image/image1.jpg){#fig:001 width=50%}

## Изменение правил rsyslog.conf
В первой вкладке терминала установите Apache, если он не был ранее инсталлирован:
  - dnf -y install httpd
  - systemctl start httpd
  - systemctl enable httpd
![Установка Apache](image/image3.jpg){#fig:003 width=50%}

## Мониторинг событий веб-службы
В каталоге /etc/rsyslog.d создайте файл мониторинга событий веб-службы: 
  - cd /etc/rsyslog.d
  - touch httpd.conf
Открыв его на редактирование, пропишите в нём:
  - local1.* -/var/log/httpd-error.log
![Редактирование файла](image/image5.jpg){#fig:005 width=50%} 

## Файл конфигурации для мониторинга отладочной информации
В третьей вкладке терминала создайте отдельный файл:
  - cd /etc/rsyslog.d
  - touch debug.conf
В этом же терминале введите:
  - echo "*.debug /var/log/messages-debug" > /etc/rsyslog.d/debug.conf
А после: - logger -p daemon.debug "Daemon Debug Message"
![Работа с третьим окном](image/image6.jpg){#fig:006 width=70%}

## Использование journalctl
Во второй вкладке терминала посмотрите содержимое журнала с событиями с момента последнего запуска системы:
  - journalctl
Просмотр содержимого журнала без использования пейджера: 
  - journalctl --no-pager
Режим просмотра журнала в реальном времени:
  - journalctl -f
![Журналы без пейджера и в реальном времени](image/image8.jpg){#fig:008 width=70%}

## Фильтрация
Для использования фильтрации просмотра конкретных параметров журнала введите
  - journalctl 
и дважды нажмите клавишу Tab 
![Фильтрация](image/image9.jpg){#fig:009 width=70%}

## Команды для работы
Просмотрите события для UID0:
  - journalctl _UID=0
Для отображения последних 20 строк журнала введите:
  - journalctl -n 20
Для просмотра только сообщений об ошибках введите:
  - journalctl -p err
Для просмотра всех сообщений со вчерашнего дня введите:
  - journalctl --since yesterday
![Сообщения об ошибках](image/image10.jpg){#fig:010 width=70%}

## Постоянный журнал journald
Скорректируйте права доступа для каталога /var/log/journal, чтобы journald смог записывать в него информацию:
  - chown root:systemd-journal /var/log/journal
  - chmod 2755 /var/log/journal
Для принятия изменений необходимо или перезагрузить систему: - killall -USR1 systemd-journald
![Каталог для хранения записей журнала](image/image11.jpg){#fig:011 width=70%}

## Журнал с момента посл. перезугрузхки
Если вы хотите видеть сообщения журнала с момента последней перезагрузки, используйте:
  - journalctl -b
![Журнал с момента посл. перезагрузки](image/image12.jpg){#fig:012 width=70%}

## Результаты

Получены навыки работы с журналами мониторинга различных событий в системе.
