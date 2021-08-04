## Решение
1. При помощи [Vagrantfile](https://github.com/loshkarevev/Homeworks/blob/main/12.4%20%D0%A0%D0%B0%D0%B7%D0%B2%D0%B5%D1%80%D1%82%D1%8B%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%20%D0%BA%D0%BB%D0%B0%D1%81%D1%82%D0%B5%D1%80%D0%B0%20%D0%BD%D0%B0%20%D1%81%D0%BE%D0%B1%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D1%8B%D1%85%20%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0%D1%85%2C%20%D0%BB%D0%B5%D0%BA%D1%86%D0%B8%D1%8F%202/Vagrantfile) создал 5 виртуалок;
2. Настроил [ssh](https://myadventuresincoding.wordpress.com/2011/12/22/linux-how-to-ssh-between-two-linux-computers-without-needing-a-password/) между ними;
3. По [этой](https://github.com/kubernetes-sigs/kubespray) инструкции сформировал [hosts.yaml](https://github.com/loshkarevev/Homeworks/blob/main/12.4%20%D0%A0%D0%B0%D0%B7%D0%B2%D0%B5%D1%80%D1%82%D1%8B%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%20%D0%BA%D0%BB%D0%B0%D1%81%D1%82%D0%B5%D1%80%D0%B0%20%D0%BD%D0%B0%20%D1%81%D0%BE%D0%B1%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D1%8B%D1%85%20%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0%D1%85%2C%20%D0%BB%D0%B5%D0%BA%D1%86%D0%B8%D1%8F%202/hosts.yaml);
4. В файле group_vars/etcd.yml поменял параметр на etcd_deployment_type: containerd;
5. Запустил команду ansible-playbook -i inventory/mycluster/hosts.yaml  --become --become-user=root cluster.yml.

В процессе выполнения запроса возникали различные ошибки, но все их поправил. Итого кластер не получилось поднять, т.к. мой ноутбук не справился (i5 восьмого поколения, 8Гб RAM, SSD) и виртуал бокс свалился с ошибкой.


По [другой](https://rebrainme.com/blog/kubernetes/sozdanie-klastera-kubernetes-na-vps-s-pomoshhyu-kubespray/) инструкции также пробовал, получилось создать [такой](https://github.com/loshkarevev/Homeworks/blob/main/12.4%20%D0%A0%D0%B0%D0%B7%D0%B2%D0%B5%D1%80%D1%82%D1%8B%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%20%D0%BA%D0%BB%D0%B0%D1%81%D1%82%D0%B5%D1%80%D0%B0%20%D0%BD%D0%B0%20%D1%81%D0%BE%D0%B1%D1%81%D1%82%D0%B2%D0%B5%D0%BD%D0%BD%D1%8B%D1%85%20%D1%81%D0%B5%D1%80%D0%B2%D0%B5%D1%80%D0%B0%D1%85%2C%20%D0%BB%D0%B5%D0%BA%D1%86%D0%B8%D1%8F%202/hosts.ini) конфиг. 
