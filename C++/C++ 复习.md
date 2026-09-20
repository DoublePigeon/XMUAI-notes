
### `<algorithm>`
*注：参数中的 `first`, `last` 均为迭代器，表示左闭右开区间 `[first, last)`。`cmp` 为自定义比较函数（可选）。*
#### 1. 排序与排列
*   `sort(first, last, cmp)` : 将区间元素按升序（或cmp逻辑）排序 -> `void`
*   `stable_sort(first, last, cmp)` : 稳定排序，保持相等元素的相对位置不变 -> `void`
*   `nth_element(first, nth, last, cmp)` : 将第k小元素（即nth迭代器位置）归位，左侧均小于它，右侧均大于它 -> `void`
*   `next_permutation(first, last)` : 将区间重排为字典序的下一个排列 -> `bool` (若存在下一个返回true，若已是最大并重置为最小则返回false)
*   `prev_permutation(first, last)` : 将区间重排为字典序的上一个排列 -> `bool`
#### 2. 二分查找（前提：区间必须已有序）
*   `lower_bound(first, last, val)` : 查找第一个 **大于等于($\ge$)** `val` 的元素 -> 返回该元素的迭代器 (找不到返回`last`)
*   `upper_bound(first, last, val)` : 查找第一个 **严格大于($>$)** `val` 的元素 -> 返回该元素的迭代器 (找不到返回`last`)
*   `binary_search(first, last, val)` : 检查区间内是否存在值为 `val` 的元素 -> `bool` (存在true，不存在false)
#### 3. 其他实用算法
*   `reverse(first, last)` : 反转区间内的所有元素 -> `void`
*   `unique(first, last)` : 消除相邻的重复元素（通常在sort后使用） -> 返回去重后新序列的尾迭代器（注意：原容器大小不变，需配合`erase`使用）
*   `max_element(first, last)` : 查找区间内的最大值 -> 返回指向最大值的迭代器 (取值需加 `*`)
*   `min_element(first, last)` : 查找区间内的最小值 -> 返回指向最小值的迭代器
*   `fill(first, last, val)` : 将区间内所有元素赋值为 `val` -> `void`
### 二、 字符串与容器类操作
*注：以下 `T` 代表数据类型，`it` 代表迭代器。*
#### 1. 字符串 `<string>`
动态大小的字符序列，支持字典序比较操作符(`==`, `<`, `>` 等)。
*   `s.size()` / `s.length()` : 获取字符串中字符的个数 -> `size_t`
*   `s.substr(pos, len)` : 提取从索引 `pos` 开始、长度为 `len` 的子串 -> `string`
*   `s.find(str, pos)` : 从 `pos` 开始查找子串 `str` 的首次出现位置 -> `size_t` (找不到则返回 `string::npos`)
*   `s.append(str)` / `s += str` : 在字符串末尾追加 `str` -> `string&`
*   `s.erase(pos, len)` : 删除从 `pos` 开始的 `len` 个字符 -> `string&`
*   `to_string(val)` : (全局函数) 将数字转换为字符串 -> `string`
*   `stoi(s)` / `stoll(s)` : (全局函数) 将字符串转换为 `int` / `long long` -> `int` / `long long`
#### 2. 动态数组 `<vector>`
支持快速随机访问，尾部增删极快。
*   `v.push_back(val)` : 在数组末尾添加元素 `val` -> `void`
*   `v.pop_back()` : 移除数组末尾的最后一个元素 -> `void`
*   `v.size()` : 获取数组当前元素个数 -> `size_t`
*   `v.empty()` : 判断数组是否为空 -> `bool` (空为true)
*   `v.resize(n, val)` : 改变数组大小为 `n`，新增元素用 `val` 填充 -> `void`
*   `v.clear()` : 清空数组中的所有元素 -> `void`
*   `v.insert(it, val)` : 在迭代器 `it` 之前插入元素 `val` -> 返回指向新插入元素的迭代器
#### 3. 红黑树字典 `<map>` (包含键值对 `pair<const Key, T>`)
按Key升序自动排序，Key唯一。
*   `m[key] = val` : 插入或修改键为 `key` 的值为 `val` -> 返回 `val` 的引用
*   `m.count(key)` : 统计键为 `key` 的元素个数 -> `size_t` (map中只能是0或1，常用于判存)
*   `m.find(key)` : 查找键为 `key` 的元素 -> 返回指向该元素的迭代器 (找不到返回 `m.end()`)
*   `m.erase(key)` : 删除键为 `key` 的元素 -> `size_t` (返回成功删除的个数)
*   `m.begin()->first` / `m.begin()->second` : 访问第一个元素的键 / 值 -> `Key` / `T`
#### 4. 双向链表 `<list>`
不支持随机访问（不能用 `[]`），但任意位置增删极快。
*   `l.push_back(val)` / `l.push_front(val)` : 在尾部 / 头部插入元素 -> `void`
*   `l.pop_back()` / `l.pop_front()` : 移除尾部 / 头部元素 -> `void`
*   `l.insert(it, val)` : 在迭代器 `it` 之前插入元素 -> 返回指向新元素的迭代器
*   `l.erase(it)` : 删除迭代器 `it` 指向的元素 -> 返回指向被删元素下一个位置的迭代器
*   `l.splice(it, list2)` : 将 `list2` 的所有元素无缝转移到 `it` 之前（list2将变空） -> `void`
*   `l.sort()` : 对链表进行升序排序（注意：链表必须用成员函数sort，不能用std::sort） -> `void`
### 三、 位运算与常见用法 (Bitwise Tricks)

