# Домашнее задание к занятию «Хранение в K8s. Часть 1»
- Цель задания: В тестовой среде Kubernetes нужно обеспечить обмен файлами между контейнерам пода и доступ к логам ноды.
## Задание 1
- Что нужно сделать
- Создать Deployment приложения, состоящего из двух контейнеров и обменивающихся данными. [data-exchange](https://github.com/EVolgina/kuber2.1/blob/main/shared.yaml)
- Создать Deployment приложения, состоящего из контейнеров busybox и multitool.
- Сделать так, чтобы busybox писал каждые пять секунд в некий файл в общей директории.
- Обеспечить возможность чтения файла контейнером multitool.
- Продемонстрировать, что multitool может читать файл, который периодоически обновляется.
- Предоставить манифесты Deployment в решении, а также скриншоты или вывод команды из п. 4.
```
vagrant@vagrant:~/kube/zad6$ kubectl apply -f shared.yaml
deployment.apps/data-exchange created
vagrant@vagrant:~/kube/zad6$ kubectl get pods
NAME                             READY   STATUS    RESTARTS        AGE
backend-7bfd89b7c7-pbqp6         1/1     Running   1 (179m ago)    4h6m
frontend-7766f6c5f8-mkczf        1/1     Running   1 (179m ago)    4h6m
frontend-7766f6c5f8-8t8dv        1/1     Running   1 (179m ago)    4h6m
frontend-7766f6c5f8-xr82f        1/1     Running   1 (179m ago)    4h6m
multitool-7f8c7df657-h2m9s       1/1     Running   6 (179m ago)    25h
my-deployment-c57565bf-nc9n2     2/2     Running   10 (179m ago)   21h
my-deployment-c57565bf-zk65h     2/2     Running   10 (179m ago)   21h
my-deployment-c57565bf-8ffm8     2/2     Running   10 (179m ago)   21h
multitool-pod                    1/1     Running   24 (56m ago)    22h
data-exchange-6ddd7fb7f5-rxg42   2/2     Running   0               20s
vagrant@vagrant:~/kube/zad6$ kubectl get deployment
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
backend         1/1     1            1           4h6m
frontend        3/3     3            3           4h6m
multitool       1/1     1            1           7d3h
my-deployment   3/3     3            3           24h
data-exchange   1/1     1            1           45s
```
```
vagrant@vagrant:~/kube/zad6$ kubectl exec -it data-exchange-6ddd7fb7f5-rxg42 -- cat /shared/data.txt
Defaulted container "busybox" out of: busybox, multitool
Sun Feb 25 11:35:07 UTC 2024
Sun Feb 25 11:35:12 UTC 2024
Sun Feb 25 11:35:17 UTC 2024
Sun Feb 25 11:35:23 UTC 2024
Sun Feb 25 11:35:28 UTC 2024
Sun Feb 25 11:35:33 UTC 2024
Sun Feb 25 11:35:38 UTC 2024
Sun Feb 25 11:35:43 UTC 2024
Sun Feb 25 11:35:48 UTC 2024
Sun Feb 25 11:35:53 UTC 2024
Sun Feb 25 11:35:58 UTC 2024
Sun Feb 25 11:36:03 UTC 2024
Sun Feb 25 11:36:08 UTC 2024
Sun Feb 25 11:36:13 UTC 2024
Sun Feb 25 11:36:18 UTC 2024
Sun Feb 25 11:36:24 UTC 2024
Sun Feb 25 11:36:29 UTC 2024
Sun Feb 25 11:36:34 UTC 2024
Sun Feb 25 11:36:39 UTC 2024
Sun Feb 25 11:36:44 UTC 2024
Sun Feb 25 11:36:49 UTC 2024
Sun Feb 25 11:36:54 UTC 2024
Sun Feb 25 11:36:59 UTC 2024
```
## Задание 2
- Что нужно сделать
- Создать DaemonSet приложения, которое может прочитать логи ноды. [daemonset](https://github.com/EVolgina/kuber2.1/blob/main/daemonset.yaml)
- Создать DaemonSet приложения, состоящего из multitool.
- Обеспечить возможность чтения файла /var/log/syslog кластера MicroK8S.
- Продемонстрировать возможность чтения файла изнутри пода.
- Предоставить манифесты Deployment, а также скриншоты или вывод команды из п. 2.
```
vagrant@vagrant:~/kube/zad6$ kubectl apply -f daemonset.yaml
daemonset.apps/daemonset created
vagrant@vagrant:~/kube/zad6$ kubectl get deployment
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
backend         1/1     1            1           4h18m
frontend        3/3     3            3           4h18m
multitool       1/1     1            1           7d3h
my-deployment   3/3     3            3           24h
data-exchange   1/1     1            1           12m
vagrant@vagrant:~/kube/zad6$ kubectl get pods
NAME                             READY   STATUS    RESTARTS         AGE
backend-7bfd89b7c7-pbqp6         1/1     Running   1 (3h11m ago)    4h18m
frontend-7766f6c5f8-mkczf        1/1     Running   1 (3h11m ago)    4h18m
frontend-7766f6c5f8-8t8dv        1/1     Running   1 (3h11m ago)    4h18m
frontend-7766f6c5f8-xr82f        1/1     Running   1 (3h11m ago)    4h18m
multitool-7f8c7df657-h2m9s       1/1     Running   6 (3h11m ago)    25h
my-deployment-c57565bf-nc9n2     2/2     Running   10 (3h11m ago)   21h
my-deployment-c57565bf-zk65h     2/2     Running   10 (3h11m ago)   21h
my-deployment-c57565bf-8ffm8     2/2     Running   10 (3h11m ago)   21h
data-exchange-6ddd7fb7f5-rxg42   2/2     Running   0                12m
multitool-pod                    1/1     Running   25 (9m3s ago)    22h
daemonset-l869k                  1/1     Running   0                27s
vagrant@vagrant:~/kube/zad6$ kubectl get daemonset  daemonset
NAME        DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
daemonset   1         1         1       1            1           <none>          6m49s
vagrant@vagrant:~/kube/zad6$ kubectl get pods -l app=multitool
NAME                         READY   STATUS    RESTARTS        AGE
multitool-7f8c7df657-h2m9s   1/1     Running   6 (3h18m ago)   26h
daemonset-l869k              1/1     Running   0               7m30s
```
```
vagrant@vagrant:~/kube/zad6$ kubectl describe daemonset daemonset
Name:           daemonset
Selector:       app=multitool
Node-Selector:  <none>
Labels:         <none>
Annotations:    deprecated.daemonset.template.generation: 1
Desired Number of Nodes Scheduled: 1
Current Number of Nodes Scheduled: 1
Number of Nodes Scheduled with Up-to-date Pods: 1
Number of Nodes Scheduled with Available Pods: 1
Number of Nodes Misscheduled: 0
Pods Status:  1 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=multitool
  Containers:
   multitool:
    Image:        wbitt/network-multitool:latest
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:
      /var/log from var-log (rw)
  Volumes:
   var-log:
    Type:          HostPath (bare host directory volume)
    Path:          /var/log
    HostPathType:
Events:            <none>
Запустила, скрин не получется сделать, очень много информации
kubectl exec -it daemonset-l869k -- cat /var/log/syslog
```
