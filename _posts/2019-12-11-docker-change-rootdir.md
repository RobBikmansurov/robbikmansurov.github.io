---
layout: post
title:  Перенос docker storage (docker Root Dir)
tags:   docker
categories: docker, rootdir
keywords: docker, rootdir, devops
---
Разрабатываю приложения в docker-контейнерах. Плюсы: не загаживается своя система, всегда есть переносимый образ, который позволит запустить этот процесс другому разработчику. Ну и, традиционно, столкнулся с проблемой нехватки места на системном разделе.

**Задача - перенести данные docker на раздел /home**.

По умолчанию в Ubuntu docker хранит образы в
```bash
$ docker info | awk '/Root Dir/ {print $NF}'
/var/lib/docker
```
Изменить место хранения образов можно различными способами. Я хочу перенести данные docker из раздела / в свой раздел /home, там больше места.
## Перенос данных docker в новое место
1. Остановить все контейнеры: `$ docker stop $(docker ps -q)`
2. Остановить демон docker: ```$ sudo systemctl stop docker```
3. Перенести данные в другой раздел (на самом деле, скопируем. паранойя) `$ sudo cp -ar /var/lib/docker /home/docker/`
4. Добавить опции запуска docker 
```bash
$ sudo vi /etc/default/docker
DOCKER_OPTS="-g /home/docker"
```
5. Запустить демон docker `$ sudo systemctl start docker`
6. Проверим новый Root Dir
```bash
$ docker info | awk '/Root Dir/ {print $NF}'
/var/lib/docker
```
У-у-п-с, ничего не поменялось переходим к плану Б

## Симлинк
После остановки демона и переноса данных сделать симлинк на новый раздел для docker
```bash ln -s /home/docker /var/lib/docker
```

Результат - освободил гигабайты места на системном разделе.
