# Домашнее задание к занятию «Хранение в K8s. Часть 1»
- Цель задания: В тестовой среде Kubernetes нужно обеспечить обмен файлами между контейнерам пода и доступ к логам ноды.
## Задание 1
- Что нужно сделать
- Создать Deployment приложения, состоящего из двух контейнеров и обменивающихся данными. [data-share]()
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
