# 個人Github 版本控制

```mermaid
gitGraph
    commit tag: "v0.0"
    branch develop
    checkout develop
    branch feature
	checkout feature
	#開發代碼 
   	commit id:"1231"
	# 開發完了準備更新回 develop
	checkout develop
    # pull 遠端代碼到 develop
    checkout feature
    # 如果 develop 有更新 
    # rebase devlop 使得 feature為最新 (解決衝突)
    checkout develop
    merge feature
    # 需要上線至主功能
	checkout main
    merge develop tag: "v0.1"
    
 
  
```

* master : 穩定上線分支
* develop: 功能開發時分支，但具體開發功能時要另外開分支，這個分支主要存放不是正式版本的專案
* feature: 當要開發一個新功能時，開的分支

## 專案初始時

1. 專案初始時先在 master  tag  `v0.0`
2. 建立devlop分支

## 開發功能時

1. 在devlop上開分支feature