#### 1. 基本操作符
*   `&` (按位与), `|` (按位或), `^` (按位异或), `~` (按位取反), `<<` (左移), `>>` (右移)
#### 2. 高频位运算技巧（一行代码）
*   `x & 1` : 判断奇偶性 -> 若为1则为奇数，为0则为偶数
*   `x & (x - 1)` : 消除二进制表示中最右边的1 -> 常用于判断是否为2的幂（若是，结果为0）
*   `x & -x` : (Lowbit操作) 提取出二进制中最右边的1及其后缀0（如 `1010` 变 `0010`） -> 树状数组核心，或找最低有效位
*   `1 << n` : 计算 $2^n$ -> 用于位掩码或快速乘2
*   `x ^ x = 0` , `x ^ 0 = x` : 异或性质 -> 常用于寻找数组中唯一出现一次的数字（成对出现的数字异或后抵消为0）
#### 3. GCC内建函数（机试/算法竞赛常用，极快）
*   `__builtin_popcount(x)` : 计算32位整数 `x` 的二进制表示中 `1` 的个数 -> `int` (若是long long用 `__builtin_popcountll`)
*   `__builtin_ctz(x)` : 计算32位整数 `x` 末尾连续 `0` 的个数 -> `int` (Count Trailing Zeros)
### 五、 Tips（建议背诵）
1.  **去重标准写法**：`v.erase(unique(v.begin(), v.end()), v.end());` （必须先 `sort` 再 `unique`，`unique`只是把重复元素移到末尾，必须配合 `erase` 真正删除）。
2.  **`std::string` 找子串判断**：`if(s.find("abc") != string::npos)` （绝对不能写 `> 0`）。
3.  **万能头文件**：如果机试平台支持（如GCC），直接在第一行写 `#include <bits/stdc++.h>`，可省去记忆和写上述所有的头文件。
4.  **`lower_bound` 获取索引**：`int index = lower_bound(v.begin(), v.end(), val) - v.begin();` （迭代器相减得到整数下标）。

### 算法
### 一、 二分查找与二分答案 (Binary Search)
**思路**：机试中极少考单纯的查数（直接用`lower_bound`即可），最常考的是**“二分答案”**：当题目要求“最大化最小值”或“最小化最大值”时，如果答案具有单调性（即某个值可行，比它大/小的值必然都可行/不可行），就可以去猜测答案，然后编写一个 `check(mid)` 函数判断该答案是否合法。

