# Домашнее задание к занятию "14.4 Сервис-аккаунты"

## Задача 1: Работа с сервис-аккаунтами через утилиту kubectl в установленном minikube

### Ответ:

Создадим новый namaspace и сервис-аккаунт, посмотрим список сервис-аккаунтов :
```
vagrant@vagrant:~$ kubectl create ns netology
namespace/netology created
vagrant@vagrant:~$ kubectl config set-context --current --namespace=netology
Context "minikube" modified.
vagrant@vagrant:~$ kubectl create serviceaccount netology
serviceaccount/netology created
vagrant@vagrant:~$ kubectl get serviceaccounts
NAME       SECRETS   AGE
default    0         9m53s
netology   0         39s
vagrant@vagrant:~$ kubectl get serviceaccount
NAME       SECRETS   AGE
default    0         9m54s
netology   0         40s
vagrant@vagrant:~$
```

Получим информацию в форматах YAML и/или JSON:
```
vagrant@vagrant:~$ kubectl get serviceaccount netology -o yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  creationTimestamp: "2023-02-04T17:56:39Z"
  name: netology
  namespace: netology
  resourceVersion: "146141"
  uid: bd303502-481b-4d62-a402-b9456b1ef1ba
vagrant@vagrant:~$ kubectl get serviceaccount default -o json
{
    "apiVersion": "v1",
    "kind": "ServiceAccount",
    "metadata": {
        "creationTimestamp": "2023-02-04T17:47:25Z",
        "name": "default",
        "namespace": "netology",
        "resourceVersion": "145751",
        "uid": "b462d091-77d6-4b27-a2d9-cc4495261d64"
    }
}
vagrant@vagrant:~$
```

Выгрузим сервис-акаунты и сохраним в файл:
```
vagrant@vagrant:~$ kubectl get serviceaccounts -o json > serviceaccounts.json
vagrant@vagrant:~$ kubectl get serviceaccount netology -o yaml > netology.yml
vagrant@vagrant:~$ cat serviceaccounts.json
{
    "apiVersion": "v1",
    "items": [
        {
            "apiVersion": "v1",
            "kind": "ServiceAccount",
            "metadata": {
                "creationTimestamp": "2023-02-04T17:47:25Z",
                "name": "default",
                "namespace": "netology",
                "resourceVersion": "145751",
                "uid": "b462d091-77d6-4b27-a2d9-cc4495261d64"
            }
        },
        {
            "apiVersion": "v1",
            "kind": "ServiceAccount",
            "metadata": {
                "creationTimestamp": "2023-02-04T17:56:39Z",
                "name": "netology",
                "namespace": "netology",
                "resourceVersion": "146141",
                "uid": "bd303502-481b-4d62-a402-b9456b1ef1ba"
            }
        }
    ],
    "kind": "List",
    "metadata": {
        "resourceVersion": ""
    }
}
vagrant@vagrant:~$ cat netology.yml
apiVersion: v1
kind: ServiceAccount
metadata:
  creationTimestamp: "2023-02-04T17:56:39Z"
  name: netology
  namespace: netology
  resourceVersion: "146141"
  uid: bd303502-481b-4d62-a402-b9456b1ef1ba
vagrant@vagrant:~$
```

Удалим сервис-акаунт:
```
vagrant@vagrant:~$ kubectl delete serviceaccount netology
serviceaccount "netology" deleted
vagrant@vagrant:~$ kubectl get serviceaccount
NAME      SECRETS   AGE
default   0         14m
vagrant@vagrant:~$ kubectl get serviceaccounts
NAME      SECRETS   AGE
default   0         14m
vagrant@vagrant:~$
```

Загрузим сервис-акаунт из файла:
```
vagrant@vagrant:~$ kubectl apply -f netology.yml
serviceaccount/netology created
vagrant@vagrant:~$ kubectl get serviceaccount
NAME       SECRETS   AGE
default    0         19m
netology   0         3s
vagrant@vagrant:~$ kubectl get serviceaccounts
NAME       SECRETS   AGE
default    0         19m
netology   0         5s
vagrant@vagrant:~$
```
