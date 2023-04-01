
University: [ITMO University](https://itmo.ru/ru/)  
Faculty: [FICT](https://fict.itmo.ru)  
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)  
Year: 2022/2023  
Group: K4111c  
Author: Mashkov Konstantin Dmitrievich  
Lab: Lab4  
Date of create: 31.03.2023  
Date of finished: -

# Лабораторная работа №4 "Сети связи в Minikube, CNI и CoreDNS"

## Описание
Это последняя лабораторная работа в которой вы познакомитесь с сетями связи в Minikube. Особенность Kubernetes заключается в том, что у него одновременно работают underlay и overlay сети, а управление может быть организованно различными CNI.

## Цель работы
Познакомиться с CNI Calico и функцией IPAM Plugin, изучить особенности работы CNI и CoreDNS.

## Ход работы

Запускаю minicube и устанавливаю плагин Calico и разворачиваю 2 ноды:

`minikube start --network-plugin=cni --cni=calico --nodes 2 -p multinode-demo`

![изображение](https://user-images.githubusercontent.com/90138874/229294728-58e99e8a-d400-44fc-8d8c-e59cda6c2b0f.png)

 
Проверим работу Calico (поды с пометкой Calico-node)

`kubectl get pods -l k8s-app=calico-node -A`

![изображение](https://user-images.githubusercontent.com/90138874/229294737-823ee00d-b59f-4ff7-9975-827598722c68.png)
 
### Calicoctl и IPPool

Чтобы назначить метки узлам, использую следующие команды

`kubectl label nodes multinode-demo zone=east`  

`kubectl label nodes multinode-demo-m02 zone=west`

![изображение](https://user-images.githubusercontent.com/90138874/229294809-f4e18863-a422-4f47-a0b2-d854eb633523.png)

 
Пишем манифест для IPPool с использованием https://docs.tigera.io/calico/latest/networking/ipam/assign-ip-addresses-topology

![изображение](https://user-images.githubusercontent.com/90138874/229294840-ff97c0c3-d0ed-48d3-ab3f-55e238fadf3c.png)

 
Установка CNI сводится к применению файла calico.yaml, скачанного с официального сайта. Загружаем и разворачиваем манифест 

`kubectl apply -f calicoctl.yaml`

![изображение](https://user-images.githubusercontent.com/90138874/229294851-6dbf2dba-e869-4995-affd-afd9f2435374.png)

 
Проверим IPPool’ы по умолчанию

![изображение](https://user-images.githubusercontent.com/90138874/229294864-b6c80ac3-0d45-404e-b281-64cc34b91f13.png)

 
Удаляем его

`kubectl delete ippools default-ipv4-ippool`

![изображение](https://user-images.githubusercontent.com/90138874/229294877-9c611063-20fa-4104-a267-ef17d3c05a40.png)

Создаём новые IPPool’ы
`kubectl exec -i -n kube-system calicoctl -- /calicoctl --allow-version-mismatch create -f - < ippool.yaml`

PowerShell выдаёт ошибку, пробуем cmd
 
 ![изображение](https://user-images.githubusercontent.com/90138874/229294903-cc294db5-9024-42df-88b7-f7f77e45fd45.png)

Проверяем:

![изображение](https://user-images.githubusercontent.com/90138874/229294918-ec468c9b-44c3-4253-848e-5527f9378aaf.png)

 
### Deployment и Service
Шаблон для манифеста возьмём с https://kubernetes.io/docs/tasks/access-application-cluster/create-external-load-balancer/?hl=ru
 
 ![изображение](https://user-images.githubusercontent.com/90138874/229294942-70323f87-574f-4e7c-8d24-a9e50ffcfbff.png)

Deployment возьму со 2 лабораторной 

![изображение](https://user-images.githubusercontent.com/90138874/229294954-e0bdaafa-34f0-4f42-bb98-f1f439708bd1.png)

 
Проверяем:
 
 ![изображение](https://user-images.githubusercontent.com/90138874/229294958-534bf1f7-9ea3-44c2-933d-f24e92744cc7.png)

IP – адреса созданных портов:

![изображение](https://user-images.githubusercontent.com/90138874/229294965-a3210675-7ac6-437f-8801-170eaea6bcb4.png)
 
Пробрасываю порт

`kubectl port-forward service/lab4-service 8200:3000`

Контейнер не работал, переустановил minikube, заработало

![изображение](https://user-images.githubusercontent.com/90138874/229294981-0b30a59f-dce2-43dd-843d-a0f2074375e2.png)
 
 ![изображение](https://user-images.githubusercontent.com/90138874/229294993-79e84326-a767-40c8-8dcc-c0584d8a3a84.png)

Пингую с первого контейнера:

 ![изображение](https://user-images.githubusercontent.com/90138874/229294998-647b1b8e-1e8b-4d1f-9477-95a25494e22e.png)

Пинг со второго контейнера:

![изображение](https://user-images.githubusercontent.com/90138874/229295005-ca21908a-52cf-4c6d-b0c3-800408d1d1f7.png)

 
## Схема организации:

![изображение](https://user-images.githubusercontent.com/90138874/229295021-1900f67f-092b-4381-aa62-1c5efabd7b26.png)

 

