# [Kubernetes The Hard Way](https://github.com/caicloud/kube-ladder/blob/master/tutorials/lab3-manual-installtion.md#kubernetes-the-hard-way)



## 目標

在三台 vm 上，使用二進治手動安裝kubernetes。
目的是收斂 kubernetes 證書機制知識。

---

## 環境

三台 VM

這邊使用 multipass建立三台機器

``` sh
multipass launch --name 'k8s1' --network VM  --cpus 2 --memory 4G --cloud-init=C:\Multipass\cloud-init.yaml
multipass launch --name 'k8s1' --network VM  --cpus 2 --memory 4G --cloud-init=C:\Multipass\cloud-init.yaml
multipass launch --name 'k8s1' --network VM  --cpus 2 --memory 4G --cloud-init=C:\Multipass\cloud-init.yaml
```

IP

1. k8s1: `192.168.137.234`
2. k8s2: `192.168.137.120`
3. k8s3: `192.168.137.248`

## 準備環境

### [開啟必要端口](https://kubernetes.io/zh-cn/docs/reference/networking/ports-and-protocols/)

這邊選擇直接關閉防火牆/也可關閉指定端口

```sh
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

### 禁用

``` sh
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
```

### 關閉 swap

```sh
swapoff -a
```

---

## 艱難的方式部屬 kubernetes



### [安裝 kubectl 及 kubelet](https://kubernetes.io/zh-cn/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl)

**注意不要安裝到 kubeadm**



### [參考PKI 证书和要求](https://kubernetes.io/zh-cn/docs/setup/best-practices/certificates/)

了解k8s

1. 需要的證書
2. 需要修改的字段
3. 存放位置



### 建立 CA 證書

需要建立 kubernetes-ca, etcd-ca

``` sh
# 建立私鑰
openssl genrsa -out  kubernetes-ca.key 2048

# 使用私鑰建立CSR
openssl req -new -key  kubernetes-ca.key -out  kubernetes-ca.csr

# 產生 CRT=CSR + SIG, 期限 1000 天
openssl x509 -req -in  kubernetes-ca.csr -signkey  kubernetes-ca.key -CAcreateserial  -out  kubernetes-ca.crt -days 1000

# 建立私鑰
openssl genrsa -out  etcd-ca.key 2048

# 使用私鑰建立CSR
openssl req -new -key etcd-ca.key -out  etcd-ca.csr

# 產生 CRT=CSR + SIG, 期限 1000 天
openssl x509 -req -in  etcd-ca.csr -signkey  etcd-ca.key -CAcreateserial  -out  etcd-ca.crt -days 1000
```

[[Why private key is used amidst creation of CSR?](https://stackoverflow.com/questions/56449727/why-private-key-is-used-amidst-creation-of-csr)]

([RFC 2986](https://www.rfc-editor.org/rfc/rfc2986)

存放路徑

``` 
/etc/kubernetes/pki/ca.crt
/etc/kubernetes/pki/ca.key
/etc/kubernetes/pki/etcd/ca.crt
/etc/kubernetes/pki/etcd/ca.key
```



### 生成所有證書

[參考](https://kubernetes.io/zh-cn/docs/setup/best-practices/certificates/#all-certificates)

在`kubernets`，各個組件互相通信時才用雙向tls認證來識別對方，所謂的證書就是 `CRT`。

``` shell
# kube-etcd-healthcheck-client
openssl genrsa -out kube-etcd-healthcheck-client.key 2048
openssl req -new -key kube-etcd-healthcheck-client.key -out kube-etcd-healthcheck-client.csr
openssl x509 -req -in kube-etcd-healthcheck-client.csr -CA /home/ubuntu/cv/ca/etcd-ca.crt -CAkey /home/ubuntu/cv/ca/etcd-ca.key -CAcreateserial  -out kube-etcd-healthcheck-client.crt -days 1000
# kube-apiserver-etcd-client
openssl genrsa -out kube-apiserver-etcd-client.key 2048
openssl req -new -key kube-apiserver-etcd-client.key -out kube-apiserver-etcd-client.csr -subj "/O=system:masters"
openssl x509 -req -in kube-apiserver-etcd-client.csr -CA /home/ubuntu/cv/ca/etcd-ca.crt -CAkey /home/ubuntu/cv/ca/etcd-ca.key -CAcreateserial  -out kube-apiserver-etcd-client.crt -days 1000
# kube-apiserver-kubelet-client
openssl genrsa -out kube-apiserver-kubelet-client.key 2048
openssl req -new -key kube-apiserver-kubelet-client.key -out kube-apiserver-kubelet-client.csr -subj "/O=system:masters"
openssl x509 -req -in kube-apiserver-kubelet-client.csr -CA /home/ubuntu/cv/ca/kubernetes-ca.crt -CAkey /home/ubuntu/cv/ca/kubernetes-ca.key -CAcreateserial  -out kube-apiserver-kubelet-client.crt -days 1000
```

需要 `SAN` 字段

``` sh
# kube-etcd
openssl genrsa -out kube-etcd.key 2048
vi kube-etcd.cnf
---
[req]
distinguished_name = req_distinguished_name
req_extensions = req_ext
prompt = no
[req_distinguished_name]
C   = TW
ST  = TW
L   = Taipei
O   = Dynasafe
OU  = CNS
[req_ext]
subjectAltName = @alt_names
[alt_names]
IP.1 = 127.0.0.1
IP.2 = 192.168.137.234
DNS.1 = localhost
---
openssl req -new -key kube-etcd.key -out kube-etcd.csr -config kube-etcd.cnf
openssl x509 -req -in kube-etcd.csr -CA /home/ubuntu/cv/ca/etcd-ca.crt -CAkey /home/ubuntu/cv/ca/etcd-ca.key -CAcreateserial  -out kube-etcd.crt -days 1000

