# K3D

* k3d
* kubectl

建立集群，1 master、 2 worker

``` sh
sudo k3d cluster create test -s 1 -a 2	#建立集群
sudo k3d kubeconfig merge test	# 產生指定集群的kubeconfig
# 並印出生成路徑
# /root/.k3d/kubeconfig-test.yaml 
export KUBECONFIG=/root/.k3d/kubeconfig-test.yaml	#使得kubectl指向指定kubeconfig
```

查看集群

``` sh
sudo kubectl get noode
```

[可用選項參考至這裡](https://github.com/k3d-io/k3d/blob/main/pkg/config/v1alpha3/schema.json) 可能會更新

配置以下為 `simple.yaml` 

```yaml
apiVersion: k3d.io/v1alpha3
kind: Simple
name: my
servers: 1 
agents: 2
image: rancher/k3s:v1.20.4-k3s1
options:
  runtime:
    serversMemory: 1G
    agentMemory: 1G
  k3s: # options passed on to K3s itself
    extraArgs: # additional arguments passed to the `k3s server|agent` command; same as `--k3s-arg`
      - arg: eviction-hard=memory.available<300Mi
        nodeFilters:
        - agent:*
```

```sh
sudo k3d cluster create my-cluster --config ./simple.yaml
sudo k3d cluster list
```

