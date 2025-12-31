Udemy lab未

Security

- service account
- image security
- security context

Network

- ingress networking1
- ingress networking2

## ネットワーク系

- ClusterIP
クラスタ内部で接続可能。
外部から接続する場合は、NodePortやLBを使う。
- NodePort
クラスタ外部から接続可能。
しかし、実際端末から接続する際はLBを前段に置き、そのTargetPortとしてNodePort
- LoadBalancer（LBはL4、IngressはL7）

## コマンド　★なるべく公式見る

（公式）https://kubernetes.io/ja/docs/reference/kubectl/cheatsheet/

https://qiita.com/hana_shin/items/ef1a20239001ac83a78d

```bash
# 一覧表示
$ kubectl get
# 詳細表示
$ kubectl describe
# 作成 
$ kubectl create
# 削除
$ kubectl delete
```

```bash
# contextをワークロードクラスタに切り替え
$ kubectl config use-context p-tckmrb-esxm-wlc001-admin@p-tckmrb-esxm-wlc001
# contextの一覧・カレントを表示
$ kubectl config get-contexts

# コンテキストのデフォルト Namespace を "mynamespace" に変更する
$ kubectl config set-context $(kubectl config current-context) --namespace=mynamespace

# Namespace を確認する
$ kubectl config view | grep namespace:
```

## 入門本6.3.1

```bash
  377  kind delete cluster
  378  kind create cluster --name kind-nodeport --config kind/export-mapping.yaml --image=kindest/node:v1.29.0
  379  kubectl apply --filename chapter-06/deployment-hello-server.yaml
  380  kubectl apply --filename chapter-06/service-nodeport.yaml
  381  kubectl get service
  382  kubectl get service hello-server-external
  383  kubectl get nodes -o wide
  384  kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}'
  385  curl 172.18.0.2:30599
  386  history
  387  curl localhost:30599
  388  kubectl delete --filename chapter-06/deployment-hello-server.yaml
  389  kubectl delete --filename chapter-06/service-nodeport.yaml
```

## 入門本8.3

```bash
kubectl get deployment # depoloymentの一覧
kubectl get replicaset # replicasetの一覧
kubectl get service # serviceの一覧

kubuctl get pod # pod確認
kubectl describe pod [pod名]
kubuctl logs [pod名]　# ログ確認
kubectl get deployment [deployment名] --output yaml # deploymentをyaml表示

# デバッグ用コンテナを立ち上げてコンテナ内部から確認
kubectl debug --stdin --tty [任意のpod名] --image=curlimages/curl --container=debug-container -- sh

# pod確認(詳細)
kubectl get pod --output wide
```

## Udemy(CKA) HandsOn (KodeKloud)

