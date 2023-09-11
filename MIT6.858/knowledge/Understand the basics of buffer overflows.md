
To Read [Smashing the Stack in the 21st Century](https://thesquareplanet.com/blog/smashing-the-stack-21st-century/)


## Pre-knowledge
在x86-64架構中，有許多寄存器，可分為幾個不同的類型。以下是一些主要的寄存器類型和代表性的寄存器：

1. **通用寄存器**:
   - `RAX`: 累加器寄存器，用於算數運算
   - `RBX`: 基址寄存器，用於地址計算
   - `RCX`: 計數寄存器，常用於迴圈計數
   - `RDX`: 資料寄存器，用於I/O操作和乘法/除法
   - `RSI`: 來源索引寄存器，用於字串和記憶體操作
   - `RDI`: 目的索引寄存器，用於字串和記憶體操作，通常存放函數的第一個參數
   - `RBP`: 基指針寄存器，用於堆疊框架
   - `RSP`: 堆疊指針寄存器，指向當前堆疊頂部
   - `R8`到`R15`: 額外的通用寄存器

```asm
movq    %rdi, -24(%rbp)   %%rdi 的值儲存到 rbp偏移-24的位置%%
```

