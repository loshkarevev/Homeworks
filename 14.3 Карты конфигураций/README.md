##### Как создать карту конфигураций?
```
root@vagrant:~# mkdir 14.3
root@vagrant:~# cd 14.3/
root@vagrant:~/14.3# nano nginx.conf
root@vagrant:~/14.3# kubectl create configmap nginx-config --from-file=nginx.conf
configmap/nginx-config created
root@vagrant:~/14.3# kubectl create configmap domain --from-literal=name=netology.ru
configmap/domain created
```
##### Как просмотреть список карт конфигураций?
```
root@vagrant:~/14.3# kubectl get configmaps
NAME               DATA   AGE
domain             1      72s
kube-root-ca.crt   1      4d19h
nginx-config       1      92s
root@vagrant:~/14.3# kubectl get configmap
NAME               DATA   AGE
domain             1      87s
kube-root-ca.crt   1      4d19h
nginx-config       1      107s
```
##### Как просмотреть карту конфигурации?
```
root@vagrant:~/14.3# kubectl get configmap nginx-config
NAME           DATA   AGE
nginx-config   1      2m43s
root@vagrant:~/14.3# kubectl describe configmap domain
Name:         domain
Namespace:    default
Labels:       <none>
Annotations:  <none>

Data
====
name:
----
netology.ru

BinaryData
====

Events:  <none>
```
##### Как получить информацию в формате YAML и/или JSON?
```
root@vagrant:~/14.3# kubectl get configmap nginx-config -o yaml
apiVersion: v1
data:
  nginx.conf: |
    server {
        listen 80;
        server_name  netology.ru www.netology.ru;
        access_log  /var/log/nginx/domains/netology.ru-access.log  main;
        error_log   /var/log/nginx/domains/netology.ru-error.log info;
        location / {
            include proxy_params;
            proxy_pass http://10.10.10.10:8080/;
        }
    }
kind: ConfigMap
metadata:
  creationTimestamp: "2021-08-28T11:17:27Z"
  name: nginx-config
  namespace: default
  resourceVersion: "30908"
  uid: e36e8b15-2464-49d7-8aff-d53ed4474dc0
root@vagrant:~/14.3# kubectl get configmap domain -o json
{
    "apiVersion": "v1",
    "data": {
        "name": "netology.ru"
    },
    "kind": "ConfigMap",
    "metadata": {
        "creationTimestamp": "2021-08-28T11:17:47Z",
        "name": "domain",
        "namespace": "default",
        "resourceVersion": "30922",
        "uid": "6f5d0350-9abc-4509-8b1c-3e9753a4d93a"
    }
}
```
##### Как выгрузить карту конфигурации и сохранить его в файл?
```
root@vagrant:~/14.3# kubectl get configmaps -o json > configmaps.json
root@vagrant:~/14.3# kubectl get configmap nginx-config -o yaml > nginx-config.yml
```
##### Как удалить карту конфигурации?
```
root@vagrant:~/14.3# kubectl delete configmap nginx-config
configmap "nginx-config" deleted
```
##### Как загрузить карту конфигурации из файла?
```
root@vagrant:~/14.3# kubectl apply -f nginx-config.yml
configmap/nginx-config created
```
