<h1> Методология разработки ПО</h2>
<h2> Лабораторная работа №2: создание кластера Kubernetes и деплой приложения 
<h4> Сафонов Егор Игоревич, Группа 3mac2001</h4>
<h4>Цель лабораторной работы</h4> 
Знакомство с кластерной архитектурой на примере kubernetes, а также деплоем приложения в кластер.

 ``` apiVersion: apps/v1
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
            # https://medium.com/bb-tutorials-and-thoughts/how-to-use-own-local-doker-images-with-minikube-2c1ed0b0968
            imagePullPolicy: Never 
            name: myapi
            ports:
              - containerPort: 8080
        hostAliases:
        - ip: "192.168.49.1" # The IP of your VM
          hostnames:
          - postgres.local 
