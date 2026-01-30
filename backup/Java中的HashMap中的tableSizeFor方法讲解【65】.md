
# 01 tableSizeFor的作用

> java中HashMap中的下面的函数的作用是什么，给出几个案例
>     static final int tableSizeFor(int cap) {
>         int n = cap - 1;
>         n |= n >>> 1;
>         n |= n >>> 2;
>         n |= n >>> 4;
>         n |= n >>> 8;
>         n |= n >>> 16;
>         return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
>     }

`tableSizeFor` 方法的作用是：**返回大于或等于给定容量 `cap` 的最小的 2 的幂次方数**（即找到最接近且不小于 cap 的 2^n）。

HashMap 要求数组长度必须是 2 的幂次方，这样才能通过 `(n - 1) & hash` 高效计算索引。

## 工作原理

通过一系列无符号右移和按位或操作，将最高位的 `1` 及其右侧所有位都变为 `1`，最后加 `1` 得到 2 的幂次方：

```java
static final int tableSizeFor(int cap) {
    int n = cap - 1;           // 先减1，防止cap本身就是2的幂时结果翻倍
    n |= n >>> 1;              // 将最高位1向右传播1位
    n |= n >>> 2;              // 将最高位1向右传播2位  
    n |= n >>> 4;              // 将最高位1向右传播4位
    n |= n >>> 8;              // 将最高位1向右传播8位
    n |= n >>> 16;             // 将最高位1向右传播16位
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```

## 案例演示

| 输入 (cap) | 二进制过程 | 输出 (返回值) | 说明 |
|-----------|-----------|--------------|------|
| **10** | 9(1001) → 右移或操作后变为 15(1111) → +1 | **16** | 10不是2的幂，返回最近的16 (2^4) |
| **16** | 15(1111) → 保持 15 → +1 | **16** | 16本身是2的幂，返回16 |
| **17** | 16(10000) → 右移或操作后变为 31(11111) → +1 | **32** | 17不是2的幂，返回最近的32 (2^5) |
| **1** | 0 → 保持 0 → +1 | **1** | 1是2的幂 (2^0) |
| **0** | -1(全1) → 判断 n<0 | **1** | 非正数返回1 |
| **-5** | -6 → 判断 n<0 | **1** | 负数返回1 |
| **1073741824** (2^30) | 1073741823 → 操作后变为 1073741823 → +1 | **1073741824** | 正好是最大容量的一半 |
| **1073741825** (2^30+1) | 超出 MAXIMUM_CAPACITY (2^30) | **1073741824** | 超过最大值则返回最大值 |

### 详细计算示例（cap = 10）

```java
cap = 10
n = cap - 1 = 9              // 二进制: 0000 0000 0000 0000 0000 0000 0000 1001

n |= n >>> 1;                // 9 >>> 1 = 4 (0100)
                             // 1001 | 0100 = 1101 (13)

n |= n >>> 2;                // 13 >>> 2 = 3 (0011)  
                             // 1101 | 0011 = 1111 (15)

n |= n >>> 4;                // 15 >>> 4 = 0
                             // 1111 | 0000 = 1111 (15)
                             
// 后续 >>> 8 和 >>> 16 结果不变，n 保持 15

return n + 1;                // 返回 16 (2^4)
```

**实际应用场景**：当你 `new HashMap<>(10)` 时，内部实际创建的数组长度是 **16** 而不是 10，确保哈希表性能最优。



# 02 tableSizeFor与之类似的实现

> 继续上述方法，这个方法还可以写成什么形式（越多越好），为什么jdk8和jdk17中的实现方式不一样，jdk17中的实现
>     static final int tableSizeFor(int cap) {
>         int n = -1 >>> Integer.numberOfLeadingZeros(cap - 1);
>         return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
>     }

## 其他实现形式

### 1. 使用 `Integer.highestOneBit`（最简洁）
```java
static final int tableSizeFor(int cap) {
    int n = cap - 1;
    if (n <= 0) return 1;
    if (n >= MAXIMUM_CAPACITY) return MAXIMUM_CAPACITY;
    return Integer.highestOneBit(n) << 1;
}
```

