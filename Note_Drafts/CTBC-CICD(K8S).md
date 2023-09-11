
tool: 
使用部門的鏡像倉庫: [Harbor](https://goharbor.io/)
k8s
tekton

---
## tekton

### 安裝

https://tekton.dev/docs/getting-started/tasks/

tekton dashboard
https://tekton.dev/docs/dashboard/install/

tekton cli
https://tekton.dev/docs/cli/

kaniko 構建鏡像倉庫
```sh
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/kaniko/0.6/kaniko.yaml -n ctbc
```

git-clone
```sh
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.9/git-clone.yaml -n ctbc
```

Argo CD
https://argo-cd.readthedocs.io/en/stable/?_gl=1*srmjev*_ga*MTExMjY3MzM3Ny4xNjkyNTk1MjQ1*_ga_5Z1VTPDL73*MTY5MzQ2MTI1Ni4zLjEuMTY5MzQ2MTI2My4wLjAuMA..
```sh
```
### 測試

在minikube上

暴露 `tekton dashboard`


暴露 `event-listerner`
```sh
kubectl port-forward service/el-hello-listener 8080
```

測試觸發器
```sh
curl -v \
   -H 'content-Type: application/json' \
   -d '{"revision": "master"}' \
   http://localhost:8080
```

暴露 argocd
```sh
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
### docker login
```sh
docker login <ip>

# success..

cat ~/.docker/config.json | base64 -w0


dyna.ctbc002@gmail.com
Dyna2@ctbc
```



發布branch
1. build
2. cypress
3. cd

e2e 測試環境撰寫
1. 每個人都有各自的命名空間，並使用rbac為其kubectl 配置，只部屬database
2. 部屬方式寫在cypress代碼裡面