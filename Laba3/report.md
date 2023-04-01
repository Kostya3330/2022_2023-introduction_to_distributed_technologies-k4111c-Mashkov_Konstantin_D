
University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2022/2023  
Group: K4111c  
Author: Mashkov Konstantin Dmitrievich  
Lab: Lab3  
Date of create: 30.3.2023  
Date of finished: -

# Лабораторная работа №3 "Сертификаты и "секреты" в Minikube, безопасное хранение данных."

## Описание
В данной лабораторной работе вы познакомитесь с сертификатами и "секретами" в Minikube, правилами безопасного хранения данных в Minikube.

## Цель работы
Познакомиться с сертификатами и "секретами" в Minikube, правилами безопасного хранения данных в Minikube.

## Ход работы

Запускаю minikube
Нахожу пример манифеста для configmap на https://kubernetes.io/docs/concepts/configuration/configmap/ и создаю его с переменными REACT_APP_USERNAME, REACT_APP_COMPANY_NAME

![изображение](https://user-images.githubusercontent.com/90138874/229293750-84e1eab7-67e0-4835-a91a-18ed190021e7.png)

 
Перехожу в нужную папку и запускаю configmap

![изображение](https://user-images.githubusercontent.com/90138874/229293760-3a811ba1-e0e8-48fa-874f-e24bfa7acd96.png)

 
Проверяю

![изображение](https://user-images.githubusercontent.com/90138874/229293764-974ec00c-f62b-4ade-8c5d-76be0280a2e3.png)

 
Создаю Replicaset с использованием шаблона с https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
Цель ReplicaSet — поддерживать стабильный набор подов реплик, работающих в любой момент времени. Таким образом, он часто используется, чтобы гарантировать доступность определенного количества идентичных подов.
В env объявляем REACT_APP_USERNAME и REACT_APP_COMPANY_NAME, на значения ссылаемся из файла configmap

![изображение](https://user-images.githubusercontent.com/90138874/229293772-1ee62640-02d4-4df0-a2dd-b445b5e21fb5.png)

 
Что-то не так

![изображение](https://user-images.githubusercontent.com/90138874/229293780-6d2a1033-c49c-4c4a-9ed5-5fb6448a0113.png)

 
После исправления табуляции следующая ошибка:

![изображение](https://user-images.githubusercontent.com/90138874/229293787-12bdf004-27e1-445f-ac07-e3b9fa49dbb9.png)

 
Вместо configmap надо было написать configMapKeyRef

![изображение](https://user-images.githubusercontent.com/90138874/229293796-6db1fc94-6296-4b61-b9da-b8b09b4b7efc.png)

 
Итоговый вариант:

![изображение](https://user-images.githubusercontent.com/90138874/229293805-2d583a4f-ef89-4b67-8fba-f820e05df0e8.png)

 
Проверяем
 
 ![изображение](https://user-images.githubusercontent.com/90138874/229293811-5e97f51b-e4a1-4a3f-8543-4f978b6b8f42.png)


Создаём Ingress, пользуясь указаниями и шаблоном на https://kubernetes.io/docs/concepts/services-networking/ingress/
Выбираем NodePort на 30000 порте

![изображение](https://user-images.githubusercontent.com/90138874/229293818-42e2a9ea-51a5-401f-a18b-088212c528e2.png)

 
Создаю сервис

![изображение](https://user-images.githubusercontent.com/90138874/229293824-25cfd07a-1dcd-4432-b630-75ab48f804ea.png)

 
Генерация TLS
Установил OpenSSL, генерирую и подписываю ключ:

![изображение](https://user-images.githubusercontent.com/90138874/229293832-a0db2a95-488c-4d73-9fca-74796c383af2.png)

 
Можно посмотреть сертификаты:

`openssl req -text -noout -verify -in domain.csr`

![изображение](https://user-images.githubusercontent.com/90138874/229293851-17e8b8a7-e3c7-40e2-859c-2c13abb4c44d.png)

 
Создание Secret

![изображение](https://user-images.githubusercontent.com/90138874/229293855-5962d385-dc24-467f-a71f-44ee398d04e5.png)

 
Возникла ошибка
Подписываю сертификаты ключом, который был недавно сгенерирован

`openssl x509 -signkey lab3.key -in lab3.csr -req -days 30 -out lab3.crt`

![изображение](https://user-images.githubusercontent.com/90138874/229293874-2a09f23a-5506-4d79-a5f8-721180711518.png)

 
Secret создался

![изображение](https://user-images.githubusercontent.com/90138874/229293879-6e248fb5-3dc0-4af1-bd83-0faed7fc6978.png)

 
### Создание Ingress

`minikube addons enable ingress`

`minikube addons enable ingress-dns`

![изображение](https://user-images.githubusercontent.com/90138874/229293893-e3baca70-46ff-4f22-a465-1a7108041fab.png)

 
Создаю манифест с использованием https://kubernetes.io/docs/concepts/services-networking/ingress/

![изображение](https://user-images.githubusercontent.com/90138874/229293895-4fd56cfa-9ed2-4758-9efa-7915c63f20c7.png)

 
Создаю точку входа в minicube:

![изображение](https://user-images.githubusercontent.com/90138874/229293899-1eb0a887-9a98-415a-a6f8-dcf631f73565.png)

 
Проверяю

![изображение](https://user-images.githubusercontent.com/90138874/229293911-30dac557-a7bf-44b8-9dd4-29d87476f536.png)

 
Подключаюсь к Ingress 

`minikube tunnel`

![изображение](https://user-images.githubusercontent.com/90138874/229293921-152546ad-0328-49e6-9818-686a40a3caa2.png)

 
Переходим в браузер и смотрим сертификат

![изображение](https://user-images.githubusercontent.com/90138874/229293932-f5165d55-ef8f-4527-ae03-7a69bcca444f.png)

 ![изображение](https://user-images.githubusercontent.com/90138874/229293935-709ced0c-f327-4489-b161-d47c219de69a.png)

### Схема организации

![изображение](https://user-images.githubusercontent.com/90138874/229293944-bbcd2476-3710-4b66-85e6-d71eecb1a7b1.png)

 
