## Решение
1. Устанавливаем calico через kubespray
``` git clone https://github.com/kubernetes-sigs/kubespray.git
cd kubespray/
pip3 install -r requirements.txt
ansible-playbook -i inventory/local/hosts.ini cluster.yml
kubectl get po --all-namespaces
```
2. Убедились, что калико запустился ![get po.png](https://github.com/loshkarevev/Homeworks/blob/main/12.5%20%D0%A1%D0%B5%D1%82%D0%B5%D0%B2%D1%8B%D0%B5%20%D1%80%D0%B5%D1%88%D0%B5%D0%BD%D0%B8%D1%8F%20CNI/get%20po.png)
3. По [этой](https://docs.projectcalico.org/security/tutorials/kubernetes-policy-basic) ссылке настраиваем политику доступа. В тело решения решил не копипастить, делал все команды один в один.
Результат выполнения когда есть доступ:
```root@node1:~# kubectl run --namespace=policy-demo access --rm -ti --image busybox /bin/sh
If you don't see a command prompt, try pressing enter.
/ # wget -q --timeout=5 nginx -O -
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
/ # 
```
5. Результат выполнения когда нет доступа:
```root@node1:~# kubectl run --namespace=policy-demo cant-access --rm -ti --image busybox /bin/sh
If you don't see a command prompt, try pressing enter.
/ # wget -q --timeout=5 nginx -O -
wget: download timed out```
6. Под также отображается ![get po+nginx.png](https://github.com/loshkarevev/Homeworks/blob/main/12.5%20%D0%A1%D0%B5%D1%82%D0%B5%D0%B2%D1%8B%D0%B5%20%D1%80%D0%B5%D1%88%D0%B5%D0%BD%D0%B8%D1%8F%20CNI/get%20po%2Bnginx.png)
7. Вывод команд, который сказали сделать на лекции:
```root@node1:~# calicoctl get nodes
NAME
node1

root@node1:~# calicoctl get ipPool
NAME           CIDR             SELECTOR
default-pool   10.233.64.0/18   all()
```
