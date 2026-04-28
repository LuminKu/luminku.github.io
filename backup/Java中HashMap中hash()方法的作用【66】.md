#01 Java中HashMap中hash()方法的作用

> HashMap中hash()方法的作用，以及为什么用到了算术右移而不是逻辑右移
>     static final int hash(Object key) {
>         int h;
>         return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
>     }

`hash()` 方法的作用是：**扰动函数（perturbation function）**，目的是让 `hashCode()` 的高 16 位也参与索引计算，减少哈希冲突。

但首先需要澄清一个概念：**代码中使用的是逻辑右移（无符号右移 `>>>`），而非算术右移（`>>`）**。

---

## 1. hash() 方法的作用

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

### 功能拆解：
1. **处理 null key**：返回 0，对应数组索引 0 的位置
2. **扰动计算**：`(h = key.hashCode()) ^ (h >>> 16)`
   - 将 32 位 hashCode 的高 16 位与低 16 位进行**异或**运算
   - 让高位的信息"扩散"到低位，参与最终的索引定位

### 为什么需要扰动？
HashMap 通过 `(n - 1) & hash` 计算索引（n 是数组长度，为 2 的幂）。
- 如果 n 较小（如默认 16），`(n-1)` 是 `0x0000_000F`，只有低 4 位有效
- 如果不扰动，高位信息完全被忽略，容易导致冲突（如多个 key 只有高位不同，低位相同）

---

## 2. 为什么用逻辑右移 `>>>` 而非算术右移 `>>`？

### 概念澄清：
- **`>>>`（逻辑右移/无符号右移）**：高位补 **0**
- **`>>`（算术右移）**：高位补**符号位**（正数补 0，负数补 1）

### 关键原因：**避免负数时符号位扩展破坏高位信息**

hashCode() 返回的是 `int`，**可能是负数**（最高位为 1）。

#### 反例：如果用算术右移 `>>`
假设 `hashCode = 0x8000_1234`（负数，最高位为 1）：
```java
h >> 16    // 算术右移：符号位是1，高位补1
// 结果：0xFFFF_8000（高16位全变成1）

h ^ (h >> 16)  
// 0x8000_1234 ^ 0xFFFF_8000 = 0x7FFF_9234
// 问题：高16位信息（0x8000）被破坏了！
```

#### 正例：使用逻辑右移 `>>>`
```java
h >>> 16   // 逻辑右移：高位补0
// 结果：0x0000_8000（高16位是0，低16位是原高16位）

h ^ (h >>> 16)
// 0x8000_1234 ^ 0x0000_8000 = 0x8000_9234
// 正确：原高16位（0x8000）与低16位（0x1234）完成了异或
```

### 总结对比

| 右移方式 | 符号位处理 | 对负数 hashCode 的影响 | 结果 |
|---------|-----------|---------------------|------|
| `>>` (算术) | 补符号位(1) | 高16位全变1 | 高位信息丢失，冲突增加 |
| `>>>` (逻辑) | 补0 | 高16位保持为0 | 正确保留高位信息，均匀分布 |

---

## 3. 实际案例演示

```java
// 案例1：正数 hashCode = 0x0A0B_0C0D
int h = 0x0A0B_0C0D;
// h >>> 16 = 0x0000_0A0B
// h ^ (h >>> 16) = 0x0A0B_0606
// 高16位(0A0B)与低16位(0C0D)混合

// 案例2：负数 hashCode = 0xF0F0_1234（某对象返回的hash）
int h = 0xF0F0_1234;
// 如果用 >> 16:  得到 0xFFFFF0F0（错误！高16位被符号扩展）
// 如果用 >>> 16: 得到 0x0000F0F0（正确！）
// 最终异或结果：0xF0F0_E2C4（保留了F0F0的信息）
```

**结论**：使用 `>>>` 逻辑右移是为了确保无论 hashCode 是正数还是负数，都能将其**高 16 位的原始值**完整地参与低位运算，实现真正的"高位扰动"。


# 02 在HashMap中的resize()方法中，这段(e.hash & oldCap) == 0代码的作用是什么吗，不同JDK版本中的差异（性能）

`(e.hash & oldCap) == 0` 是 HashMap 扩容时的**核心优化逻辑**，用于快速判断元素在新数组中的位置，**避免重新计算 hash**。

---

## 1. 代码作用

**判断元素在扩容后的新数组中，应该留在原索引位置，还是移动到 `原索引 + oldCap` 的位置。**

- 结果为 **0**：元素留在**原索引**位置（`newIndex = oldIndex`）
- 结果不为 **0**：元素移动到**原索引 + oldCap**（`newIndex = oldIndex + oldCap`）

---

## 2. 原理详解（基于 2 的幂特性）

HashMap 容量始终是 2 的幂（如 16, 32, 64...），二进制只有一位是 1：
- `oldCap = 16` → `0001 0000`（第 5 位为 1）
- `newCap = 32` → `0010 0000`（第 6 位为 1）

### 索引计算的本质
索引计算是 `hash & (n-1)`，即取 hash 的低 `log2(n)` 位：
- 容量 16 时：`hash & 1111`（取低 4 位）
- 容量 32 时：`hash & 11111`（取低 5 位）

