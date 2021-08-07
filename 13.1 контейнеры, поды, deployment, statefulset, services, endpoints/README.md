## Решение
### Подготовить тестовый конфиг для запуска приложения
#### Фронтэнд
1. kubectl create -f [stage.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.1%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D1%8B,%20%D0%BF%D0%BE%D0%B4%D1%8B,%20deployment,%20statefulset,%20services,%20endpoints/stage.yaml)
2. 
#### БД ([Статья](https://www.bmc.com/blogs/kubernetes-postgresql/))
1. kubectl apply -f [postgresql.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.1%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D1%8B%2C%20%D0%BF%D0%BE%D0%B4%D1%8B%2C%20deployment%2C%20statefulset%2C%20services%2C%20endpoints/postgresql.yaml)
```
root@node1:~/kubespray# kubectl get statefulset
NAME            READY   AGE
postgresql-db   0/1     67m
root@node1:~/kubespray# kubectl describe statefulset postgresql-db
Name:               postgresql-db
Namespace:          default
CreationTimestamp:  Thu, 05 Aug 2021 16:35:26 +0000
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
      /data from postgresql-db-disk (rw)
  Volumes:  <none>
Volume Claims:
  Name:          postgresql-db-disk
  StorageClass:
  Labels:        <none>
  Annotations:   <none>
  Capacity:      1Gi
  Access Modes:  [ReadWriteOnce]
Events:          <none>
```
