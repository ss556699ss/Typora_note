# SQL 學習

## 前言

書本: SQL必之必會

為了練習書本例題，先下載腳本創建

https://forta.com/wp-content/uploads/books/0672336073/TeachYourselfSQL_MicrosoftSQLServer.zip

### 1. 建立資料庫

![image-20220714211713777](SQL學習.assets/image-20220714211713777.png)

### 2. 新增查詢

![image-20220714212621712](SQL學習.assets/image-20220714212621712.png)





### 3.貼上網址提供的create裡的內容，創建表



![image-20220714212705976](SQL學習.assets/image-20220714212705976.png)

### 4. 寫入數據

![image-20220714213055015](SQL學習.assets/image-20220714213055015-16578055323911.png)

### 5. 測試

![image-20220714213228446](SQL學習.assets/image-20220714213228446.png)

## 1.排序檢索數據

在DBMS裡面我們不應該假定認為檢所出的數據是有排序的。

### 1. 排序數據

使用ORDER BY進行排序，此子句要在最後一條子句。

![image-20220714214405782](SQL學習.assets/image-20220714214405782.png)





### 2. 按多列進行排序

![image-20220714215028663](SQL學習.assets/image-20220714215028663.png)



### 3. 融合降序排序

先按照price價格由高到低，再以名稱a~z排序

![image-20220714215445281](SQL學習.assets/image-20220714215445281.png)

## 2. 過濾數據

### 1. 使用where子句

![image-20220714220306473](SQL學習.assets/image-20220714220306473.png)

### 2. where 子句操作符

![image-20220714220537648](SQL學習.assets/image-20220714220537648.png)

每一個DBMS可能不太一樣

#### 1. 判斷字符串

​	查找指定id

​		![image-20220714220945157](SQL學習.assets/image-20220714220945157.png)

####2. 範圍值檢查

使用BETWEEN操作符

​	![image-20220714221134966](SQL學習.assets/image-20220714221134966.png)













