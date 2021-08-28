##### Запустить модуль Vault конфигураций через утилиту kubectl в установленном minikube
```
root@vagrant:~# mkdir 14.2
root@vagrant:~# cd 14.2/
root@vagrant:~/14.2# nano vault-pod.yml
root@vagrant:~/14.2# cd ..
root@vagrant:~# kubectl apply -f 14.2/vault-pod.yml
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
sh-5.1# python3.9
Python 3.9.6 (default, Jul 16 2021, 00:00:00)
[GCC 11.1.1 20210531 (Red Hat 11.1.1-3)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import hvac
>>> client = hvac.Client(
...     url='http://172.17.0.7:8200',
...     token='aiphohTaa0eeHei'
... )
>>> client.is_authenticated()
True
>>> client.secrets.kv.v2.create_or_update_secret(
...     path='hvac',
...     secret=dict(netology='Big secret!!!'),
... )
{'request_id': 'c6560551-4f49-d9e8-b90c-8db20648085a', 'lease_id': '', 'renewable': False, 'lease_duration': 0, 'data': {'created_time': '2021-08-28T10:10:10.244191361Z', 'deletion_time': '', 'destroyed': False, 'version': 1}, 'wrap_info': None, 'warnings': None, 'auth': None}
>>> client.secrets.kv.v2.read_secret_version(
...     path='hvac',
... )
{'request_id': 'd284703c-2142-d73b-a04f-6bc91fc28cb5', 'lease_id': '', 'renewable': False, 'lease_duration': 0, 'data': {'data': {'netology': 'Big secret!!!'}, 'metadata': {'created_time': '2021-08-28T10:10:10.244191361Z', 'deletion_time': '', 'destroyed': False, 'version': 1}}, 'wrap_info': None, 'warnings': None, 'auth': None}
>>>
```
