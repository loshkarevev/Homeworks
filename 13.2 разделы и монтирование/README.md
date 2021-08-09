## Решение
Установил Helm, в качестве примера создания pvc выдал это:
```
kind: PersistentVolumeClaim
    apiVersion: v1
    metadata:
      name: test-dynamic-volume-claim
    spec:
      storageClassName: "nfs"
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 100Mi
```
### Задание 1: подключить для тестового конфига общую папку
Взял [пример](https://github.com/loshkarevev/Homeworks/blob/main/13.2%20%D1%80%D0%B0%D0%B7%D0%B4%D0%B5%D0%BB%D1%8B%20%D0%B8%20%D0%BC%D0%BE%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5/pod-int-volumes.yaml) из лекции. Можно было взять с [сайта](https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/).

Выполнил kubectl apply -f pod-int-volumes.yaml

Пишем в один под данные:
```
root@node1:~/kubespray# kubectl exec pod-int-volumes -c busybox -- ls -la /tmp/cache
total 12
drwxrwxrwx    2 root     root          4096 Aug  9 16:34 .
drwxrwxrwt    1 root     root          4096 Aug  9 16:26 ..
-rw-r--r--    1 root     root             5 Aug  9 16:34 test.txt
```
На втором поде читаем:
```
root@node1:~/kubespray# kubectl exec pod-int-volumes -c nginx -- ls -la /static
total 12
drwxrwxrwx 2 root root 4096 Aug  9 16:34 .
drwxr-xr-x 1 root root 4096 Aug  9 16:26 ..
-rw-r--r-- 1 root root    5 Aug  9 16:34 test.txt
```
### Задание 2: подключить общую папку для прода
Сделал [pvс3.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.2%20%D1%80%D0%B0%D0%B7%D0%B4%D0%B5%D0%BB%D1%8B%20%D0%B8%20%D0%BC%D0%BE%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5/pvc3.yaml)

Создались и запустились [back2.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.2%20%D1%80%D0%B0%D0%B7%D0%B4%D0%B5%D0%BB%D1%8B%20%D0%B8%20%D0%BC%D0%BE%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5/back2.yaml), [front2.yaml](https://github.com/loshkarevev/Homeworks/blob/main/13.2%20%D1%80%D0%B0%D0%B7%D0%B4%D0%B5%D0%BB%D1%8B%20%D0%B8%20%D0%BC%D0%BE%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5/front2.yaml) без pv.
```
Every 2.0s: kubectl get po --all-namespaces                                                                                   node1: Mon Aug  9 18:20:20 2021

NAMESPACE     NAME                                       READY   STATUS             RESTARTS   AGE
default       front-back-5b8f5b9c5b-98x5t                2/2     Running            8          7h14m
default       nfs-server-nfs-server-provisioner-0        1/1     Running            0          3h5m
default       nginx-deployment-66b6c48dd5-fm7wp          1/1     Running            1          7h20m
default       pod-int-volumes                            2/2     Running            1          113m
default       postgres-0                                 1/1     Running            1          5h2m
default       postgresql-db-0                            0/1     CrashLoopBackOff   42         3h15m
default       product-be-56f584c9b7-b222q                1/1     Running            1          5h4m
default       product-be-56f584c9b7-ffntb                1/1     Running            1          5h4m
default       product-be2-6db8586945-7cx6d               1/1     Running            0          35m
default       product-be2-6db8586945-ch99k               1/1     Running            0          35m
default       product-fe-56946dbfdc-k8fpp                1/1     Running            1          5h8m
default       product-fe2-59b799b66f-b29nb               1/1     Running            0          32m
kube-system   calico-kube-controllers-5b4d7b4594-bjsn2   0/1     Shutdown           0          6d4h
kube-system   calico-kube-controllers-5b4d7b4594-r9kq8   1/1     Running            6          29h
kube-system   calico-node-vvcc5                          1/1     Running            130        6d4h
kube-system   coredns-8474476ff8-dkbdn                   1/1     Running            7          6d4h
```
