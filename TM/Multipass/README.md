

## 環境

1. Multipass



## 建立 Bridge

![image-20230518140236388](https://raw.githubusercontent.com/roger9491/Typora_note/main/img/image-20230518140236388.png)

## 設置ssh

產生 ssh

![image-20230518141659899](https://raw.githubusercontent.com/roger9491/Typora_note/main/img/image-20230518141659899.png)

配置 cloud-init.yaml

``` yaml
apt_update: true
apt_upgrade: true
ssh_import_id:
- gh:roger9491
 
 # - gh:roger9491 效果為從 帳戶拿取所有公鑰
```



啟動 實例

``` sh
multipass launch --name 'VM' --network VM  --cpus 2 --memory 4G --cloud-init=C:\Multipass\cloud-init.yaml
```

刪除 VM

``` sh
multipass delete <vm name>
# 永久刪除
multipass purge
```

