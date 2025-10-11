---
## Front matter
lang: ru-RU
title: Лабораторная работа №5
subtitle: Презентация
author:
  - Ермишина М. К.
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 15 марта 2025

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

Целью данной лабораторной работы является получение навыков управления системными службами операционной системы посредством systemd.

# Выполнение лабораторной работы

## Управление сервисами
Для начала получите права администратора. После проверьте статус службы Very Secure FTP:
  - systemctl status vsftpd
Установите службу Very Secure FTP:
  - dnf -y install vsftpd
Запустите службу Very Secure FTP и проверьте ее статус:
  - systemctl start vsftpd
  - systemctl status vsftpd

![Служба VSFTP](image/image1.jpg){#fig:001 width=70%}

## Very Secure FTP
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

## firewalld и iptables
Попробуйте запустить firewalld и iptables:
  - systemctl start firewalld
  - systemctl start iptables
Введите для проверки настроек юнита: (рис. [-@fig:003])
  - cat /usr/lib/systemd/system/firewalld.service
  - cat /usr/lib/systemd/system/iptables.service

![Настройки юнитов](image/image3.jpg){#fig:003 width=70%}
  
## iptables
Заблокируйте запуск iptables, введя:
  - systemctl mask iptables
Попробуйте запустить iptables:
  - systemctl start iptables
Попробуйте добавить iptables в автозапуск:
  - systemctl enable iptables

![iptables](image/image4.jpg){#fig:004 width=70%}

## Изолируемые цели
Получите полномочия администратора. Перейдите в каталог systemd и найдите список всех целей, которые можно изолировать:
  - cd /usr/lib/systemd/system
  - grep Isolate *.target
Переключите операционную систему в режим восстановления:
  - systemctl isolate rescue.target 
Перезапустите операционную систему следующим образом:
  - systemctl isolate reboot.target

![Настройки ОС](image/image5.jpg){#fig:005 width=70%}

## Цель по умолчанию
Для запуска по умолчанию текстового режима введите
  - systemctl set-default multi-user.target
Перегрузите систему командой reboot. Убедитесь, что система загрузилась в текстовом режиме. Получите полномочия администратора. Для запуска по умолчанию графического режима введите
  - systemctl set-default graphical.target

![Изменения виды ОС](image/image6.jpg){#fig:006 width=70%}

## Результаты

В ходе данной лабораторной работы получены навыков управления системными службами операционной системы посредством systemd. 
