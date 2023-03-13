

arp -> router





# Calico

---

## Calico 安裝流程

1. Install the Tigera Calico operator and custom resource definitions.

   ``` 
   kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.24.5/manifests/tigera-operator.yaml

2. Install Calico by creating the necessary custom resource. For more information on configuration options available in this manifest, see [the installation reference](https://projectcalico.docs.tigera.io/reference/installation/api).

   ``` 
   kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.24.5/manifests/custom-resources.yaml2
   ```

   以上跑完基本上Calico就安裝完成了

   ---

   ## 詳解Calico 流程

   

