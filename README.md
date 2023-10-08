# Steps to deploy:
1. `ssh-keygen -t rsa` - генерирует SSH-ключи для безопасного соединения с удаленными серверами.
2. `ssh-copy-id 10.1.1.101` - копирует публичный ключ SSH на удаленный сервер для автоматической аутентификации.
3. `sudo vi etc/sudoers` - открывает файл sudoers с помощью текстового редактора vi для добавления привилегий пользователяm ALL=(ALL) NOPASSWD:ALL` - позволяет пользователю выполнять команды sudo без ввода пароля.
    1. `user ALL=(ALL) NOPASSWD:ALL`
4. `sudo apt install python3-pip` - устанавливает пакетный менеджер Python pip3.
5. `sudo pip3 install upgrade pip` - обновляет pip до последней версии.
6. `git clone https://github.com/kubernetes-sigs/kubespray.git` - клонирует репозиторий Kubespray с GitHub.
7. `cd kubespray` - переходит в директорию kubespray.
8. `sudo pip install -r requirements.txt` - устанавливает зависимости Kubespray из файла requirements.txt.
9. `cp -rfp inventory/sample inventory/mycluster` - копирует пример инвентарного файла в папку mycluster.
10. `declare -a IPS=(10.1.1.101 10.1.1.102 10.1.1.103)` - объявляет переменную с IP-адресами узлов кластера.
11. `CONFIG_FILE=inventory/mycluster/hosts.yml python3 contrib/inventory_builder/inventory.py ${IPS[@]}` - создает инвентарный файл hosts.yml на основе IP-адресов узлов.
12. `vi inventory/mycluster/hosts.yml` - открывает инвентарный файл hosts.yml для проверки и настройки хостов и их ролей.
13. `vi inventory/mycluster/group_vars/all/all.yml` - открывает файл конфигурации all.yml для настройки общих переменных.
14. `vi inventory/mycluster/group_vars/k8s-cluster/k8s-cluster.yml` - открывает файл конфигурации k8s-cluster.yml для указания версии Kubernetes, адреса сервисов и подсети подов.
15. `ansible-playbook -i inventory/mycluster/hosts.yml --become --become-user=root cluster.yml` - запускает процесс развертывания кластера Kubernetes с использованием Kubespray.
16. `curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/darwin/amd64/kubectl"` - загружает клиентскую утилиту kubectl для управления кластером Kubernetes.
17. `ssh 10.1.1.101 sudo copy /etc/kubernetes/admin.conf /home/user/config` - копирует файл конфигурации kubectl с удаленного сервера на локальную машину.
18. `ssh 10.1.1.101 sudo chmod +r ~/config` - изменяет права доступа к файлу конфигурации на чтение.
19. `scp 10.1.1.101:~/config .` - копирует файл конфигурации с удаленного сервера на локальную машину.
20. `mkdir .kube` - создает директорию .kube для хранения файла конфигурации.
21. `mv config .kube/` - перемещает файл конфигурации в директорию .kube.
22. `ssh 10.1.1.101 sudo rm ~/config` - удаляет файл конфигурации с удаленного сервера.
23. `kubectl version` - выводит информацию о версии установленного клиента kubectl.
24. `kubectl get nodes` - выводит информацию о доступных узлах в кластере Kubernetes.