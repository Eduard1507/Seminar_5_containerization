Урок 5. Docker Compose и Docker Swarm
Classwork
Знакомились с работой Docker Compose и Docker Swarm

Homework
Задание 1:
создать сервис, состоящий из 2 различных контейнеров: 1 - веб, 2 - БД
далее необходимо создать 3 сервиса в каждом окружении (dev, prod, lab)
по итогу на каждой ноде должно быть по 2 работающих контейнера
выводы зафиксировать
Задание 2*:
нужно создать 2 ДК-файла, в которых будут описываться сервисы
повторить задание 1 для двух окружений: lab, dev
обязательно проверить и зафиксировать результаты, чтобы можно было выслать преподавателю для проверки
Задание со звездочкой - повышенной сложности, это нужно учесть при выполнении (но сделать его необходимо).

Проверить работоспособность контейнеров.
Термины

Для того чтобы пользоваться swarm, надо запомнить несколько типов сущностей:

Node - это наши виртуальные машины, на которых установлен docker. Есть manager и workers ноды. Manager нода управляет workers нодами. Она отвечает за создание/обновление/удаление сервисов на workers, а также за их масштабирование и поддержку в требуемом состоянии. Workers ноды используются только для выполнения поставленных задач и не могут управлять кластером.

Stack - это набор сервисов, которые логически связаны между собой. По сути это набор сервисов, которые мы описываем в обычном compose файле. Части stack (services) могут располагаться как на одной ноде, так и на разных.

Service - это как раз то, из чего состоит stack. Service является описанием того, какие контейнеры будут создаваться. Если вы пользовались docker-compose.yaml, то уже знакомы с этой сущностью. Кроме стандартных полей docker в режиме swarm поддерживает ряд дополнительных, большинство из которых находятся внутри секции deploy.

Task - это непосредственно созданный контейнер, который docker создал на основе той информации, которую мы указали при описании service. Swarm будет следить за состоянием контейнера и при необходимости его перезапускать или перемещать на другую ноду.

Первый вариант решение(без использования второй node):

Создаем два ДК-файла:
![1](https://github.com/user-attachments/assets/53c094f3-ca46-472c-a3f0-b1525c1882b1)





Компилируем node:



Проверяем:



Пытаемся задеплоить в Stack:





Получаем ошибку: "версия данного файла не поддерживается", продолжаем делать, как делали на семинаре. Запускаем первый ДК-файл:



Проверяем:





Запускаем второй ДК-файл:







Второй вариант решение(с использованием второй node):

Подключаемся по SSH ко второй VM и устанавливаем swarm соединение, предварительно инициализировав manager node:



Запускаем первый ДК-файл на manager node:



Проверяем:



Запускаем второй ДК-файл на worker node:



Проверяем:



Попробуем запустить первый ДК-файл на node worker:



Проверяем, все работает, ошибки дублирования не произошло т. к имена image разные(ДК-файлы в разных директориях находятся на VM)



Запускаем на manager node второй ДК-файл и получаем четыре работающих контейнера на одной node и на другой (не считая hello-word):



Подготовил студент GeekBrains Калуга Эдуард, Seminar_5_containerization
