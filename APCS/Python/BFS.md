# BFS

---

## 圖的儲存

在電腦上儲存圖得方式有兩種

``` python
1. adjacency matrix
鄰接矩陣    (二維陣列)  少用
    查詢 有沒有路 O(1)
         子節點 O(n)    n:節點數量
    0   1   2   3   4   5
0   1   0   1   0   1   1    
1   0   1   0   0   1   1
2   1   0   1   1   1   0
3   0   0   1   1   0   0
4   1   1   1   0   1   1
5   1   1   0   0   1   1

a:
for i in range(6):
    if a[0][i] == 1:
        print(i)

2. adjacency list
鄰接串列

0: 2 4 5
1: 4 5
2: 3 0 4
3: 2
4: 0 1 2
5: 0 1 4
優勢: 快速找子節點
字典
dic = {0: [2 ,4 ,5],1: [4,5]....}

串列    索引值:節點 值:子節點
a = [[2,4,5],[4,5],[],[],....]
```







---



bfs 廣度優先搜索

核心思想 : 擴散

dict={0:[2,3],2:[10],3:[4,5],5:[6],8:[9]}

``` python
# dic : 字典 {key:value}
# if a in dic:


dic={0:[2,3],2:[10],3:[4,5],5:[6],8:[9]}

queue = [0] #看誰的周圍

while queue:    #當所有的點看完時，queue就空了
    node = queue.pop(0)     #刪除索引值0，並傳出
    print(node)

    if node in dic:
        for i in dic[node]:
            queue.append(i)
```

``` python
dic = {0:[2,3],2:[1,10],3:[4,5],5:[6],10:[9,11]}
a = [0]
while True:
    if len(a) == 0:
        break
    if a[0] in dic:
        for i in dic[a[0]]:
            a.append(i)
    v = a.pop(0)
    print(v)
```

---

輸入 n，代表接下來n個輸入

輸入 兩個數字 代表節點相連



輸出

所有0能連通的節點

``` python
'''
ex
9
0 2
0 3
2 1
2 10
10 9
10 11
3 4
3 5
5 6
'''
n = int(input())
dic = {}
for i in range(n):
    c = list(map(int,input().split()))
    a = c[0]
    b = c[1]
    if a in dic:
        dic[a].append(b)
    else:
        dic[a] = [b]
    
    if b in dic:
        dic[b].append(a)
    else:
        dic[b] = [a]

a = [0]
x = []
while True:
    if len(a) == 0:
        break
    if a[0] in dic:
        for i in dic[a[0]]:
            if i not in x:  #判斷i有沒有走過
                a.append(i)
    v = a.pop(0)
    print(v)
    x.append(v)
```



---

優化判斷 i 有沒有走過

利用索引值取值的方式

索引值:節點

值: 有 沒有

``` python
n = int(input())
dic = {}
for i in range(n):
    c = list(map(int,input().split()))
    a = c[0]
    b = c[1]
    if a in dic:
        dic[a].append(b)
    else:
        dic[a] = [b]
    
    if b in dic:
        dic[b].append(a)
    else:
        dic[b] = [a]

a = [0]
x = [0]*12
while True:
    if len(a) == 0:
        break
    if a[0] in dic:
        for i in dic[a[0]]:
            if x[i] == 0:  #判斷i有沒有走過
                a.append(i)
                x[i] = 1
    v = a.pop(0)
    print(v)
```

---

bfs 搜索所有點

``` python
def bfs(i,j):
    queue = [[i,j]]
    record = [[0]*4 for i in range(5)]
    record[0][0] = 1
    dire = [[-1,0], [1,0], [0,-1], [0,1]]

    while queue:
        node = queue.pop(0)
        print(matrix[node[0]][node[1]])
        for x in dire:
            i = node[0] + x[0]      #周圍的節點座標
            j = node[1] + x[1]
            if 0 <= i < len(matrix) and 0 <= j < len(matrix[0]):    #判斷座標合理性
                if record[i][j] == 0 and matrix[i][j] != "x":
                    queue.append([i,j])
                    record[i][j] = 1


#       上        下      左     右
# i,j = [-1,0], [1,0], [0,-1], [0,1]
matrix = [ ["s", 1 , 2 ,"x"],
           [ 3 ,"x", 4 , 5 ],
           [ 6 , 7 , 8 ,"x"],
           [ 9 ,"x", 10, 11],
           [ 12, 13, 14,'e']]
bfs(0,0)
```

---

##　最短距離

``` python
def bfs(i,j):
    queue = [[i,j]]
    path = [[0]*4 for i in range(5)]
    path[0][0] = 1
    ans = 0
    while queue:
        size = len(queue)

        while size != 0:
            node = queue.pop(0) #s
            
            print(matrix[node[0]][node[1]])
            for d in dire:
                i = node[0] + d[0]
                j = node[1] + d[1]
                if 0 <= i < 5 and 0 <= j < 4 and path[i][j] != 1:
                    if matrix[i][j] != "x":
                        path[i][j] = 1
                        queue.append([i,j])
            size -= 1
        ans += 1
matrix = [["s", 1 , 2 ,"x"],
           [ 3 ,"x", 4 , 5 ],
           [ 6 , 7 , 8 ,"x"],
           [ 9 ,"x", 10, 11],
           [ 12, 13, 14,'e']]
dire = [[1,0],[0,1],[-1,0],[0,-1]]

bfs(0,0)





```

---

## 結論

(1) 走訪所有的點

(2) 判斷兩點之間有沒有路徑

(3) 判斷兩點之間最短距離

缺點: 不好紀錄路徑 

---

## 練習題

練習題

闖關路線 bfs (最短路)
d094: Q-7-5. 闖關路線
https://judge.tcirc.tw/ShowProblem?problemid=d094

蓋步道 bfs&二分搜
https://zerojudge.tw/ShowProblem?problemid=j125

