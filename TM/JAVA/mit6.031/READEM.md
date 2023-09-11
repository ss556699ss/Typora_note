
## Avoiding Debugging

善用不可變類型

## 定位錯誤

```java
/**
 * @param x  requires x >= 0
 * @return approximation to square root of x
 */
public double sqrt(double x) { 
    if (! (x >= 0)) throw new IllegalArgumentException("required x >= 0, but was: " + x);
    ...
}
```
即使注釋說明，也可以加入 `defensive programming`， 即早發現錯誤