# kube-etcd-peer
openssl genrsa -out kube-etcd-peer.key 2048
vi kube-etcd-peer.cnf
---
[req]
distinguished_name = req_distinguished_name
req_extensions = req_ext
prompt = no
[req_distinguished_name]
C   = TW
ST  = TW
L   = Taipei
O   = Dynasafe
OU  = CNS
[req_ext]
subjectAltName = @alt_names
[alt_names]
IP.1 = 127.0.0.1
IP.2 = 192.168.137.234
DNS.1 = localhost
---
openssl req -new -key kube-etcd-peer.key -out kube-etcd-peer.csr -config kube-etcd-peer.cnf
openssl x509 -req -in kube-etcd-peer.csr -CA /home/ubuntu/cv/ca/etcd-ca.crt -CAkey /home/ubuntu/cv/ca/etcd-ca.key -CAcreateserial  -out kube-etcd-peer.crt -days 1000

# kube-apiserver
openssl genrsa -out kube-apiserver.key 2048
vi kube-apiserver.cnf
---
[req]
distinguished_name = req_distinguished_name
req_extensions = req_ext
prompt = no
[req_distinguished_name]
C   = TW
ST  = TW
L   = Taipei
O   = Dynasafe
OU  = CNS
[req_ext]
subjectAltName = @alt_names
[alt_names]
IP.1 = 127.0.0.1
IP.2 = 192.168.137.234
DNS.1 = localhost
---
openssl req -new -key kube-apiserver.key -out kube-apiserver.csr -config kube-apiserver.cnf
openssl x509 -req -in kube-apiserver.csr -CA /home/ubuntu/cv/ca/kubernetes-ca.crt -CAkey /home/ubuntu/cv/ca/kubernetes-ca.key -CAcreateserial  -out kube-apiserver.crt -days 1000

# sa
openssl genrsa -out sa.key 2048
openssl rsa -in sa.key -pubout -out sa.pub
```

存放路徑

``` 
/etc/kubernetes/pki/etcd/ca.key
/etc/kubernetes/pki/etcd/ca.crt
/etc/kubernetes/pki/apiserver-etcd-client.key
/etc/kubernetes/pki/apiserver-etcd-client.crt
/etc/kubernetes/pki/ca.key
/etc/kubernetes/pki/ca.crt
/etc/kubernetes/pki/apiserver.key
/etc/kubernetes/pki/apiserver.crt
/etc/kubernetes/pki/apiserver-kubelet-client.key
/etc/kubernetes/pki/apiserver-kubelet-client.crt
/etc/kubernetes/pki/front-proxy-ca.key
/etc/kubernetes/pki/front-proxy-ca.crt
/etc/kubernetes/pki/front-proxy-client.key
/etc/kubernetes/pki/front-proxy-client.crt
/etc/kubernetes/pki/etcd/server.key
/etc/kubernetes/pki/etcd/server.crt
/etc/kubernetes/pki/etcd/peer.key
/etc/kubernetes/pki/etcd/peer.crt
/etc/kubernetes/pki/etcd/healthcheck-client.key
/etc/kubernetes/pki/etcd/healthcheck-client.crt
/etc/kubernetes/pki/sa.key
/etc/kubernetes/pki/sa.pub
```

用戶配置證書

``` shell
# admin.conf
openssl genrsa -out admin-key.pem 2048
openssl req -new -key admin-key.pem -out admin-csr.pem -subj "/O=system:masters/CN=kubernetes-admin"
openssl x509 -req -in admin-csr.pem -CA /home/ubuntu/cv/ca/kubernetes-ca.crt -CAkey /home/ubuntu/cv/ca/kubernetes-ca.key -CAcreateserial  -out admin-crt.pem -days 1000
KUBECONFIG=admin.conf kubectl config set-cluster default-cluster --server=https://192.168.137.234:6443 --certificate-authority ./ca/kubernetes-ca.crt --embed-certs 
KUBECONFIG=admin.conf kubectl config set-credentials admin --client-key admin-key.pem --client-certificate admin-crt.pem --embed-certs
KUBECONFIG=admin.conf kubectl config set-context default-system --cluster default-cluster --user admin
KUBECONFIG=admin.conf kubectl config use-context default-system

