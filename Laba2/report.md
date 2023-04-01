
University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2022/2023  
Group: K4111c  
Author: Mashkov Konstantin Dmitrievich  
Lab: Lab2  
Date of create: 29.03.2023  
Date of finished: -

# Лабораторная работа №2 "Развертывание веб сервиса в Minikube, доступ к веб интерфейсу сервиса. Мониторинг сервиса."

## Описание
В данной лабораторной работе вы познакомитесь с развертыванием полноценного веб сервиса с несколькими репликами.

## Цель работы
Ознакомиться с типами "контроллеров" развертывания контейнеров, ознакомится с сетевыми сервисами и развернуть свое веб приложение.

## Ход работы

Запускаю minicube

![изображение](https://user-images.githubusercontent.com/90138874/229292834-1cb10a6f-e33d-4161-ae22-75f743b77eb4.png)
 
Скачиваю имейдж нужного контейнера:

![изображение](https://user-images.githubusercontent.com/90138874/229292846-5230d702-d5dc-49e9-b5f8-6785a214e9a3.png)

 
Проверяем

![изображение](https://user-images.githubusercontent.com/90138874/229292851-9f8b85a8-cb07-4fcf-9d2d-9c46f7581eac.png)

 
### Создание Deployment
Нахожу шаблон на странице https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
Создаю манифест для развёртывания пода

![изображение](https://user-images.githubusercontent.com/90138874/229292870-368e9e49-e057-4b2a-98c0-4fd5d09854ac.png)

Не забываю передать в env нужные переменные
Перехожу в нужную папку и создаю манифест
 
Проверяю
 
Прочитал статью https://habr.com/ru/company/southbridge/blog/358824/
Создаю сервис через yaml файл
 
 ![изображение](https://user-images.githubusercontent.com/90138874/229292896-39ab5f5d-b798-42d5-b4fe-ac612fc10a2b.png)

![изображение](https://user-images.githubusercontent.com/90138874/229292903-aa04c1db-50fd-4e8f-b47e-a3e77ab525ab.png)

 
Проверяем

![изображение](https://user-images.githubusercontent.com/90138874/229292908-12dc2424-18c9-45be-8d92-bfd561813ea5.png)

 
Пробрасываю порт контейнера

![изображение](https://user-images.githubusercontent.com/90138874/229292920-11c77b9d-b00e-424f-8f7e-5c70623c367c.png)

 
Я каким-то образом стёр лишнюю строчку в файле сервиса (исправление: добавляю строчку и заново создаю сервис)
 
 ![изображение](https://user-images.githubusercontent.com/90138874/229292934-0fc17cb8-d944-472c-ae12-10782e891e98.png)

 ![изображение](https://user-images.githubusercontent.com/90138874/229292945-ffab903a-fd11-40f5-a74b-8544353a408d.png)


Проверяем
 
 ![изображение](https://user-images.githubusercontent.com/90138874/229292952-eb61b746-5bb4-45f1-8015-63a6dbc2a746.png)

 
Смотрю логи
 
 ![изображение](https://user-images.githubusercontent.com/90138874/229292961-3e3dc63a-ffae-4ff1-9a72-ed26b76beccc.png)

Удаляем Deployment
 
 ![изображение](https://user-images.githubusercontent.com/90138874/229292969-5d89d0f8-c3d6-4934-8f6a-bbe5b6c5781d.png)

Проверяем
 
 ![изображение](https://user-images.githubusercontent.com/90138874/229292979-8434f3e0-e0de-44e0-bcca-ba3b783ab9b9.png)

Останавливаем minikube

![изображение](https://user-images.githubusercontent.com/90138874/229292986-af0ff153-10f0-4c8a-b33f-5b9e59d35102.png)

 
Проверяем

![изображение](https://user-images.githubusercontent.com/90138874/229292994-6d60e356-633b-4e0d-a251-3be0b1a6c646.png)

 
## Схема контейнерно-сервисной организации:
 
![изображение](https://user-images.githubusercontent.com/90138874/229293000-6be433d3-1e51-476d-9261-b761dbf4f684.png)

