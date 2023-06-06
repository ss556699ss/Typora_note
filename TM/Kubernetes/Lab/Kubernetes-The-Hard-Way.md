# [Kubernetes The Hard Way](https://github.com/caicloud/kube-ladder/blob/master/tutorials/lab3-manual-installtion.md#kubernetes-the-hard-way)



## 目標

在三台 vm 上，使用二進治手動安裝kubernetes。
目的是收斂 kubernetes 證書機制知識。

---

## 環境

三台 VM

這邊使用 multipass建立三台機器

``` sh
multipass launch --name 'VM1' --network VM  --cpus 2 --memory 4G --cloud-init=C:\Multipass\cloud-init.yaml
multipass launch --name 'VM2' --network VM  --cpus 2 --memory 4G --cloud-init=C:\Multipass\cloud-init.yaml
multipass launch --name 'VM3' --network VM  --cpus 2 --memory 4G --cloud-init=C:\Multipass\cloud-init.yaml
```



## 準備環境

### [開啟必要端口](https://kubernetes.io/zh-cn/docs/reference/networking/ports-and-protocols/)

這邊選擇直接關閉防火牆/也可關閉指定端口

```sh
systemctl stop firewalld
systemctl disable firewalld
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

``` sh
# 建立私鑰
openssl genrsa -out ca.key 2048

# 使用私鑰建立CSR
openssl req -new -key ca.key -subj "/CN=kubernetes-ca" -out ca.csr

# 產生 CRT=CSR + SIG
openssl x509 -req -in ca.csr -signkey ca.key -CAcreateserial  -out ca.crt -days 1000

# 可以讀取CSR
openssl req -in yourfile.csr -noout -text
```

[[Why private key is used amidst creation of CSR?](https://stackoverflow.com/questions/56449727/why-private-key-is-used-amidst-creation-of-csr)]

([RFC 2986](https://www.rfc-editor.org/rfc/rfc2986)

![image-20230519110534550](https://raw.githubusercontent.com/roger9491/Typora_note/main/img/image-20230519110534550.png)

### 客戶端和服務器證書

在`kubernets`，各個組件互相通信時才用雙向tls認證來識別對方，所謂的證書就是 `CRT`。

生成客戶端證書

``` shell
# 建立私鑰
openssl genrsa -out admin.key 2048

# 使用私鑰建立CSR
openssl req -new -key admin.key -subj "/CN=admin/O=system:masters" -out admin.csr

# CRT = CSR + SIG, SIG = 使用CA的私鑰加密CSR 
openssl x509 -req -in admin.csr -CA ca.crt -CAcreateserial  -out admin.crt -days 1000
```

5/19 不理解 為啥要使用 ca.crt









# 參考資料

https://github.com/Win32Sector/kubernetes-the-hard-way-1

https://github.com/caicloud/kube-ladder/blob/master/tutorials/lab3-manual-installtion.md
