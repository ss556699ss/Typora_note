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

#### 待實驗
