# DFS

---

##　遞迴複習

遞迴

  函式 呼叫自己

``` python
def a(c):
    c += 1
    print(c)
    if c > 3:
        return
    a(c)
    print(c)
a(0)
```



印出 0 ~ 10 間格為2

``` python
'''
0
2
4
6
8
10
'''

def s(n):
    print(n)
    n+=2
    if n>10:
        return
    s(n)
s(0)
```



用遞迴

1 + 2 + .. +  100

``` python
'''
(1) 邊界值 n == 1 return n
(2) 遞迴關係式
    原本的問題 和 比自己小的問題 之間的關係
a(n): 1+..+ n
a(2): 1+2
a(3): 1 + 2 + 3
a(4): 1 + 2 + 3 + 4
a(4) = a(3) + 4
a(5) = a(4) + 5
a(n) = a(n-1) + n 遞迴關係式


1 + 2 + 3 + 4 + 5   5階
1 + 2 + 3 + 4   4階

f(5): 回傳 5階
f(4): 回傳 4階

f(5) = f(4) + 5
f(4) = f(3) + 4
f(3) = f(2) + 3
f(2) = f(1) + 2
f(1) = 1

f(n) = f(n-1) + n

if n == 1 return 1
'''
def f(n):   #5
    if n == 1:
        return 1
    return f(n-1) + n
print(f(5))


```



輸入一個數字 n

用遞迴 1+ ..+n

中間要印出過程

最後印出 1+ ..+n 結果

``` python
'''
5

1
3
6
10
15
15
'''
def a(n):
    if n==1:

        return n
    c=a(n-1)+n
    print(c)
    return c

n=int(input())
a(n)
```

---

## DFS深度優先搜索

真的模仿我們走路情況

解決的問題

(1) 兩點之間有沒有路徑

(2) 走完所有的點



遍歷所有的點

``` python
'''
dict={0:[2,3],2:[10],3:[4,5],5:[6]}
'''
dict={0:[2,3],2:[10],3:[4,5],5:[6]}
def a(n):   #a(0)   a(3)
    print(n)
    if n not in dict:
        return
    else:
        for i in dict[n]:#2,3
            a(i)    #a(3)

a(0)
```



承上題 印出 0 到 葉節點的路徑

``` python
'''
dict={0:[2,3],2:[10],3:[4,5],5:[6]}
'''
dic = {0:[2,3],2:[10],3:[4,5],5:[6]}

def f(n):
    temp.append(n)
    if n not in dic:
        print(temp)
        return
    else:
        for i in dic[n]:
            f(i)
            del temp[-1]
        
temp = []
f(0)
print(temp)
```



有向圖

改無向圖
遍歷

```python
'''
dict={0:[2,3],2:[10],3:[4,5],5:[6],2:[10,0]}
'''

dic = {0:[2,3],2:[0,10],3:[4,5],5:[3,6],10:[2],4:[3],6:[5]}

def f(n):   #0  2
    print(n)
    record.append(n)
    for i in dic[n]:    #2,3    0,10
        if i not in record:
            f(i)    
record = []
f(0)
```



dfs 搜索所有點

``` python
def dfs(i,j):
    print("索引值",i,j)
    print(matrix1[i][j])
    print(record)
    # input()

    for x in dir:
        now_i = i + x[0]
        now_j = j + x[1]
        if 0 <= now_i < 3 and 0 <= now_j < 3:
            if matrix1[now_i][now_j] not in record:
                record.append(matrix1[now_i][now_j])    #紀錄
                dfs(now_i,now_j)    #走這條路

matrix1 = [[1,2,3],
           [4,5,6],
           [7,8,9],]
record = [1]    #用來記錄你走過路
#       下     右    上     左
dir = [[1,0],[0,1],[-1,0],[0,-1]]
dfs(0,0)
```

---

`作業:

​	獨眼怪走迷宮1



練習題

血緣關係   40% (不用全過)

https://zerojudge.tw/ShowProblem?problemid=b967

樹狀圖分析  90% (不用全過)

https://zerojudge.tw/ShowProblem?problemid=c463



---

