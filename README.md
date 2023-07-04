# k8s
### 1. Ответы на вопросы.
#### 1.1 Что такое k8s? 
* "k" 8 символов и "s".
* k8s или "Kubernetes" — это открытое программное обеспечение для автоматизации развёртывания, масштабирования и управления контейнеризированными приложениями.
* А также развертывание хранилища, мониторинг сервисов, обеспечение отказоустойчивости сервисов и приложений, безперебойной поставки новых версий приложений и балансировка ресурсов.
#### 1.2 В чём преимущество контейнеризации над виртуализацией?
* Отказоустйчивость
* Бесперебойная поставка нового ПО.
* Распеделение физических ресурсов
* Легкость маштабирования
#### 1.3 В чём состоит принцип самоконтроля k8s?
* В контроле работоспособности контейнеров и при необходимости их перезапускает, меняет или отключает.
#### 1.4 Как вы думаете, зачем Вам понимать принципы деплоя в k8s? 
* Для развертвывания контейнеров микросервисов.
#### 1.5 Какое из средств управления секретами наиболее распространено в использовании совместно с k8s?
* Secret
#### 1.6 Какие типы нод есть в k8s, каковы их базовые функции?
* Master и Worker
* Master - api, контроль, хранилище k8s.
* Worker - агент, сеть, среда для контейнеро.
### 2. Манифест
* apiVersion – v1 
* название – netology-ml
* внутри него сервер приложений tomcat версии 8.5.69 с портом 8080
* наличие 2 реплик
* использование стратегии rollingupdate
#### 2.1 [pod.yaml](https://github.com/MSRaith/k8s_manifest/blob/main/pod.yaml)
```
apiVersion: v1
kind: Pod
metadata:
  name: pod-netology-ml
spec:
  containers:
  - name: tomcat
    image: tomcat:8.5.69
    ports:
    - containerPort: 8080 
  ```
#### 2.2 [replicaset.yaml](https://github.com/MSRaith/k8s_manifest/blob/main/replicaset.ayml)
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: rep-netology-ml 
spec:
  replicas: 2
  selector:
    matchLabels:
      app: netology-ml 
  template:
    metadata:
      labels:
        app: netology-ml
    spec:
      containers:
      - name: tomcat
        image: tomcat:8.5.69
```
#### 2.3 [deployment.yaml](https://github.com/MSRaith/k8s_manifest/blob/main/deployment.yaml)
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deploy-netology-ml
spec:
  replicas: 2
  selector:
    matchLabels:
      app: netology-ml
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: rollingUpdate
  template:
    metadata:
      labels:
        app: netology-ml
    spec:
      containers:
      - name: tomcat
        image: tomcat:8.5.69
        ports:
        - containerPort: 8080
```
#### 2.3 [configmap.yaml](https://github.com/MSRaith/k8s_manifest/blob/main/configmap.yaml)
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: map-netology-ml 
data:
  default.conf: |
    server {
      listen 8080 default_server;
      server_name_;
      default_type text/plain;
      location / {
        return 200 'hostname\n'
      }
    }
```
