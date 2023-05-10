kubeadm join 10.250.75.102:6443 --token h7tv2a.w7l3nvjd4ftnl927 \
        --discovery-token-ca-cert-hash sha256:8cb4dbb26a7cedfd06492b6af61dcb29aaa45fc8f3a2bb7ff088e8d2fbcef006



無法使用kubectl 被refuced時

![image-20230107121139061](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20230107121139061.png)



kubectl describe node 詳細資訊介紹

``` 
Name:               a0917-roger-1
Roles:              <none>
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=a0917-roger-1
                    kubernetes.io/os=linux
Annotations:        node.alpha.kubernetes.io/ttl: 0
                    volumes.kubernetes.io/controller-managed-attach-detach: true
CreationTimestamp:  Sat, 07 Jan 2023 12:10:59 +0800
Taints:             node.kubernetes.io/not-ready:NoSchedule
Unschedulable:      false
Lease:
  HolderIdentity:  a0917-roger-1
  AcquireTime:     <unset>
  RenewTime:       Sat, 07 Jan 2023 13:03:09 +0800
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime
       Reason                       Message
  ----             ------  -----------------                 ------------------
       ------                       -------
  MemoryPressure   False   Sat, 07 Jan 2023 13:02:29 +0800   Sat, 07 Jan 2023 12:10:57 +0800   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Sat, 07 Jan 2023 13:02:29 +0800   Sat, 07 Jan 2023 12:10:57 +0800   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Sat, 07 Jan 2023 13:02:29 +0800   Sat, 07 Jan 2023 12:10:57 +0800   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            False   Sat, 07 Jan 2023 13:02:29 +0800   Sat, 07 Jan 2023 12:10:57 +0800   KubeletNotReady              container runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:Network plugin returns error: cni plugin not initialized
  
  '''
  Conditions
描述所有運行節點目前的狀態，狀態的描述有以下幾種：

Node Condition	Description
Ready	True表示節點運行狀況良好並準備好接受Pod，False表示節點運行狀況不佳並且不接受Pod，Unknown表示節點控制器最近一次未從節點收到消息node-monitor-grace-period（默認值為40秒）	
DiskPressure	True表示磁盤容量不足；除此以外False	
MemoryPressure	True表示節點內存不足; 除此以外False	
PIDPressure	True表示節點上的Process太多；除此以外False	
NetworkUnavailable	True表示節點的網絡配置不正確，否則 False
'''

Addresses:
  InternalIP:  10.250.75.102
  Hostname:    a0917-roger-1
Capacity: #描述該節點上可用資源最大數量，包含cpu、memory與pods的數量等...。
  cpu:                2
  ephemeral-storage:  13106Mi
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             3820696Ki
  pods:               110
Allocatable:
  cpu:                2
  ephemeral-storage:  12368373330
  hugepages-1Gi:      0
  hugepages-2Mi:      0
  memory:             3718296Ki
  pods:               110
System Info:
  Machine ID:                 da62ceefe6cc456ca5ca688e538a5c95
  System UUID:                c94a1b42-282c-b2de-106f-75a76a818ad5
  Boot ID:                    a21dcf9f-844a-4ef4-a231-c6ca02470b9a
  Kernel Version:             4.18.0-425.3.1.el8.x86_64
  OS Image:                   Rocky Linux 8.7 (Green Obsidian)
  Operating System:           linux
  Architecture:               amd64
  Container Runtime Version:  containerd://1.6.15
  Kubelet Version:            v1.26.0
  Kube-Proxy Version:         v1.26.0
PodCIDR:                      192.168.0.0/24
PodCIDRs:                     192.168.0.0/24
Non-terminated Pods:          (4 in total)
  Namespace                   Name                                     CPU Requests  CPU Limits  Memory Requests  Memory Limits  Age
  ---------                   ----                                     ------------  ----------  ---------------  -------------  ---
  kube-system                 etcd-a0917-roger-1                       100m (5%)     0 (0%)      100Mi (2%)       0 (0%)         52m
  kube-system                 kube-apiserver-a0917-roger-1             250m (12%)    0 (0%)      0 (0%)           0 (0%)         52m
  kube-system                 kube-controller-manager-a0917-roger-1    200m (10%)    0 (0%)      0 (0%)           0 (0%)         52m
  kube-system                 kube-scheduler-a0917-roger-1             100m (5%)     0 (0%)      0 (0%)           0 (0%)         52m
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource           Requests    Limits
  --------           --------    ------
  cpu                650m (32%)  0 (0%)
  memory             100Mi (2%)  0 (0%)
  ephemeral-storage  0 (0%)      0 (0%)
  hugepages-1Gi      0 (0%)      0 (0%)
  hugepages-2Mi      0 (0%)      0 (0%)
Events:
  Type     Reason                   Age                From             Message
  ----     ------                   ----               ----             -------
  Normal   Starting                 52m                kubelet          Starting kubelet.
  Warning  InvalidDiskCapacity      52m                kubelet          invalid capacity 0 on image filesystem
  Normal   NodeAllocatableEnforced  52m                kubelet          Updated Node Allocatable limit across pods
  Normal   NodeHasSufficientMemory  52m (x8 over 52m)  kubelet          Node a0917-roger-1 status is now: NodeHasSufficientMemory
  Normal   NodeHasNoDiskPressure    52m (x7 over 52m)  kubelet          Node a0917-roger-1 status is now: NodeHasNoDiskPressure
  Normal   NodeHasSufficientPID     52m (x7 over 52m)  kubelet          Node a0917-roger-1 status is now: NodeHasSufficientPID
  Normal   RegisteredNode           52m                node-controller  Node a0917-roger-1 event: Registered Node a0917-roger-1 in Controller
```



calico control 一直pending ，突然下一些指令就好了,不確定是哪個奏效

``` 
lsmod | grep br_netfilter
lsmod | grep overlaysystemctl restart containerd
systemctl restart containerd
```

