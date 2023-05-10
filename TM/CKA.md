# CKA

``` mermaid
mindmap
root((Kubernetes))
	1.部屬
	2.服務
	3.卷
	4.配置
	5.安全


```

## 5. 安全

### 1.为部署流水线创建一个新的 ClusterRole 并将其绑定到范围为特定的 namespace 的特定ServiceAccount。

Task

创建一个名为 deployment-clusterrole 的 clusterrole，该 clusterrole 只允许对 Deployment、Daemonset、Statefulset 具有 create 权限，在现有的 namespace app-team1 中创建
一个名为 cicd-token 的新 ServiceAccount。
限于 namespace app-team1 中，将新的 ClusterRole deployment-clusterrole 绑定到新的 ServiceAccount cicd-token。 
