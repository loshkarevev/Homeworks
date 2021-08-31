##### Создайте модуль
```
root@node1:~# kubectl apply -f example-security-context.yml
pod/security-context-demo created
```
##### Проверьте установленные настройки внутри контейнера
```
root@node1:~# kubectl logs security-context-demo
uid=1000 gid=3000 groups=3000
```