### 2. 使用位长度计算
```java
static final int tableSizeFor(int cap) {
    if (cap <= 1) return 1;
    if (cap > MAXIMUM_CAPACITY) return MAXIMUM_CAPACITY;
    // 32 - 前导零个数 = 有效位数，1 << 位数 = 2^位数
    return 1 << (32 - Integer.numberOfLeadingZeros(cap - 1));
}
```

### 3. 循环展开式（通用模板）
```java
static final int tableSizeFor(int cap) {
    int n = cap - 1;
    for (int shift = 1; shift < 32; shift <<= 1) {
        n |= n >>> shift;
    }
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```

### 4. 数学计算版（不推荐用于生产）
```java
static final int tableSizeFor(int cap) {
    if (cap <= 1) return 1;
    if (cap > MAXIMUM_CAPACITY) return MAXIMUM_CAPACITY;
    return (int) Math.pow(2, Math.ceil(Math.log(cap - 1) / Math.log(2)));
}
```

### 5. 查表法（适用于小范围）
```java
static final int[] POW2_TABLE = {
    1, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 
    2048, 4096, 8192, 16384, 32768 // ... 扩展到 MAXIMUM_CAPACITY
};

static final int tableSizeFor(int cap) {
    if (cap <= 1) return 1;
    if (cap > MAXIMUM_CAPACITY) return MAXIMUM_CAPACITY;
    // 二分查找合适的 2 的幂
    int lo = 0, hi = POW2_TABLE.length - 1;
    while (lo < hi) {
        int mid = (lo + hi) >>> 1;
        if (POW2_TABLE[mid] < cap) lo = mid + 1;
        else hi = mid;
    }
    return POW2_TABLE[lo];
}
```

---

## JDK8 vs JDK17 实现差异分析

### JDK8 实现（经典位运算）
```java
int n = cap - 1;
n |= n >>> 1;
n |= n >>> 2;
n |= n >>> 4;
n |= n >>> 8;
n |= n >>> 16;
```

### JDK17 实现（前导零计算）
```java
int n = -1 >>> Integer.numberOfLeadingZeros(cap - 1);
```

### 为什么 JDK17 改变了？

#### 1. **硬件指令支持**
现代 CPU（x86 的 `LZCNT` 指令，ARM 的 `CLZ`）原生支持**前导零计数**操作，`Integer.numberOfLeadingZeros()` 会被编译为单条硬件指令，而 JDK8 的 5 次移位+或运算需要多条指令。

#### 2. **代码简洁性**
JDK17 版本将 6 行位运算压缩为 1 行，逻辑更清晰：
- `-1` 是 `0xFFFFFFFF`（32 位全 1）
- 右移 `leadingZeros` 位后，保留的低位全为 1（如 `1111...`）
- 加 1 后自然得到 `10000...`（2 的幂）

**示例计算（cap = 10）：**
```java
cap - 1 = 9                    // 二进制: 0000...0000 1001
Integer.numberOfLeadingZeros(9) // 返回 28（前面有28个0）
-1 >>> 28                      // 0xFFFFFFFF >>> 28 = 0xF (15)
return 15 + 1                  // 16
```

#### 3. **JVM 优化**
现代 JVM（HotSpot C2 编译器）对标准库方法的 intrinsic 优化更好，`numberOfLeadingZeros` 被识别为 intrinsic 方法，直接映射到 CPU 指令，消除了方法调用开销。

#### 4. **可移植性**
虽然 JDK8 的位运算在所有平台表现一致，但 JDK17 依赖的 `numberOfLeadingZeros` 在各平台都有高效的硬件/软件实现，整体性能更优。

### 性能对比

| 实现方式 | 指令数（x86） | 延迟 | 适用场景 |
|---------|-------------|------|---------|
| JDK8（5次位运算） | ~10 条 | 较高 | 旧 JVM、无 LZCNT 的老 CPU |
| JDK17（LZCNT） | 1-2 条 | 极低 | 现代 CPU + JDK9+ |

**结论**：JDK17 的改变是为了利用现代硬件特性，在保持功能等价的前提下提升性能并简化代码。