### 扩容后的变化
扩容后，索引计算多用了 1 位（第 5 位，即 `oldCap` 对应的那一位）：
- 如果 hash 的第 5 位是 **0**：`hash & 01111` 与 `hash & 1111` 结果相同 → 位置不变
- 如果 hash 的第 5 位是 **1**：新索引 = 旧索引 + `10000`（16）→ 位置后移 oldCap

**`e.hash & oldCap` 正是提取 hash 在 `oldCap` 这一位上的值。**

---

## 3. 具体示例

假设 `oldCap = 16`，两个元素的 hash 值分别为：
- **Key A**: `hash = 0b...0101`（第 5 位为 0）
- **Key B**: `hash = 0b...1101`（第 5 位为 1）

```
Key A: hash & oldCap = 0b...0101 & 0001 0000 = 0  → 留在原索引 5
Key B: hash & oldCap = 0b...1101 & 0001 0000 = 16 → 移到新索引 5 + 16 = 21
```

---

## 4. 不同 JDK 版本的实现差异

### JDK 7（头插法 + 重新哈希）

```java
// JDK 7 伪代码
void resize(int newCapacity) {
    Entry[] oldTable = table;
    int oldCapacity = oldTable.length;
    // ... 创建新数组 ...
    
    for (Entry e : oldTable) {
        while (null != e) {
            Entry next = e.next;
            int i = indexFor(e.hash, newCapacity); // ❌ 重新计算索引
            e.next = newTable[i];  // 头插法
            newTable[i] = e;
            e = next;
        }
    }
}
```

**特点：**
- **重新计算 hash**：每个元素调用 `indexFor()` 重新定位
- **头插法**：新元素插入链表头部（并发环境下会产生**死循环**）
- **性能较差**：O(n) 时间，且散列计算有额外开销

### JDK 8+（尾插法 + 高低位双链表）

```java
// JDK 8 核心逻辑（简化版）
final Node<K,V>[] resize() {
    Node<K,V>[] oldTab = table;
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    int newCap = oldCap << 1;  // 容量翻倍
    
    // 遍历旧数组每个桶
    for (int j = 0; j < oldCap; ++j) {
        Node<K,V> e;
        if ((e = oldTab[j]) != null) {
            oldTab[j] = null;
            
            if (e.next == null)  // 单个节点
                newTab[e.hash & (newCap - 1)] = e;
            else if (e instanceof TreeNode)  // 红黑树
                ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
            else {  // 链表处理：拆分为 lo 和 hi 两个链表
                Node<K,V> loHead = null, loTail = null;
                Node<K,V> hiHead = null, hiTail = null;
                Node<K,V> next;
                
                do {
                    next = e.next;
                    // ✅ 关键判断：不需要重新计算 hash，只需位运算
                    if ((e.hash & oldCap) == 0) {  
                        // 低位链表：位置不变
                        if (loTail == null) loHead = e;
                        else loTail.next = e;
                        loTail = e;
                    } else {
                        // 高位链表：位置 = 原索引 + oldCap
                        if (hiTail == null) hiHead = e;
                        else hiTail.next = e;
                        hiTail = e;
                    }
                } while ((e = next) != null);
                
                // 放置两个链表
                if (loTail != null) {
                    loTail.next = null;
                    newTab[j] = loHead;  // 原位置
                }
                if (hiTail != null) {
                    hiTail.next = null;
                    newTab[j + oldCap] = hiHead;  // 原位置 + oldCap
                }
            }
        }
    }
}
```

**特点：**
- **无需重新哈希**：通过 `(e.hash & oldCap)` 快速分流
- **尾插法**：保持元素相对顺序（解决死循环问题）
- **双链表拆分**：将原链表拆成 `lo`（低位）和 `hi`（高位）两个链表，一次性整体迁移
- **红黑树优化**：树节点也有 `split` 方法，逻辑类似

---

## 5. 性能对比

| 维度 | JDK 7 | JDK 8+ |
|------|-------|--------|
| **哈希计算** | 每个元素重新计算 `hash % newCap` | 仅位运算 `hash & oldCap` |
| **链表插入** | 头插法（改变顺序） | 尾插法（保持顺序） |
| **并发安全** | 死循环风险（头插法导致环形链表） | 不会死循环（但仍非线程安全） |
| **时间复杂度** | O(n) 且常数较大 | O(n) 常数更小 |
| **缓存友好性** | 分散插入，缓存命中率低 | 双链表整体迁移，局部性好 |

### 关键性能提升点

1. **避免取模运算**：`%` 运算比 `&` 慢 10-20 倍
2. **避免重复哈希**：不需要再调用 `hash()` 方法扰动
3. **批量迁移**：JDK8 将链表整体拆分为两个链表后一次性放置，而 JDK7 是逐个插入，指针操作更频繁

**结论**：`(e.hash & oldCap) == 0` 是 JDK8 对 HashMap 扩容的重大优化，利用位运算替代取模和重哈希，将时间复杂度从 O(n×cost_of_hash) 优化到 O(n)，且保证了链表元素在扩容后的相对顺序。
