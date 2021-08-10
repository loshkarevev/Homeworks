## Решение
### 1. Проверить работоспособность каждого компонента
Проверку осуществляем на ранее созданных приложениях
![get po.png](https://github.com/loshkarevev/Homeworks/blob/main/13.3%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20kubectl/get%20po.png)

Проверяем доступ к фронту, через port-forward
![product-fe.png](https://github.com/loshkarevev/Homeworks/blob/main/13.3%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20kubectl/product-fe.png)

Проверяем доступ к бэку, через port-forward
![product-be.png](https://github.com/loshkarevev/Homeworks/blob/main/13.3%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20kubectl/product-be.png)

Проверяем доступ к БД, через port-forward
![DB.png](https://github.com/loshkarevev/Homeworks/blob/main/13.3%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20kubectl/DB.png)

Проверяем доступ к фронту, бэку, БД через exec
![exec.png](https://github.com/loshkarevev/Homeworks/blob/main/13.3%20%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D0%B0%20%D1%81%20kubectl/exec.png)
### 2. Ручное масштабирование
