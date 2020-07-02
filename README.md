# Установка k3s + rancher
1. Запуск скрипта установки. 
     ```shell script
     curl -sfL https://get.k3s.io | sh -s - --disable metrics-server,traefik --write-kubeconfig-mode 777 --node-ip 192.168.56.117 --node-external-ip 192.168.56.117
     ```
    **--disable metrics-server,traefik** - отключаем стандартный ingress(проблемы с tls) и metrics-server(проблемы с tls)
    
    **node-ip и node-external-ip** - ip виртуалки
2. Переопределяем настройки для coredns.  

    1. Заменяем файл coredns.yml в /var/lib/rancher/k3s/server/manifests 

3. Ставим ingress-nginx
    ```shell script
    kubectl create namespace ingress-nginx
    kubectl apply -f nginx-helm.yml
    ```
 
4. Ставим cert-manager
     ```shell script
     kubectl create namespace cert-manager
     kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v0.15.1/cert-manager.crds.yaml
     kubectl apply -f cert-manager-helm.yml
     ```
  
5. Ставим rancher
     ```shell script
     kubectl create namespace cattle-system
     kubectl apply -f rancher-helm.yml
     ```
____

### Полезные команды:
* Удаление кластера
     ```shell script
      /usr/local/bin/k3s-uninstall.sh
     ```
* Удаление всего в namespace
     ```shell script
      kubectl delete all,secrets,configmap,daemonsets --all -n test
     ```
