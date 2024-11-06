# Домашнее задание к занятию «Kubernetes. Причины появления. Команда kubectl»

### Цель задания

Для экспериментов и валидации ваших решений вам нужно подготовить тестовую среду для работы с Kubernetes. Оптимальное решение — развернуть на рабочей машине или на отдельной виртуальной машине MicroK8S.

------

### Чеклист готовности к домашнему заданию

1. Личный компьютер с ОС Linux или MacOS 

или

2. ВМ c ОС Linux в облаке либо ВМ на локальной машине для установки MicroK8S  

------

### Инструкция к заданию

1. Установка MicroK8S:
    - sudo apt update,
    - sudo apt install snapd,
    - sudo snap install microk8s --classic,
    - добавить локального пользователя в группу `sudo usermod -a -G microk8s $USER`,
    - изменить права на папку с конфигурацией `sudo chown -f -R $USER ~/.kube`.

2. Полезные команды:
    - проверить статус `microk8s status --wait-ready`;

  <img width="503" alt="status" src="https://github.com/user-attachments/assets/99c643ed-044c-4c0e-b6a0-45eb27e85ed6">

    - подключиться к microK8s и получить информацию можно через команду `microk8s command`, например, `microk8s kubectl get nodes`;

  <img width="258" alt="get nodes" src="https://github.com/user-attachments/assets/0e5c552f-e944-4f94-8060-38d9d4400d3b">

    - включить addon можно через команду `microk8s enable`; 

<img width="571" alt="enable addon dashboard" src="https://github.com/user-attachments/assets/6e6ccca0-6f9e-4706-aadd-48d757dfb010">

<img width="420" alt="status dashboard" src="https://github.com/user-attachments/assets/32097800-b10d-4445-8b18-c373d832acfb">

    - список addon `microk8s status`;

<img width="530" alt="microk8s status" src="https://github.com/user-attachments/assets/31e8d8e4-884f-4ee5-b91a-0248179308e1">

    - вывод конфигурации `microk8s config`;

<img width="712" alt="microk8s config" src="https://github.com/user-attachments/assets/8644965c-d834-4ad9-aa8d-3b9288a83abc">
 
    - проброс порта для подключения локально `microk8s kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443`.

<img width="532" alt="image" src="https://github.com/user-attachments/assets/359cf379-4d87-4503-9d96-9a2b7d5191a2">


3. Настройка внешнего подключения:
    - отредактировать файл /var/snap/microk8s/current/certs/csr.conf.template
    ```shell
    # [ alt_names ]
    # Add
    # IP.4 = 123.45.67.89
    ```
    - обновить сертификаты `sudo microk8s refresh-certs --cert front-proxy-client.crt`.

4. Установка kubectl:
    - curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl;
    - chmod +x ./kubectl;
    - sudo mv ./kubectl /usr/local/bin/kubectl;
    - настройка автодополнения в текущую сессию `bash source <(kubectl completion bash)`;
    - добавление автодополнения в командную оболочку bash `echo "source <(kubectl completion bash)" >> ~/.bashrc`.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. [Инструкция](https://microk8s.io/docs/getting-started) по установке MicroK8S.
2. [Инструкция](https://kubernetes.io/ru/docs/reference/kubectl/cheatsheet/#bash) по установке автодополнения **kubectl**.
3. [Шпаргалка](https://kubernetes.io/ru/docs/reference/kubectl/cheatsheet/) по **kubectl**.

------

### Задание 1. Установка MicroK8S

1. Установить MicroK8S на локальную машину или на удалённую виртуальную машину.

<img width="198" alt="microk8s version" src="https://github.com/user-attachments/assets/092044fd-8509-4876-81d2-fb1915e87d6b">

2. Установить dashboard.

<img width="571" alt="enable addon dashboard" src="https://github.com/user-attachments/assets/2fa5e48e-ef6b-490f-897f-ae7d1908059e">

<img width="420" alt="status dashboard" src="https://github.com/user-attachments/assets/d8d0f65b-87cb-4630-8ea1-ade5d9c2c9b1">


3. Сгенерировать сертификат для подключения к внешнему ip-адресу.

Добавить вначале внешний IP в файл с данными для формирования сертификатов

<img width="510" alt="Add IP" src="https://github.com/user-attachments/assets/4c89d55e-84e3-4e05-b608-385f18bca537">

Затем обновляем сертификаты

**sudo microk8s refresh-certs --cert front-proxy-client.crt
sudo microk8s refresh-certs --cert ca.crt**

<img width="478" alt="refresh cert" src="https://github.com/user-attachments/assets/e7505d66-a535-4a4d-af75-d978ff5fd06b">

------

### Задание 2. Установка и настройка локального kubectl
1. Установить на локальную машину kubectl.

<img width="566" alt="kubectl status" src="https://github.com/user-attachments/assets/358ea932-acb4-43e5-8330-43d48a59ea96">

2. Настроить локально подключение к кластеру.

Передаем конфиг с сертификатами, тогда только появится файл config
microk8s config > ~/.kube/config

3. Подключиться к дашборду с помощью port-forward.

<img width="374" alt="connect to server" src="https://github.com/user-attachments/assets/6dd63d25-82f1-4ff2-abd5-35fe337b4a7f">

Генерируем токен
**microk8s kubectl create token default**
Затем копируем этот токен и вставляем по адресу https://127.0.0.1:10443 в поле для Token

<img width="632" alt="Dashboard" src="https://github.com/user-attachments/assets/94e99519-9e13-4476-a4d6-be0bb7cf6c87">

<img width="689" alt="Dashboard kubectl" src="https://github.com/user-attachments/assets/969bce82-8b6a-4c4f-9650-8b3243e85e61">

<img width="1204" alt="All dashboard" src="https://github.com/user-attachments/assets/0308ab55-07bd-4d6d-bf4c-33d51e5694c4">

**Get nodes**

<img width="258" alt="get nodes" src="https://github.com/user-attachments/assets/48caba9e-d15b-4503-9de5-de3710226094">


------

### Правила приёма работы

1. Домашняя работа оформляется в своём Git-репозитории в файле README.md. Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.
2. Файл README.md должен содержать скриншоты вывода команд `kubectl get nodes` и скриншот дашборда.

------

### Критерии оценки
Зачёт — выполнены все задания, ответы даны в развернутой форме, приложены соответствующие скриншоты и файлы проекта, в выполненных заданиях нет противоречий и нарушения логики.

На доработку — задание выполнено частично или не выполнено, в логике выполнения заданий есть противоречия, существенные недостатки.
