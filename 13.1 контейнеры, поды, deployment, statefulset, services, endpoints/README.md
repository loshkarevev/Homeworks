## Решение
### 1. Подготовить тестовый конфиг для запуска приложения
#### Фронтэнд
kubectl apply -f [stage.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.1%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D1%8B,%20%D0%BF%D0%BE%D0%B4%D1%8B,%20deployment,%20statefulset,%20services,%20endpoints/stage.yaml)
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
```
root@node1:~/kubespray# kubectl get pv
NAME     CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                            STORAGECLASS   REASON   AGE
nfs-pv   5Gi        RWX            Retain           Bound    default/nfs-pv-postgresql-db-0                           36m
root@node1:~/kubespray# kubectl get pvc
NAME                     STATUS   VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
nfs-pv-postgresql-db-0   Bound    nfs-pv   5Gi        RWX                           35m
root@node1:~/kubespray# kubectl get po --all-namespaces
NAMESPACE     NAME                                       READY   STATUS              RESTARTS   AGE
default       front-back-5b8f5b9c5b-98x5t                2/2     Running             0          43m
default       nginx-deployment-66b6c48dd5-fm7wp          1/1     Running             0          49m
default       postgresql-db-0                            0/1     ContainerCreating   0          35m
kube-system   calico-kube-controllers-5b4d7b4594-bjsn2   0/1     Shutdown            0          5d22h
kube-system   calico-kube-controllers-5b4d7b4594-r9kq8   1/1     Running             5          23h
kube-system   calico-node-vvcc5                          1/1     Running             125        5d22h
kube-system   coredns-8474476ff8-dkbdn                   1/1     Running             6          5d22h
kube-system   coredns-8474476ff8-x762p                   0/1     Pending             0          5d22h
kube-system   dns-autoscaler-7df78bfcfb-87bsm            1/1     Running             6          5d22h
kube-system   kube-apiserver-node1                       1/1     Running             7          5d22h
kube-system   kube-controller-manager-node1              1/1     Running             7          5d22h
kube-system   kube-proxy-lbqp6                           1/1     Running             7          5d22h
kube-system   kube-scheduler-node1                       1/1     Running             7          5d22h
kube-system   nodelocaldns-cfp6v                         1/1     Running             7          5d22h
policy-demo   nginx-6799fc88d8-qw9h5                     1/1     Running             0          104m
```
#### Для того чтобы заработал nfs еще выполнил:
```
apt install nfs-kernel-server
sudo chown nobody:nogroup /home/nfs
root@node1:/etc# nano exports
/home/nfs       10.0.2.15(rw,sync,no_subtree_check)
systemctl restart nfs-kernel-server
```
И сделал [pv.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.1%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D1%8B%2C%20%D0%BF%D0%BE%D0%B4%D1%8B%2C%20deployment%2C%20statefulset%2C%20services%2C%20endpoints/pv.yaml) и [pv_prod.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.1%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D1%8B%2C%20%D0%BF%D0%BE%D0%B4%D1%8B%2C%20deployment%2C%20statefulset%2C%20services%2C%20endpoints/pv_prod.yaml).
Почему падает postgresql-db-0 так и не смог разобраться.
### 2. Конфиг для production окружения
[Статья](https://humanitec.com/blog/deploy-with-kubectl-hands-on-with-kubernetes) в основе выполнения задания.
#### Фронтэнд
kubectl apply -f [front.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.1%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D1%8B%2C%20%D0%BF%D0%BE%D0%B4%D1%8B%2C%20deployment%2C%20statefulset%2C%20services%2C%20endpoints/front.yaml)
#### Бэкэнд
kubectl apply -f [back.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.1%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D1%8B%2C%20%D0%BF%D0%BE%D0%B4%D1%8B%2C%20deployment%2C%20statefulset%2C%20services%2C%20endpoints/back.yaml)
#### БД
kubectl apply -f [postgresql_prod.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.1%20%D0%BA%D0%BE%D0%BD%D1%82%D0%B5%D0%B9%D0%BD%D0%B5%D1%80%D1%8B%2C%20%D0%BF%D0%BE%D0%B4%D1%8B%2C%20deployment%2C%20statefulset%2C%20services%2C%20endpoints/postgresql_prod.yaml)
```
Every 2.0s: kubectl get po --all-namespaces                                                                                   node1: Mon Aug  9 15:09:47 2021

NAMESPACE     NAME                                       READY   STATUS             RESTARTS   AGE
default       front-back-5b8f5b9c5b-98x5t                2/2     Running            5          4h3m
default       nginx-deployment-66b6c48dd5-fm7wp          1/1     Running            1          4h9m
default       postgres-0                                 1/1     Running            1          112m
default       postgresql-db-0                            0/1     CrashLoopBackOff   5          5m14s
default       product-be-56f584c9b7-b222q                1/1     Running            1          114m
default       product-be-56f584c9b7-ffntb                1/1     Running            1          114m
default       product-fe-56946dbfdc-k8fpp                1/1     Running            1          118m
kube-system   calico-kube-controllers-5b4d7b4594-bjsn2   0/1     Shutdown           0          6d1h
kube-system   calico-kube-controllers-5b4d7b4594-r9kq8   1/1     Running            6          26h
kube-system   calico-node-vvcc5                          1/1     Running            126        6d1h
kube-system   coredns-8474476ff8-dkbdn                   1/1     Running            7          6d1h
kube-system   coredns-8474476ff8-x762p                   0/1     Pending            0          6d1h
kube-system   dns-autoscaler-7df78bfcfb-87bsm            1/1     Running            7          6d1h
kube-system   kube-apiserver-node1                       1/1     Running            8          6d1h
kube-system   kube-controller-manager-node1              1/1     Running            8          6d1h
kube-system   kube-proxy-lbqp6                           1/1     Running            8          6d1h
kube-system   kube-scheduler-node1                       1/1     Running            8          6d1h
kube-system   nodelocaldns-cfp6v                         1/1     Running            8          6d1h
policy-demo   nginx-6799fc88d8-qw9h5                     1/1     Running            1          5h4m
```