# k8s2-kubelet.conf	k8s2
openssl genrsa -out k8s2-kubelet-key.pem 2048
openssl req -new -key k8s2-kubelet-key.pem -out k8s2-kubelet-csr.pem -subj "/O=system:nodes/CN=system:node:k8s2"
openssl x509 -req -in k8s2-kubelet-csr.pem -CA /home/ubuntu/cv/ca/kubernetes-ca.crt -CAkey /home/ubuntu/cv/ca/kubernetes-ca.key -CAcreateserial  -out k8s2-kubelet-crt.pem -days 1000
KUBECONFIG=k8s2-kubelet.conf kubectl config set-cluster default-cluster --server=https://192.168.137.234:6443 --certificate-authority ./ca/kubernetes-ca.crt --embed-certs 
KUBECONFIG=k8s2-kubelet.conf kubectl config set-credentials k8s2-kubelet --client-key k8s2-kubelet-key.pem --client-certificate k8s2-kubelet-crt.pem --embed-certs
KUBECONFIG=k8s2-kubelet.conf kubectl config set-context default-system --cluster default-cluster --user k8s2-kubelet
KUBECONFIG=k8s2-kubelet.conf kubectl config use-context default-system

# k8s3-kubelet.conf	k8s3
openssl genrsa -out k8s3-kubelet-key.pem 2048
openssl req -new -key k8s3-kubelet-key.pem -out k8s3-kubelet-csr.pem -subj "/O=system:nodes/CN=system:node:k8s3"
openssl x509 -req -in k8s3-kubelet-csr.pem -CA /home/ubuntu/cv/ca/kubernetes-ca.crt -CAkey /home/ubuntu/cv/ca/kubernetes-ca.key -CAcreateserial  -out k8s3-kubelet-crt.pem -days 1000
KUBECONFIG=k8s3-kubelet.conf kubectl config set-cluster default-cluster --server=https://192.168.137.234:6443 --certificate-authority ./ca/kubernetes-ca.crt --embed-certs 
KUBECONFIG=k8s3-kubelet.conf kubectl config set-credentials k8s3-kubelet --client-key k8s3-kubelet-key.pem --client-certificate k8s3-kubelet-crt.pem --embed-certs
KUBECONFIG=k8s3-kubelet.conf kubectl config set-context default-system --cluster default-cluster --user k8s3-kubelet
KUBECONFIG=k8s3-kubelet.conf kubectl config use-context default-system

# controller-manager.conf
openssl genrsa -out controller-manager-key.pem 2048
openssl req -new -key controller-manager-key.pem -out controller-manager-csr.pem -subj "/O=system:nodes/CN=system:node:k8s3"
openssl x509 -req -in k8s3-kubelet-csr.pem -CA /home/ubuntu/cv/ca/kubernetes-ca.crt -CAkey /home/ubuntu/cv/ca/kubernetes-ca.key -CAcreateserial  -out k8s3-kubelet-crt.pem -days 1000
KUBECONFIG=k8s3-kubelet.conf kubectl config set-cluster default-cluster --server=https://192.168.137.234:6443 --certificate-authority ./ca/kubernetes-ca.crt --embed-certs 
KUBECONFIG=k8s3-kubelet.conf kubectl config set-credentials k8s3-kubelet --client-key k8s3-kubelet-key.pem --client-certificate k8s3-kubelet-crt.pem --embed-certs
KUBECONFIG=k8s3-kubelet.conf kubectl config set-context default-system --cluster default-cluster --user k8s3-kubelet
KUBECONFIG=k8s3-kubelet.conf kubectl config use-context default-system
```









# 參考資料

https://github.com/Win32Sector/kubernetes-the-hard-way-1

https://github.com/caicloud/kube-ladder/blob/master/tutorials/lab3-manual-installtion.md
