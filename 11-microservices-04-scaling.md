
# Домашнее задание к занятию «Микросервисы: масштабирование»

Вы работаете в крупной компании, которая строит систему на основе микросервисной архитектуры.
Вам как DevOps-специалисту необходимо выдвинуть предложение по организации инфраструктуры для разработки и эксплуатации.

## Задача 1: Кластеризация

Предложите решение для обеспечения развёртывания, запуска и управления приложениями.
Решение может состоять из одного или нескольких программных продуктов и должно описывать способы и принципы их взаимодействия.

Решение должно соответствовать следующим требованиям:
- поддержка контейнеров;
- обеспечивать обнаружение сервисов и маршрутизацию запросов;
- обеспечивать возможность горизонтального масштабирования;
- обеспечивать возможность автоматического масштабирования;
- обеспечивать явное разделение ресурсов, доступных извне и внутри системы;
- обеспечивать возможность конфигурировать приложения с помощью переменных среды, в том числе с возможностью безопасного хранения чувствительных данных таких как пароли, ключи доступа, ключи шифрования и т. п.

Обоснуйте свой выбор.


|Критерий/Оркестратор|	Docker Swarm | Nomad	| Apache Mesos	| Fleet |	K8s |
|--------------------|:-------------:|:------:|:-------------:|:-----:|:---:|
|Типы рабочих нагрузок|:white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|
|Легкость установки и исходной настройки (вручную)|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|	:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark:|
|Легкость администрирования кластеров	|:white_check_mark::white_check_mark:	|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|
|Требования к платформе для развертывания|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark:|:white_check_mark::white_check_mark:|
|Производительность	|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|
|Ограничения на количество узлов и контейнеров в кластере|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark:|:white_check_mark::white_check_mark:|
|Конфигурация как код	|:white_check_mark::white_check_mark:|:white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|
|Шаблоны	|:white_check_mark:|:white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|
|Сети	|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|
|Возможность обнаружения сервисов|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|
|Автомасштабирование|:white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|
|Выполнение обновлений и отката	|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:	|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|
|Отказоустойчивость|:white_check_mark::white_check_mark::white_check_mark:|	:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|
|Мониторинг	|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|
|Безопасность	|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark::white_check_mark:|:white_check_mark:|:white_check_mark::white_check_mark::white_check_mark:|

- Kubernetes поддерживает разные системы контейнеризации.
- Kubernetes DNS. Каждому сервису, созданному с помощью объекта service присваивается доменное имя совпадающее с именем самого сервиса. Маршрутизация происходит через kube-proxy и virtual ip.
В Kubernetes поддерживается автомасштабирование на основе наблюдаемого использования ЦП, памяти и ряда других пользовательских метрик. При этом масштабирование доступно на нескольких уровнях:
  - Автомасштабирование кластера с использованием Cluster Autoscaler, отвечающее за изменение числа узлов в кластере.
  - Горизонтальное автомасштабирование подов (Horizontal Pod Autoscaler, HPA), которое автоматически изменяет количество подов в зависимости от значений выбранных показателей.
  - Вертикальное автомасштабирование подов (Vertical Pod Autoscaler, VPA), которое автоматически изменяет объем ресурсов, выделяемых существующим подам.
- Namespaces - это способ разделить кластер на индивидуальные зоны. NetworkPolicy и GlobalNetworkPolicy дает разграничение доступа внутри кластера, контроль исходящего и входящего трафика. ingress-controller - обеспечивает маршрутизацию трафика.
- В Kubernetes есть объект - secret для хранения чувствительных данных. Также есть vault для множества функционала по хранению секретов. Env для установки переменных сред в контейнеры.

Самодостаточный инструмент оркестровки, в который встроено множество сервисов. Kubernetes предоставляет все функции, необходимые для запуска приложений на основе контейнеров, включая: управление кластером, планирование, обнаружение служб, мониторинг, управление безопасностью и многое другое.
Поддерживается фондом CNCF (Cloud Native Computing Foundation). У Kubernetes самое впечатляющее по числу участников сообщество среди всех оркестраторов, что обеспечивает богатый инструментарий и большое число готовых решений.
Это бесплатный инструмент с открытым исходным кодом, который работает в любой ОС.

[Источник](https://habr.com/ru/companies/vk/articles/543232/)



## Задача 2: Распределённый кеш * (необязательная)

Разработчикам вашей компании понадобился распределённый кеш для организации хранения временной информации по сессиям пользователей.
Вам необходимо построить Redis Cluster, состоящий из трёх шард с тремя репликами.

### Схема:

![11-04-01](https://user-images.githubusercontent.com/1122523/114282923-9b16f900-9a4f-11eb-80aa-61ed09725760.png)

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
