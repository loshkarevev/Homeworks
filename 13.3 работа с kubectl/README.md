## Решение
### 1. Проверить работоспособность каждого компонента
Проверку осуществляем на ранее созданных приложениях
![get po.png](https://github.com/loshkarevev/Homeworks/blob/main/13.3%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20kubectl/get%20po.png)

Проверяем доступ к фронту, через port-forward
![product-fe.png](https://github.com/loshkarevev/Homeworks/blob/main/13.3%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20kubectl/product-fe.png)

Проверяем доступ к бэку, через port-forward
![product-be.png](https://github.com/loshkarevev/Homeworks/blob/main/13.3%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20kubectl/product-be.png)

Проверяем доступ к БД, через port-forward
![DB.png](https://github.com/loshkarevev/Homeworks/blob/main/13.3%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20kubectl/DB.png)

Проверяем доступ к фронту, бэку, БД через exec
![exec.png](https://github.com/loshkarevev/Homeworks/blob/main/13.3%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20kubectl/exec.png)
### 2. Ручное масштабирование
#### Увеличение количество бекенда и фронта до 3
```
root@node1:~/kubespray# kubectl scale --replicas=3 deployment/product-be
deployment.apps/product-be scaled
root@node1:~/kubespray# kubectl scale --replicas=3 deployment/product-fe
deployment.apps/product-fe scaled
```
```
root@node1:~# kubectl describe deployment/product-be
Name:                   product-be
Namespace:              default
CreationTimestamp:      Mon, 09 Aug 2021 13:15:38 +0000
Labels:                 app=ecommerce
                        tier=backend
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=ecommerce,tier=backend
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=ecommerce
           tier=backend
  Containers:
   product-be:
    Image:      chrischinchilla/humanitech-product-be
    Port:       8080/TCP
    Host Port:  0/TCP
    Environment:
      DATABASE_HOST:      postgres
      DATABASE_NAME:      product
      DATABASE_PASSWORD:  pr0dr0b0t
      DATABASE_USER:      product_robot
      DATABASE_PORT:      5432
    Mounts:               <none>
  Volumes:                <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      True    MinimumReplicasAvailable
OldReplicaSets:  <none>
NewReplicaSet:   product-be-56f584c9b7 (3/3 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  2m11s  deployment-controller  Scaled up replica set product-be-56f584c9b7 to 3
```
```
root@node1:~# kubectl describe deployment/product-fe
Name:                   product-fe
Namespace:              default
CreationTimestamp:      Mon, 09 Aug 2021 13:11:27 +0000
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=ecommerce,tier=frontend
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=ecommerce
           tier=frontend
  Containers:
   client:
    Image:      chrischinchilla/humanitech-product-fe
    Port:       8080/TCP
    Host Port:  0/TCP
    Environment:
      PRODUCT_BE_SERVER_URL:  product-be
    Mounts:                   <none>
  Volumes:                    <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      True    MinimumReplicasAvailable
OldReplicaSets:  <none>
NewReplicaSet:   product-fe-56946dbfdc (3/3 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  2m53s  deployment-controller  Scaled up replica set product-fe-56946dbfdc to 3
```
#### Уменьшение количество копий до 1
```
root@node1:~/kubespray# kubectl scale --replicas=1 deployment/product-be
deployment.apps/product-be scaled
root@node1:~/kubespray# kubectl scale --replicas=1 deployment/product-fe
deployment.apps/product-fe scaled
```
```
root@node1:~# kubectl describe deployment/product-be
Name:                   product-be
Namespace:              default
CreationTimestamp:      Mon, 09 Aug 2021 13:15:38 +0000
Labels:                 app=ecommerce
                        tier=backend
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=ecommerce,tier=backend
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=ecommerce
           tier=backend
  Containers:
   product-be:
    Image:      chrischinchilla/humanitech-product-be
    Port:       8080/TCP
    Host Port:  0/TCP
    Environment:
      DATABASE_HOST:      postgres
      DATABASE_NAME:      product
      DATABASE_PASSWORD:  pr0dr0b0t
      DATABASE_USER:      product_robot
      DATABASE_PORT:      5432
    Mounts:               <none>
  Volumes:                <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      True    MinimumReplicasAvailable
OldReplicaSets:  <none>
NewReplicaSet:   product-be-56f584c9b7 (1/1 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  4m57s  deployment-controller  Scaled up replica set product-be-56f584c9b7 to 3
  Normal  ScalingReplicaSet  40s    deployment-controller  Scaled down replica set product-be-56f584c9b7 to 1
```
```
root@node1:~# kubectl describe deployment/product-fe
Name:                   product-fe
Namespace:              default
CreationTimestamp:      Mon, 09 Aug 2021 13:11:27 +0000
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=ecommerce,tier=frontend
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=ecommerce
           tier=frontend
  Containers:
   client:
    Image:      chrischinchilla/humanitech-product-fe
    Port:       8080/TCP
    Host Port:  0/TCP
    Environment:
      PRODUCT_BE_SERVER_URL:  product-be
    Mounts:                   <none>
  Volumes:                    <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      True    MinimumReplicasAvailable
OldReplicaSets:  <none>
NewReplicaSet:   product-fe-56946dbfdc (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  5m9s  deployment-controller  Scaled up replica set product-fe-56946dbfdc to 3
  Normal  ScalingReplicaSet  65s   deployment-controller  Scaled down replica set product-fe-56946dbfdc to 1
```
