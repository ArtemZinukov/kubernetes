# Разворачиваем сайт в Minikube
Набор манифестов для запуска `Django-проекта` в кластере.

### Как поднять кластер Kubernetes в Minikube
Установить [kubectl](https://kubernetes.io/ru/docs/tasks/tools/install-kubectl/) и [minikube](https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download).  

После запустить команду:

```
minikube start --vm-driver=<драйвер>
```
### Как развернуть сайт
Развернуть PostgresQL в кластере с помощью [Helm](https://helm.sh/) и [Helm Chart for PostgreSQL](https://artifacthub.io/packages/helm/bitnami/postgresql).

Скачать репозиторий с кодом и перейти в директорию:

```
cd kubernetes
```

Создать файл с названием app-secret.yaml с данными:

```commandline
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
  namespace: default
type: Opaque
stringData:
  SECRET_KEY: 'ваш ключ Django'
  DEBUG: 'False' либо 'True'
  ALLOWED_HOSTS: 'записать сюда ваш хост'
  DATABASE_URL: 'данные для бд'
```

Последовательно выполнить команды:

```
kubectl apply -f app-secret.yaml
```

```
kubectl apply -f deployment.yaml
```

```
kubectl apply -f service-django-app.yaml
```

```
kubectl apply -f ingress-hosts.yaml
```

```
kubectl apply -f migrate.yaml
```

```
kubectl apply -f clearsessions.yaml
```

Сайт будет доступен по адресу [star-burger.test](http://star-burger.test)

