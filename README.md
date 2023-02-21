
## Задание D2.4.1

Работа производилась на локальной ВМ под управление virtualbox с Ubuntu 22.04 и docker 20.10  

---

Порядок запуска кластера:  

1. Скачать бинарник minikube:  
```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```
2. Установить:  
```
install ./minikube-linux-amd64 /usr/local/bin/minikube
```
3. Добавить текущего пользователя в группу docker:  
```
sudo usermod -aG docker esm
```
4. Перелогиниваемся для обновления членсва в группах
5. Запустить кластер (я создаю кластер с 3-мя узлами, т.к. на ВМ недостаточно ресурсов для 5 как по заданию) и ограничить оперативную пямять, выделяемую кластеру, минимально возможным значением:  
```
minikube start -n 3 --memory 1800
```

---

## Задание D2.4.2

---

По своей сути Docker является комплексной средой деплоя контейнеров и предоставляет конечному пользователю удобный UI или командную строку. Пользователь управляет контейнерами через docker engine, который в свою очередь в качестве среды выполения использует containerd, поддержка, в том числе которого, остается в k8s. Для разработчиков это означает, что хотя и потребуется изменить среду выполнения контейнеров, сам по себе Docker все еще остается полезным. Например, по прежнему можно собирать образы из Dockerfile, а Kubernetes будет выполнять их в containerd или другой, совместимой с Open Container Initiative образами контейнеров, среде.  
Почему принято такое решение? Разработчики объясняют это тем, что k8s не может напрямую взаимодействовать с Docker Engine для доступа к среде containerd, в результате приходится использовать вспомогательный инструмент Dockershim, который, как утверждают разработчики, является дополнительной точкой отказа и который будет удален из агентов Kubelet.  
Исходя из всего вышенаписанного, именно поэтому kind утверждает, что образы, **созданные** в docker, по прежнему будут работать в средах выполнения, поддерживаемых k8s. 

Список полезных ссылок:  
* https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/
* https://blog.purestorage.com/purely-informational/containerd-vs-docker-whats-the-difference/
* https://www.knowledgehut.com/blog/devops/docker-vs-containerd
* https://github.com/SkillfactoryCoding/DEVOPS-BasicKubernates-kubernetes/blob/master/CHANGELOG/CHANGELOG-1.20.md#dockershim-deprecation
* https://kubernetes.io/blog/2020/12/02/dockershim-faq/

---
