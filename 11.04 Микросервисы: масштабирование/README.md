## Решение
Задание выполнено на основании [этой](https://mcs.mail.ru/blog/sravnenie-kubernetes-s-drugimi-resheniyami) статьи.

| Параметр\Название | Kubernetes | Docker Swarm | Nomand | Apache Mesos (+Marathon, Aurora) |
|---|---|---|---|---|
| Поддержка контейнеров | + | + | + | + |
| Обеспечивать обнаружение сервисов и маршрутизацию запросов | + | + | - | + |
| Обеспечивать возможность горизонтального масштабирования | + | + | + | + |
| Обеспечивать возможность автоматического масштабирования | + | - | + | + |
| Обеспечивать явное разделение ресурсов доступных извне и внутри системы | + | - | + | + |
| Обеспечивать возможность конфигурировать приложения с помощью переменных среды | + | + | + | + |

Решения представленные в таблице во многом похожи. Выбор того или иного решения зависит от условий, в которых мы находимся.

Если говорить о том, что это крупная компания, то помимо вышеперечисленных критериев, я бы еще ориентировался кол-во фич, безопастность и на популярность сервиса. т.к. если сервис популярный, значит максимально выловлены все баги и сервис продолжит развиваться.

**Как итог: я бы остановился на k8s**.
