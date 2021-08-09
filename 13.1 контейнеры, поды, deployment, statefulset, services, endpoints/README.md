## Решение
### Подготовить тестовый конфиг для запуска приложения
#### Фронтэнд
kubectl create -f [stage.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.1%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D1%8B,%20%D0%BF%D0%BE%D0%B4%D1%8B,%20deployment,%20statefulset,%20services,%20endpoints/stage.yaml)
```
root@node1:~/kubespray# kubectl describe deployment front-back
Name:                   front-back
Namespace:              default
CreationTimestamp:      Mon, 09 Aug 2021 11:05:56 +0000
Labels:                 app=frontback
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=frontback
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=frontback
  Containers:
   front:
    Image:        nginx:1.14.2
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
   back:
    Image:      debian
    Port:       <none>
    Host Port:  <none>
    Command:
      sleep
      3600
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   front-back-5b8f5b9c5b (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  12m   deployment-controller  Scaled up replica set front-back-5b8f5b9c5b to 1
```
#### БД ([Статья](https://www.bmc.com/blogs/kubernetes-postgresql/))
kubectl apply -f [postgresql.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.1%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D1%8B%2C%20%D0%BF%D0%BE%D0%B4%D1%8B%2C%20deployment%2C%20statefulset%2C%20services%2C%20endpoints/postgresql.yaml)
```
root@node1:~/kubespray# kubectl get statefulset
NAME            READY   AGE
postgresql-db   0/1     2m20s
root@node1:~/kubespray# kubectl describe statefulset postgresql-db
Name:               postgresql-db
Namespace:          default
CreationTimestamp:  Mon, 09 Aug 2021 11:14:01 +0000
Selector:           app=postgresql-db
Labels:             <none>
Annotations:        <none>
Replicas:           1 desired | 1 total
Update Strategy:    RollingUpdate
  Partition:        0
Pods Status:        0 Running / 1 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=postgresql-db
  Containers:
   postgresql-db:
    Image:      postgres:latest
    Port:       <none>
    Host Port:  <none>
    Environment:
      POSTGRES_PASSWORD:  testpassword
      PGDATA:             /data/pgdata
    Mounts:
      /data from nfs-pv (rw)
  Volumes:  <none>
Volume Claims:
  Name:          nfs-pv
  StorageClass:
  Labels:        <none>
  Annotations:   <none>
  Capacity:      1Gi
  Access Modes:  [ReadWriteMany]
Events:
  Type    Reason            Age    From                    Message
  ----    ------            ----   ----                    -------
  Normal  SuccessfulCreate  2m48s  statefulset-controller  create Claim nfs-pv-postgresql-db-0 Pod postgresql-db-0 in StatefulSet postgresql-db success
  Normal  SuccessfulCreate  2m48s  statefulset-controller  create Pod postgresql-db-0 in StatefulSet postgresql-db successful
```
