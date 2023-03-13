![image-20230109222727512](C:\Users\user\AppData\Roaming\Typora\typora-user-images\image-20230109222727512.png)

查看所使用的 kind 屬於哪一個 apiVersion

kubectl api-resources

查看所有的支援的資源

kubectl log



試試看探針

# 常用指令

## 印出加入worker node

``` sh
kubeadm token create --print-join-command
```