**核心模板（防死循环闭区间写法 `[l, r]`）**：
#### 1. 寻找满足条件的 **最小/最左** 值（例如：第一个 $\ge x$ 的数）
```cpp
// 场景：在区间 [l, r] 中找最小的满足 check() 的数
int l = 0, r = max_val; 
while (l < r) {
    int mid = l + (r - l) / 2; // 防溢出
    if (check(mid)) {
        r = mid;    // mid 满足条件，答案可能是 mid，也可能在左边
    } else {
        l = mid + 1; // mid 不满足条件，答案一定在右边（严格大于mid）
    }
}
// 循环结束时 l == r，返回 l 即为答案
```
#### 2. 寻找满足条件的 **最大/最右** 值（例如：最后一个 $\le x$ 的数）
*注意：计算 mid 时必须 `+1`，否则会死循环！*
```cpp
// 场景：在区间 [l, r] 中找最大的满足 check() 的数
int l = 0, r = max_val;
while (l < r) {
    int mid = l + (r - l + 1) / 2; // 注意这里的 +1 ！！！
    if (check(mid)) {
        l = mid;     // mid 满足条件，答案可能是 mid，也可能在右边
    } else {
        r = mid - 1; // mid 不满足条件，答案一定在左边（严格小于mid）
    }
}
// 循环结束时 l == r，返回 l 即为答案
```
### 二、 KMP 字符串匹配
**思路**：要在文本串 `S` 中找模式串 `P`。暴力匹配在失配时会退回起点，而 KMP 通过预处理模式串 `P` 的 `nxt` 数组（最长公共前后缀），在失配时让模式串按规则“向右滑动”，避免文本串指针回退，时间复杂度 $O(|S| + |P|)$。

**实现（0-indexed 标准模板）**：
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

// 返回模式串 P 在文本串 S 中所有出现的起始下标
vector<int> KMP(string S, string P) {
    int n = S.length(), m = P.length();
    vector<int> res;
    if (m == 0) return res;

    // 1. 构建 nxt 数组
    vector<int> nxt(m, 0);
    for (int i = 1, j = 0; i < m; i++) {
        while (j > 0 && P[i] != P[j]) {
            j = nxt[j - 1]; // 失配时，回退到上一个可能匹配的位置
        }
        if (P[i] == P[j]) j++;
        nxt[i] = j;
    }

    // 2. 匹配过程
    for (int i = 0, j = 0; i < n; i++) {
        while (j > 0 && S[i] != P[j]) {
            j = nxt[j - 1]; // 失配回退
        }
        if (S[i] == P[j]) j++;
        if (j == m) { // 匹配成功
            res.push_back(i - m + 1); // 记录起始下标
            j = nxt[j - 1]; // 继续寻找下一个匹配（核心！）
        }
    }
    return res;
}
```
### 三、 贪心算法经典问题 (Greedy)
**思路**：贪心没有固定的代码模板，本质是**“排序 + 遍历”**。机试中最经典的贪心模型是**区间调度（活动安排）问题**。

**典型例题**：给 $N$ 个区间 `[start, end]`，求最多能挑选多少个**互不重叠**的区间？
**贪心策略**：将所有区间按**结束时间**从小到大排序。每次选择结束最早且与前一个选中区间不重叠的区间。（结束越早，留给后面的时间越多）。

**实现**：
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Interval {
    int start, end;
};

// 比较函数：按结束时间升序排列
bool cmp(const Interval& a, const Interval& b) {
    return a.end < b.end;
}

int maxNonOverlappingIntervals(vector<Interval>& intervals) {
    if (intervals.empty()) return 0;
    
    sort(intervals.begin(), intervals.end(), cmp);
    
    int count = 1;
    int current_end = intervals[0].end; // 记录当前选中区间的结束时间
    
    for (int i = 1; i < intervals.size(); i++) {
        // 如果下一个区间的开始时间 >= 当前选中区间的结束时间，则不重叠，可以选中
        if (intervals[i].start >= current_end) {
            count++;
            current_end = intervals[i].end; // 更新结束时间
        }
    }
    return count;
}
```
### 四、 极其有用的其他常见算法 (强推准备)
#### 2. 前缀和与差分 (Prefix Sum & Difference Array)
**场景**：
*   **前缀和**：用于频繁查询区间 `[L, R]` 内元素的和（将 $O(N)$ 降为 $O(1)$）。
*   **差分**：用于频繁对区间 `[L, R]` 内的每个数加上同一个值 `V`，最后输出变化后的数组（将 $O(N)$ 降为 $O(1)$）。

**实现（1-indexed 数组最方便处理越界）**：
```cpp
// 1. 前缀和数组 S
// S[i] 表示 a[1] 到 a[i] 的和。S[0] = 0;
// 构建：S[i] = S[i-1] + a[i];
// 查询区间 [L, R] 的和：Sum = S[R] - S[L-1];

// 2. 差分数组 D
// D[i] = a[i] - a[i-1]; (a[0]=0)
// 区间操作：让 a[L] 到 a[R] 全部加上 V。
// 操作差分数组：D[L] += V; D[R+1] -= V;
// 最后还原数组 a：a[i] = a[i-1] + D[i];
```