Naming conventions in Java

| APPLICATION           | NAME CONVENTIONS           | EXAMPLES                  |
| --------------------- | -------------------------- | ------------------------- |
| static final 全局常量 | Screaming Snake Case       | ALL_CAPS_WITH_UNDERSCORES |
| 局部變量              | Lower Camel Case, 避免縮寫 | firstName                 |
| 方法                  | Lower Camel Case, 動詞短語 | getDate, isUpperCase      |
| 避免使用全局變量      |                            |                           |



規格說明

​	目的: 使得使用者只需閱讀`規格說明`，不須了解其程式碼就能直接使用。
​	效果: 解偶

行為等價

​	實現的程式碼必須與規格說明的行為等價



規格說明的結構

兩個條款

1. 前置條件. requires
2. 後置條件, effects

必須先符合前置條件，這個方法才會符合後置條件

如果不符合前置條件，方法可以回傳任意結果

``` java
/**
 * Find a value in an array.
 * @param: 撰寫 requires
 * @param arr array to search, requires that val occurs exactly once
 *            in arr
 * @param val value to search for
 * @return: 撰寫 effects
 * @return index i such that arr[i] = val
 */
```

避免 Null， 使用 `@NonNull`，不是原始型態應該都要檢查

``` java
static boolean addAll(@NonNull List<T> list1, @NonNull List<T> list2)
```

lit1
