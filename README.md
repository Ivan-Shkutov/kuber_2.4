## Домашнее задание к занятию «Helm»

#### Цель задания

В тестовой среде Kubernetes необходимо установить и обновить приложения с помощью Helm.

- - - - - 

### Задание 1. Подготовить Helm-чарт для приложения

  1. Необходимо упаковать приложение в чарт для деплоя в разные окружения.

  2. Каждый компонент приложения деплоится отдельным deployment’ом или statefulset’ом.

  3. В переменных чарта измените образ приложения для изменения версии.


###   Решение:

  1. Устанавливаем Helm-чарт

    helm install myapp
    kubectl get pods
    kubectl get deployments

  2. Обновляем версии образов, новые теги

    helm upgrade myapp . --set apache.image.tag=2.4.59
    helm upgrade myapp . --set nginx.image.tag=1.25.2
    kubectl get pods

  3. Проверяем Deployment и Pod

    kubectl get deployments
    kubectl describe deployment myapp-myapp-apache
    kubectl describe deployment myapp-myapp-nginx
    kubectl get pods -o wide


![1](https://github.com/Ivan-Shkutov/kuber_2.4/blob/main/1.png)

![2](https://github.com/Ivan-Shkutov/kuber_2.4/blob/main/2.png)

![3](https://github.com/Ivan-Shkutov/kuber_2.4/blob/main/3.png)

![4](https://github.com/Ivan-Shkutov/kuber_2.4/blob/main/4.png)

- - - - - 
- - - - - 

### Задание 2. Запустить две версии в разных неймспейсах

  1. Подготовив чарт, необходимо его проверить. Запуститe несколько копий приложения.

  2. Одну версию в namespace=app1, вторую версию в том же неймспейсе, третью версию в namespace=app2.

  3. Продемонстрируйте результат.

###   Решение:

  1. Создаём неймспейсы

    kubectl create namespace app1
    kubectl create namespace app2
    kubectl get namespaces
  
  2. Устанавливаем первую версию в namespace app1

    helm install myapp-v1 . --namespace app1 --set apache.image.tag=2.4.59 --set nginx.image.tag=1.25.2
    kubectl get pods -n app1
    kubectl get deployments -n app1

  3. Устанавливаем вторую версию в namespace app1

    helm install myapp-v2 . --namespace app1 --set apache.image.tag=2.4.60 --set nginx.image.tag=1.25.3
    kubectl get pods -n app1
    kubectl get deployments -n app1

  4. Устанавливаем третью версию в namespace app2

    helm install myapp-v2 . --namespace app2 --set apache.image.tag=2.4.61 --set nginx.image.tag=1.25.4
    kubectl get pods -n app2
    kubectl get deployments -n app2

  5. Проверка всех версий

    kubectl get pods -n app1
    kubectl get deployments -n app1
    
    kubectl get pods -n app2
    kubectl get deployments -n app2 


![5](https://github.com/Ivan-Shkutov/kuber_2.4/blob/main/5.png)

![6](https://github.com/Ivan-Shkutov/kuber_2.4/blob/main/6.png)

![7](https://github.com/Ivan-Shkutov/kuber_2.4/blob/main/7.png)

- - - - - 