```bash
alias k="kubectl"
### Pods ###
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml

### Replicaset ###　★要復習
kubectl scale --replicas=3 -f foo.yaml
kubectl scale rs new-replica-set --replicas=3
kubectl edit replicaset new-replica-set
kubectl edit replicaset/new-replica-set

### Namespace ###
kubectl get pod --namespace=research
kubectl get pods **--all-namespaces(-A)**

### Imperative ###　★要復習
kubectl expose pod redis --port=6379 --name=redis-service --type=ClusterIP
kubectl run httpd --image=httpd:alpine --port=80 --expose

### Manual Scheduling ###　★要復習
kubectl get nodes
kubectl get po -o wide
kubecgtl replace --force -f nginx.yaml

### Labels and Selectors ###
kubectl get all -l env=prod --no-headers | wc -l

### Taints and Tolerations ### ★要復習
kubectl describe node node01 | grep -i taints
kubectl taint nodes node01 spray=mortein:NoSchedule
kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-
---
apiVersion: v1
kind: Pod
metadata:
  name: bee
spec:
  containers:
  - image: nginx
    name: bee
  tolerations:
  - key: spray
    value: mortein
    effect: NoSchedule
    operator: Equal

### Node Affinity ### ★要復習
---
spec:
  template:
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: color
                operator: In
                values:
                - blue
 
### Resource Limits ### ★要復習
editがだめならこれでやる！（limit変更はeditでは受け付けない）
kubectl get po elephant -o yaml > elephant.yaml
kubectl replace -f elephant.yaml --force

### DaemonSets ### ★要復習
一旦Deploymentを構築するyamlを作成してから、Daemonsets用に編集するのが簡単らしい。
kubectl create deployment elasticsearch
--image=registry.k8s.io/fluentd-elasticsearch:1.20
-n kube-system --dry-run=client -o yaml > fluentd.yaml

Then, edit the YAML as follows:

Change: kind: Deployment → kind: DaemonSet
Ensure: apiVersion: apps/v1
Remove these fields from the YAML:
spec.replicas
spec.strategy
status (entire section, if present)

Finally, create the DaemonSet using:
kubectl create -f fluentd.yaml

### Static PODs ###
kubectl run --restart=Never --image=busybox static-busybox --dry-run=client -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml

### Helm ###
# helmでは欲しいリポジトリをaddして、リポジトリからChartを生成する
helm repo list
helm repo add [name] [repository]
helm install amaze-surf bitnami/apache
helm repo remove hashicorp
helm upgrade dazzling-web bitnami/nginx --version 18.3.6
helm rollback dazzling-web

### Rolling Update ###
kubectl set image deployment [deployment-name] [container-name]=[new-image-id]

### Env Variables ###
k run webapp-color --image=kodekloud/webapp-color --labels="name=webapp-color" --env="APP_COLOR=green"

### Secret ###
k create secret generic db-secret --from-literal=DB_Host=sql01 --from-literal=DB_User=root --from-literal=DB_Password=password123

### Init Container ###
k describe pod # Without pod name, i can see all containers descriptions
k logs orange -c initcontainer

### Cluster Upgrade Process ###
kubectl drain controlplane --ignore-daemonsets

### View Certificate Details ###
/etc/kubernetes/pki/apiserver-kubelet-client.crt
# CNを調べる
openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text

### Trouble Shootings ###
1. service name
2. port number
3. selector
4. DB user (env of web deployment)
5. DB user (env of mysql pod)
6. service port / 4 / 5
# コンテキストのデフォルト Namespace を "mynamespace" に変更する
$ kubectl config set-context $(kubectl config current-context) --namespace=mynamespace
# Namespace を確認する
$ kubectl config view | grep namespace:

### Trouble Shootings2 ###
1. /etc/kubernetes/manifests/kube-scheduler.yaml

### Trouble Shootings3 ###
1. systemctl status kubelet
2. journalctl -u kubelet 
   /var/lib/kubelet/config.yaml
3. /etc/kubernetes/kubelet.conf
   systemctl restart kubelet

### Explore Environment ###
# プログラム名付きで開いてるポートを表示する
ss -tlnp

### CNI ###
ps -aux | grep kubelet | grep --color container-runtime-endpoint

### CoreDNS in Kubernetes ###
kubectl exec -it hr -- nslookup mysql.payroll > /root/CKA/nslookup.out

### CSR ###
kubectl get csr
kubectl get csr [csr name] -o yaml
kubectl certificate approve

### KubeConfig ###
~/.kube/config
kubectl config --kubeconfig /root/my-kube-config use-context research
# change the default config
vi ~/.bashrc
export KUBECONFIG=/root/my-kube-config
# OR
export KUBECONFIG=~/my-kube-config
# OR
export KUBECONFIG=$HOME/my-kube-config
source ~/.bashrc

### Role Based Access Controls ###
kubectl describe rolebinding kube-proxy -n kube-system
k get pod --as dev-user

```

## Helm

**Kubernetes上でアプリケーションをパッケージ化、デプロイ、管理するためのパッケージマネージャー**です。Helmを使うことで、複数のKubernetesマニフェストファイルを「[チャート](https://www.google.com/search?safe=active&sca_esv=150a47ab3b64ff5c&rlz=1C1GCEA_enJP1109JP1109&cs=0&sxsrf=AE3TifN4SbKc-fH5kW7eg0g3iIjThf_cWw%3A1754377469984&q=%E3%83%81%E3%83%A3%E3%83%BC%E3%83%88&sa=X&ved=2ahUKEwiD3--ujfOOAxU5ha8BHRmLF54QxccNegQIBBAB&mstk=AUtExfDhWuT4LgMpNace3RQCodYRrdpH3JUuugsZXaOs51RVAr1jCwBZ9Ey6fD35uDSc22sfie-tBCNTctjBa1EkKypEj1N5N46auf4yQk9VSU2ni-seaFdVnwz_s9kWAhrAOlurob2LfD37P9cPFh7kOW45fV8dUGuwsEMAsqnxed6hgQGVwBx1_DWMg9QhS5YNM-LNLs42k1hg0aZqWxnvLPEzckSR0ExBRxCTGNvYHrLHdB2iugYeH9F2NCxjOCy-F_4jk85VekfjLf7ZyqJz69rOHwlJlnoyOrrvktOIDfK2zA&csui=3)」と呼ばれる一つの再利用可能なパッケージにまとめ、複雑なアプリケーションのインストール、構成、更新、ロールバックを自動化できます。これにより、デプロイの一貫性、再現性、信頼性が向上し、Kubernetesアプリケーションの管理が容易になります。﻿

## 勉強系の参考サイト

https://speakerdeck.com/mixi_engineers/2023-container-training-number-02-kubernetes

https://speakerdeck.com/mixi_engineers/2024-new-grad-training-container-part1

## 資格

https://qiita.com/fruscianteee/items/5963793b870835023d53