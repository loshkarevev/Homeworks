##### Запустить модуль Vault конфигураций через утилиту kubectl в установленном minikube
```
root@vagrant:~# mkdir 14.2
root@vagrant:~# cd 14.2/
root@vagrant:~/14.2# nano vault-pod.yml
root@vagrant:~/14.2# kubectl apply -f 14.2/vault-pod.yml
pod/14.2-netology-vault created
```
##### Получить значение внутреннего IP пода
```
root@vagrant:~# kubectl get pod 14.2-netology-vault -o json | jq -c '.status.podIPs'
[{"ip":"172.17.0.7"}]
```
##### Запустить второй модуль для использования в качестве клиента
```
root@vagrant:~# kubectl run -i --tty fedora --image=fedora --restart=Never -- sh
If you don't see a command prompt, try pressing enter.
sh-5.1#
```
##### Установить дополнительные пакеты
```
dnf -y install pip
pip install hvac
```
##### Запустить интепретатор Python и выполнить следующий код, предварительно поменяв IP и токен
```

```
