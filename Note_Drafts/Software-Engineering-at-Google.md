
# Software Engineering at Google

https://software-engineering-at-google.gh.miniasp.com/#/

---

## 第十二章單元測試

>單元的定義
> * 應該視為公共API 如果一個方法或類別的存在只是為了支援一兩個其他的類（即，它是一個 "輔助類"），它可能不應該被認為是自己的單元，它的功能測試應該透過這些類進行，而不是直接測試它。
> * 如果一個包或類被設計成任何人都可以存取，而不需要諮詢其所有者，那麼它幾乎肯定構成了一個應該直接測試的單元，它的測試以使用者的方式存取該單元。

單元測試應該把重點放在行為上的測試，模仿使用者進行行為測試，也就是所謂的黑盒測試，這樣做的好處就是除非行為上改變不然這份測試代碼可以持續存在。

### [Test Behaviors, Not Methods 測試行為，而不是方法](https://software-engineering-at-google.gh.miniasp.com/#/zh-cn/Chapter-12_Unit_Testing/Chapter-12_Unit_Testing?id=test-behaviors-not-methods-%e6%b8%ac%e8%a9%a6%e8%a1%8c%e7%82%ba%ef%bc%8c%e8%80%8c%e4%b8%8d%e6%98%af%e6%96%b9%e6%b3%95)
面相方法的測試通常以方法命名，例如 testFunctionName
一個好的技巧是使用 "should"，Ex shouldNotAllowWithdrawalsWhenBalanceIsEmpty的 BankAccount 類別。
如果使用到 and 則須加注意是不是涵蓋多個行為
