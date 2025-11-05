
# Домашнее задание к занятию «Установка Kubernetes» - Морозов Александр

### Цель задания

Установить кластер K8s.

### Чеклист готовности к домашнему заданию

1. Развёрнутые ВМ с ОС Ubuntu 20.04-lts.


### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. [Инструкция по установке kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/).
2. [Документация kubespray](https://kubespray.io/).

-----

### Задание 1. Установить кластер k8s с 1 master node

1. Подготовка работы кластера из 5 нод: 1 мастер и 4 рабочие ноды.
2. В качестве CRI — containerd.
3. Запуск etcd производить на мастере.
4. Способ установки выбрать самостоятельно.

### Ответ:
Устанавливаем кластер K8S с помощью git clone Kubespray

1. Подготавливаем 5 ВМ, 1 - master node, 4 - worker node
![alt text](https://github.com/Mars12121/kuber-homeworks_3.2/blob/main/img/1.png)

2. Клонируем репозиторий на ВМ с которой будет запускаться ansible-playbook
```
git clone https://github.com/kubernetes-sigs/kubespray
```

3. Копирование примера в папку с вашей конфигурацией
```
cp -rfp inventory/sample inventory/mycluster
```

4. Заполняем inventory/mycluster/inventory.ini
```
[kube_control_plane]
node1 ansible_host=84.201.171.5       ip=10.130.0.26 etcd_member_name=etcd

[etcd:children]
kube_control_plane

[kube_node]
node2 ansible_host=158.160.173.151   ip=10.130.0.30
node3 ansible_host=158.160.196.66    ip=10.130.0.36
node4 ansible_host=158.160.175.107   ip=10.130.0.27
node5 ansible_host=158.160.182.140   ip=10.130.0.14
```

5. Устанавливаем зависимости на ВМ с которой будет запускаться ansible-playbook
```
sudo apt update
sudo apt install python3.12-venv python3.12-dev python3-pip -y
```

6. Создаем виртуальное окружение и устанавливаем ansible 2.17.5
```
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -r requirements.txt
pip install ansible-core==2.17.5
```

7. Запускаем ansible-playbook c ssh ключем
```
ansible-playbook -i inventory/mycluster/inventory.ini cluster.yml -b -v --key-file ~/.ssh/id_ed25519
```
![alt text](https://github.com/Mars12121/kuber-homeworks_3.2/blob/main/img/2.png)

8. Заходим на master node и проверяем состояние нод
![alt text](https://github.com/Mars12121/kuber-homeworks_3.2/blob/main/img/3.png)



## Дополнительные задания (со звёздочкой)

**Настоятельно рекомендуем выполнять все задания под звёздочкой.** Их выполнение поможет глубже разобраться в материале.   
Задания под звёздочкой необязательные к выполнению и не повлияют на получение зачёта по этому домашнему заданию. 

------
### Задание 2*. Установить HA кластер

1. Установить кластер в режиме HA.
2. Использовать нечётное количество Master-node.
3. Для cluster ip использовать keepalived или другой способ.

### Правила приёма работы

1. Домашняя работа оформляется в своем Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода необходимых команд `kubectl get nodes`, а также скриншоты результатов.
3. Репозиторий должен содержать тексты манифестов или ссылки на них в файле README.md.
