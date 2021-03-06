<h1> Методология разработки ПО</h2>
<h2> Лабораторная работа №2: создание кластера Kubernetes и деплой приложения 
<h4> Сафонов Егор Игоревич, Группа 3mac2001</h4>
<h4>Цель лабораторной работы</h4> 
Знакомство с кластерной архитектурой на примере kubernetes, а также деплоем приложения в кластер.
Для деплоя нашего приложения напишем манифесты для kubernetes

*Манифест deploy.yaml*
<pre>
 <code class = "yaml">
 apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: my-deployment
  spec:
    replicas: 2
    selector:
        matchLabels:
          app: my-app
    strategy:
      rollingUpdate:
        maxSurge: 1
        maxUnavailable: 1
      type: RollingUpdate
    template:
      metadata:
        labels:
          app: my-app
      spec:
        containers:
          - image: myapi:latest
            imagePullPolicy: Never 
            name: myapi
            ports:
              - containerPort: 8080
        hostAliases:
        - ip: "192.168.49.1" # The IP of your VM
          hostnames:
          - postgres.local</code> 
</pre>

*Манифест service.yaml*

<pre>
<code class = "yaml">apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  ports:
    - nodePort: 31317
      port: 8080
      protocol: TCP
      targetPort: 8080
  selector:
    app: my-app</code>
</pre>
*Скриншот с запущенными подами приложения в kubernetes*
![alt text](p.5.3.jpg "minikube показывает что все поды приложения запущены и работают")

*Скриншот с отражением этих же самых подов в UI интерфейсе kubernetes* 
![alt text](p.5.5.jpg "Ui интерфейс для более наглядного и удобного мониторинга за нашими подами")
