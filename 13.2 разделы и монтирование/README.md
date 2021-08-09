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
Взял [пример](https://github.com/loshkarevev/Homeworks/tree/main/13.2%20%D1%80%D0%B0%D0%B7%D0%B4%D0%B5%D0%BB%D1%8B%20%D0%B8%20%D0%BC%D0%BE%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5) из лекции.
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
