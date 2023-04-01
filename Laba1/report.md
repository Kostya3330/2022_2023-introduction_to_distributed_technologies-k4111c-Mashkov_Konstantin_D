University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2022/2023  
Group: K4111c  
Author: Mashkov Konstantin Dmitrievich  
Lab: Lab1  
Date of create: 20.03.2023  
Date of finished: -

# Лабораторная работа №1 "Установка Docker и Minikube, мой первый манифест."

## Описание
Это первая лабораторная работа в которой вы сможете протестировать Docker, установить Minikube и развернуть свой первый "под".

## Цель работы
Ознакомиться с инструментами Minikube и Docker, развернуть свой первый "под".

## Ход работы

1. Запускаем minicube

  `minikube start`

 ![изображение](https://user-images.githubusercontent.com/90138874/229286956-fb26f480-8ccf-4afc-9434-aa98d2a3d8ff.png)

2. Для взаимодействия с k8c в дальнейшем вводим:

`minicube kubectl`

![изображение](https://user-images.githubusercontent.com/90138874/229286975-4ef6a84b-200e-4d07-ae84-6fa8d5ebd540.png)

 
3. Проверяем, запустился ли узел:

`kubectl get nodes`
 
 ![изображение](https://user-images.githubusercontent.com/90138874/229287001-2cf51ffa-9891-43a5-8d9f-c83f17e46060.png)


4.Скачиваем имейдж Vault (я решил попробовать обычную cmd):

`pull vault`

![изображение](https://user-images.githubusercontent.com/90138874/229287009-5c516bef-09fa-4aa1-ae4a-141fc59afbce.png)

 
5. Проверяем, появился ли он:
`docker image ls`

![изображение](https://user-images.githubusercontent.com/90138874/229287021-2c4ba799-3e36-4844-82b7-739a39b480d2.png)

 
6. Создаём контейнер Vault:

`docker run -d --name vault vault`

![изображение](https://user-images.githubusercontent.com/90138874/229287025-7b360cc3-514b-4825-af66-40f3ee627018.png)

 
7. Проверяем появился ли:

`docker ps`

![изображение](https://user-images.githubusercontent.com/90138874/229287033-3b8a4f5a-2039-45bb-9695-154dcc0ecfaa.png)

 
Да, появился и запущен. В графическом интерфейсе Docker’а он тоже есть

![изображение](https://user-images.githubusercontent.com/90138874/229287043-f7fd58e8-2bcd-48b3-a5b7-d9ffcc04faa5.png)

 
8. Создаём манифест для будущего пода:

![изображение](https://user-images.githubusercontent.com/90138874/229287046-958f1814-1a3d-42f6-ad25-966f08d12b19.png)

 
9. Переходим в папку и создаём под:

`kubectl create -f manifestfile.yaml`

![изображение](https://user-images.githubusercontent.com/90138874/229287049-2a7cf626-1230-4a6b-bd6e-8598b173cdc8.png)

 
10. Проверяем:

`kubectl get pods`

![изображение](https://user-images.githubusercontent.com/90138874/229287051-a6a92d83-54fe-45a9-9a83-a8e55969e6b8.png)

 
11. Создаём сервис для доступа к поду и делаем так, чтобы его можно было открыть с 8200 порта:

`minikube kubectl -- expose pod vault --type=NodePort --port=8200`

![изображение](https://user-images.githubusercontent.com/90138874/229287059-0134781f-6f25-43a4-bfb3-98a3dcfb45e9.png)

 
12. Проверяем:

![изображение](https://user-images.githubusercontent.com/90138874/229287063-68f65c1b-6bf3-43a0-a769-93548bcb141b.png)

 
13.Ищем токен в логах Vault:

![изображение](https://user-images.githubusercontent.com/90138874/229287067-4e4ddd3f-0541-433c-bc29-210cd48c48ac.png)

 
Нашли: 

![изображение](https://user-images.githubusercontent.com/90138874/229287073-34a17745-8028-4788-afa4-15714e40d493.png)

После ввода видим:

![изображение](https://user-images.githubusercontent.com/90138874/229287079-788b32ca-b774-48e5-b348-a84ddd1558df.png)

 
14. Останавливаем minicube:

`minicube stop`

![изображение](https://user-images.githubusercontent.com/90138874/229287090-4edb3d78-267b-4117-94fe-71e5767da58c.png)



 
## Схема организации контейеров и сервисов нарисованная вами в draw.io:

![изображение](https://user-images.githubusercontent.com/90138874/229287097-109d932b-496e-4949-a571-a5662316ed14.png)

 

