# [Part 2: The OOM killer and application runtime implications](https://mihai-albert.com/2022/02/13/out-of-memory-oom-in-kubernetes-part-2-the-oom-killer-and-application-runtime-implications/)

## Overcommit

linux 內存管理，內存地址分為 物理地址和虛擬地址，進程無法直接看到物理地址，一切使用都是向內核

申請虛擬的內存區域，根據獲得的區域再依地址存放數據。

所以簡單概述進程如果需要使用到內存，會有兩個步驟

1. 申請內存
2. 占用內存

而占用內存才會實際算在已分配內存裡。

所以 `overcommit` 就是在決定申請內存的部分，具體分為三種情況 [Overcommit Accounting](https://www.kernel.org/doc/Documentation/vm/overcommit-accounting.rst)

* 0 : overcommit已啟用，但內存會拒絕明顯的過度使用
* 1:  總是過度使用
* 2:  不要過度使用，注意使用

查看方式為

``` shell
cat /proc/sys/vm/overcommit_memory
```

真對模式 `2` 解釋

設置一個可申請的限制，修改 `/proc/sys/vm/overcommit_ratio` 值，系統根據公式:

```
(ratio%*物理內存) + 交換內存
```

算出可申請內存的值，可查詢

``` sh
ls /proc/sys/vm/
查看 
CommitLimit: 	可申請內存    
Committed_AS:	目前申請的內存
```









# [Part 4: Pod evictions, OOM scenarios and flows leading to them](https://mihai-albert.com/2022/02/13/out-of-memory-oom-in-kubernetes-part-4-pod-evictions-oom-scenarios-and-flows-leading-to-them/)

## Pod 驅逐

當前 kubernetes 節點上的 

` Pod 總體使用量  > Allocatable`

，kubelet就會觸發驅逐機制。

``` sh
kubectl describe <node-name>
# 檢查 Allocatable 值
```

每 10 秒(默認配置 housekeeping-interval) kubelet會檢查一次

當驅逐發生時，節點條件會轉換狀態 [节点条件](https://kubernetes.io/zh-cn/docs/concepts/scheduling-eviction/node-pressure-eviction/#node-conditions) ，過 默認值 5分鐘 才會回復到原本的狀態 [节点条件振荡](https://kubernetes.io/zh-cn/docs/concepts/scheduling-eviction/node-pressure-eviction/#%E8%8A%82%E7%82%B9%E6%9D%A1%E4%BB%B6%E6%8C%AF%E8%8D%A1)
，此時無法調度 POD 至 節點上。

針對 `Guaranteed` 和 ` Burstable` 的 POD 是具有 `node.kubernetes.io/memory-pressure` 容忍度的。

[基于节点状态添加污点](https://kubernetes.io/zh-cn/docs/concepts/scheduling-eviction/taint-and-toleration/#taint-nodes-by-condition)

#### 待實驗 k3d

#### 目的

實驗 當發生節點狀態轉換時，是否過5分鐘才會回復狀態

`Guaranteed` 和 ` Burstable` 的 POD 是具有 `node.kubernetes.io/memory-pressure` 容忍度的。

#### 準備實驗環境

前置:

* docker
* k3d
* kubectl

建立集群，1 master、 2 worker
``` sh
sudo k3d cluster create test -s 1 -a 2	#建立集群
sudo k3d kubeconfig merge test
# /root/.k3d/kubeconfig-test.yaml 
export KUBECONFIG=/root/.k3d/kubeconfig-test.yaml	#使得kubectl指向指定kubeconfig
```

查看集群

``` sh
sudo kubectl get noode
```

```yaml
apiVersion: k3d.io/v1alpha2
kind: Simple
name: mycluster
servers: 1 
agents: 2
image: rancher/k3s:v1.20.4-k3s1
options:
  k3s: # options passed on to K3s itself
    extraServerArgs: # additional arguments passed to the `k3s server` command; same as `--k3s-server-arg`
      - --memory=1G
    extraAgentArgs: 
 	  - --memory=1G
 	# addditional arguments passed to the `k3s agent` command; same as `--k3s-agent-arg`
 	
k3d cluster create test --servers 1 --agents 2 --agents-memory 1G        
```





## 驅逐根據

[驱逐信号](https://kubernetes.io/zh-cn/docs/concepts/scheduling-eviction/node-pressure-eviction/) 文章裡的 [腳本](https://kubernetes.io/zh-cn/examples/admin/resource/memory-available.sh) 給出了 kubernetes 是如何計算出 `memory.available`

``` sh
# 文章的公式為 
memory.available := node.status.capacity[memory] - node.stats.memory.workingSet
# 從腳本可以得知 
node.status.capacity[memory] #來自 cat /proc/meminfo | grep MemTotal | awk '{print $2}'
node.stats.memory.workingSet 
#計算自 memory_usage_in_bytes - memory_total_inactive_file
memory_usage_in_bytes #來自 cat /sys/fs/cgroup/memory/memory.usage_in_bytes
memory_total_inactive_file # 來自 cat /sys/fs/cgroup/memory/memory.stat | grep total_inactive_file | awk '{print $2}'
```

疑問 這些指標來自於 根cgroup ，表示說 因為驅逐是發生在 Allocatable 不夠時 不理解的是 
`node.status.capacity[memory] ` 並不是 Allocatable  ，待查



[节点可分配资源](https://kubernetes.io/zh-cn/docs/tasks/administer-cluster/reserve-compute-resources/#node-allocatable)

![image-20230417135613304](./Out-of-memory (OOM) in Kubernetes.assets/image-20230417135613304.png)

--kube-reserved: 通過規定 `cgroup` 限制來實現

所以 pod 的驅逐發生在 

```sh
Capacity - --kube-reserved < pod 驅逐 < allocatable
vt5o`標誌確保一旦節點可用存低於指定值 即驅逐，同時也間接的規定了 pod 的 'allocatable' 內存大小
--system-reserved 可選
```

