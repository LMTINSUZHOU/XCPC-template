# STL

## 1 库函数

### 1.1 pb_ds 库

其中 `gp_hash_table` 使用的最多，其等价于 `unordered_map` ，内部是无序的。

```c++
#include <bits/extc++.h>
#include <ext/pb_ds/assoc_container.hpp>
template<class S, class T> using omap = __gnu_pbds::gp_hash_table<S, T, myhash>;
```

### 1.2 查找后继 lower\_bound、upper\_bound

`lower` 表示 $\ge$ ，`upper` 表示 $>$ 。使用前记得**先进行排序**。

```c++
//返回a数组[start,end)区间中第一个>=x的地址【地址！！！】
cout << lower_bound(a + start, a + end, x);

cout << lower_bound(a, a + n, x) - a; //在a数组中查找第一个>=x的元素下标
upper_bound(a, a + n, k) - lower_bound(a, a + n, k) //查找k在a中出现了几次
```

### 1.3 数组打乱 shuffle

```c++
mt19937 rnd(chrono::steady_clock::now().time_since_epoch().count());
shuffle(ver.begin(), ver.end(), rnd);
```

### 1.4 二分搜索 binary\_search

用于查找某一元素是否在容器中，相当于 find 函数。在使用前需要**先进行排序**。

```c++
//在a数组[start,end)区间中查找x是否存在，返回bool型
cout << binary_search(a + start, a + end, x);
```

### 1.5 批量递增赋值函数 iota

对容器递增初始化。

```c++
//将a数组[start,end)区间复制成“x，x+1，x+2，…”
iota(a + start, a + end, x);
```

### 1.6 数组去重函数 unique

在使用前需要**先进行排序**。

其作用是，对于区间 `[开始位置, 结束位置)` ，**不停的把后面不重复的元素移到前面来**，也可以说是**用不重复的元素占领重复元素的位置**。并且返回**去重后容器中不重复序列的最后一个元素的下一个元素**。所以在进行操作后，数组、容器的大小并**没有发生改变**。

```c++
//将a数组[start,end)区间去重，返回迭代器
unique(a + start, a + end);

//与earse函数结合，达到去重+删除的目的
a.erase(unique(ALL(a)), a.end());
```

### 1.7 bit 库与位运算函数 \__builtin\_

```c++
__builtin_popcount(x) // 返回x二进制下含1的数量，例如x=15=(1111)时答案为4

__builtin_ffs(x) // 返回x右数第一个1的位置(1-idx)，1(1) 返回 1，8(1000) 返回 4，26(11010) 返回 2

__builtin_ctz(x) // 返回x二进制下后导0的个数，1(1) 返回 0，8(1000) 返回 3

bit_width(x) // 返回x二进制下的位数，9(1001) 返回 4，26(11010) 返回 5
```

注：以上函数的 $\tt{}long\ long$ 版本只需要在函数后面加上 `ll` 即可（例如 `__builtin_popcountll(x)` )， $\tt{}unsigned\ long\ long$ 加上 `ull` 。

### 1.8 数字转字符串函数

`itoa` 虽然能将整数转换成任意进制的字符串，但是其不是标准的C函数，且为Windows独有，且不支持 `long long` ，建议手写。

```c++
// to_string函数会直接将你的各种类型的数字转换为字符串。
// string to_string(T val);
double val = 12.12;
cout << to_string(val);
```

```c++
// 【不建议使用】itoa允许你将整数转换成任意进制的字符串，参数为待转换整数、目标字符数组、进制。
// char* itoa(int value, char* string, int radix);
char ans[10] = {};
itoa(12, ans, 2);
cout << ans << endl; /*1100*/

// 长整型函数名ltoa，最高支持到int型上限2^31。ultoa同理。
```

### 1.9 字符串转数字

```c++
// stoi直接使用
cout << stoi("12") << endl;

// 【不建议使用】stoi转换进制，参数为待转换字符串、起始位置、进制。
// int stoi(string value, int st, int radix);
cout << stoi("1010", 0, 2) << endl; /*10*/
cout << stoi("c", 0, 16) << endl; /*12*/
cout << stoi("0x3f3f3f3f", 0, 0) << endl; /*1061109567*/

// 长整型函数名stoll，最高支持到long long型上限2^63。stoull、stod、stold同理。
```

```c++
// atoi直接使用，空字符返回0，允许正负符号，数字字符前有其他字符返回0，数字字符前有空白字符自动去除
cout << atoi("12") << endl;
cout << atoi("   12") << endl; /*12*/
cout << atoi("-12abc") << endl; /*-12*/
cout << atoi("abc12") << endl; /*0*/

// 长整型函数名atoll，最高支持到long long型上限2^63。
```

### 1.10 全排列算法 next\_permutation、prev\_permutation

在提及这个函数时，我们先需要补充几点字典序相关的知识。

> 对于三个字符所组成的序列`{a,b,c}`，其按照字典序的6种排列分别为：
> `{abc}`，`{acb}`，`{bac}`，`{bca}`，`{cab}`，`{cba}`
> 其排序原理是：先固定 `a` (序列内最小元素)，再对之后的元素排列。而 `b` < `c` ，所以 `abc` < `acb` 。同理，先固定 `b` (序列内次小元素)，再对之后的元素排列。即可得出以上序列。

$\tt{}next\_permutation$ 算法，即是按照**字典序顺序**输出的全排列；相对应的， $\tt{}prev\_permutation$ 则是按照**逆字典序顺序**输出的全排列。可以是数字，亦可以是其他类型元素。其直接在序列上进行更新，故直接输出序列即可。

```c++
int n;
cin >> n;
vector<int> a(n);
// iota(a.begin(), a.end(), 1);
for (auto &it : a) cin >> it;
sort(a.begin(), a.end());

do {
    for (auto it : a) cout << it << " ";
    cout << endl;
} while (next_permutation(a.begin(), a.end()));
```

### 1.11 字符串转换为数值函数 sto

可以快捷的将**一串字符串**转换为**指定进制的数字**。

使用方法

- `stoi(字符串, 0, x进制)` ：将一串 $\tt{}x$ 进制的字符串转换为 $\tt{}int$ 型数字。

![](https://pic-albert-li.oss-cn-nanjing.aliyuncs.com/pic/20250309141637197.png)

- `stoll(字符串, 0, x进制)` ：将一串 $\tt{}x$ 进制的字符串转换为 $\tt{}long\ long$ 型数字。
- $\tt{}stoull，stod，stold$ 同理。

### 1.12 数值转换为字符串函数 to\_string

允许将**各种数值类型**转换为字符串类型。

```c++
//将数值num转换为字符串s
string s = to_string(num);
```

### 1.13 判断非递减 is\_sorted

```c++
//a数组[start,end)区间是否是非递减的，返回bool型
cout << is_sorted(a + start, a + end);
```

### 1.14 累加 accumulate

```c++
//将a数组[start,end)区间的元素进行累加，并输出累加和+x的值
cout << accumulate(a + start, a + end, x);
```

### 1.15 迭代器 iterator

```c++
//构建一个UUU容器的正向迭代器，名字叫it
UUU::iterator it;

vector<int>::iterator it; //创建一个正向迭代器，++ 操作时指向下一个
vector<int>::reverse_iterator it; //创建一个反向迭代器，++ 操作时指向上一个
```

### 1.16 其他函数

`exp2(x)` ：返回 $2^x$ 

`log2(x)` ：返回 $\log_2(x)$

`gcd(x, y) / lcm(x, y)` ：以 $\log$ 的复杂度返回 $\gcd(|x|, |y|)$ 与 ${\tt lcm}(|x|, |y|)$ ，且返回值符号也为正数。

## 2 容器与成员函数

### 2.1 元组 tuple

```c++
//获取obj对象中的第index个元素——get<index>(obj)
//需要注意的是这里的index只能手动输入，使用for循环这样的自动输入是不可以的
tuple<string, int, int> Student = {"Wida", 23, 45000);
cout << get<0>(Student) << endl; //获取Student对象中的第一个元素，这里的输出结果应为“Wida”
```

### 2.2 数组 array

```c++
array<int, 3> x; // 建立一个包含三个元素的数组x

[] // 跟正常数组一样，可以使用随机访问
cout << x[0]; // 获取数组重的第一个元素
```

### 2.3 变长数组 vector

```c++
resize(n) // 重设容器大小，但是不改变已有元素的值
assign(n, 0) // 重设容器大小为n，且替换容器内的内容为0

// 尽量不要使用[]的形式声明多维变长数组，而是使用嵌套的方式替代
vector<int> ver[n + 1]; // 不好的声明方式
vector<vector<int>> ver(n + 1);

// 嵌套时只需要在最后一个注明变量类型
vector dis(n + 1, vector<int>(m + 1));
vector dis(m + 1, vector(n + 1, vector<int>(n + 1)));
```

### 2.4 栈  stack

栈顶入，栈顶出。先进后出。

```c++
//没有clear函数
size() / empty()
push(x) //向栈顶插入x
top() //获取栈顶元素
pop() //弹出栈顶元素
```

### 2.5 队列 queue

队尾进，队头出。先进先出。

```c++
//没有clear函数
size() / empty()
push(x) //向队尾插入x
front() / back() //获取队头、队尾元素
pop() //弹出队头元素
```

```c++
//没有clear函数，但是可以用重新构造替代
queue<int> q;
q = queue<int>();
```

### 2.6 双向队列 deque

```c++
size() / empty() / clear()
push_front(x) / push_back(x)
pop_front(x) / pop_back(x)
front() / back()
begin() / end()
[]
```

### 2.7 优先队列 priority\_queue

默认升序（大根堆），自定义排序需要重载 `<` 。

```c++
//没有clear函数
priority_queue<int, vector<int>, greater<int> > p; //重定义为降序（小根堆）
push(x); //向栈顶插入x
top(); //获取栈顶元素
pop(); //弹出栈顶元素
```

```c++
//重载运算符【注意，符号相反！！！】
struct Node {
    int x; string s;
    friend bool operator < (const Node &a, const Node &b) {
        if (a.x != b.x) return a.x > b.x;
        return a.s > b.s;
    }
};
```

### 2.8 字符串 string

```c++
size() / empty() / clear()
```

```c++
//从字符串S的S[start]开始，取出长度为len的子串——S.substr(start, len)
//len省略时默认取到结尾，超过字符串长度时也默认取到结尾
cout << S.substr(1, 12);

find(x) / rfind(x); //顺序、逆序查找x，返回下标，没找到时返回一个极大值【！建议与 size() 比较，而不要和 -1 比较，后者可能出错】
//注意，没有count函数
```

### 2.9 有序、多重有序集合 set、multiset

默认升序（大根堆），$\tt set$ 去重，$\tt multiset$ 不去重，$\mathcal O(\log N)$ 。

```c++
set<int, greater<> > s; //重定义为降序（小根堆）
size() / empty() / clear()
begin() / end()
++ / -- //返回前驱、后继

insert(x); //插入x
find(x) / rfind(x); //顺序、逆序查找x，返回迭代器【迭代器！！！】，没找到时返回end()
count(x); //返回x的个数
lower_cound(x); //返回第一个>=x的迭代器【迭代器！！！】
upper_cound(x); //返回第一个>x的迭代器【迭代器！！！】
```

特殊函数 `next` 和 `prev` 详解：

```c++
auto it = s.find(x); // 建立一个迭代器
prev(it) / next(it); // 默认返回迭代器it的前/后一个迭代器
prev(it, 2) / next(it, 2); // 可选参数可以控制返回前/后任意个迭代器

/* 以下是一些应用 */
auto pre = prev(s.lower_bound(x)); // 返回第一个<x的迭代器
int ed = *prev(S.end(), 1); // 返回最后一个元素
```

`erase(x);` 有两种删除方式：

- 当x为某一元素时，删除**所有**这个数，复杂度为 $\mathcal O (num_x+logN)$ ；
- 当x为迭代器时，删除这个迭代器。

```c++
//连续头部删除
set<int> S = {0, 9, 98, 1087, 894, 34, 756};
auto it = S.begin();
int len = S.size();
for (int i = 0; i < len; ++ i) {
    if (*it >= 500) continue;
    it = S.erase(it); //删除所有小于500的元素
}
//错误用法如下【千万不能这样用！！！】
//for (auto it : S) {
//    if (it >= 500) continue;
//    S.erase(it); //删除所有小于500的元素
//}
```

### 2.10 map、multimap

默认升序（大根堆），$\tt map$ 去重，$\tt mulitmap$ 不去重，$\mathcal O(logS)$ ，其中 $S$ 为元素数量。

```c++
map<int, int, greater<> > mp; //重定义为降序（小根堆）
size() / empty() / clear()
begin() / end()
++ / -- //返回前驱、后继

insert({x, y}); //插入二元组
[] //随机访问，multimap不支持
count(x); //返回x为下标的个数
lower_cound(x); //返回第一个下标>=x的迭代器
upper_cound(x); //返回第一个下标>x的迭代器
```

`erase(x);` 有两种删除方式：

- 当x为某一元素时，删除所有**以这个元素为下标的二元组**，复杂度为 $\mathcal O (num_x+logN)$ ；
- 当x为迭代器时，删除这个迭代器。

**慎用随机访问！**——当不确定某次查询是否存在于容器中时，不要直接使用下标查询，而是先使用 `count()` 或者 `find()` 方法检查key值，防止不必要的零值二元组被构造。

```c++
int q = 0;
if (mp.count(i)) q = mp[i];
```

慎用自带的 pair、tuple 作为key值类型！使用自定义结构体！

```c++
struct fff { 
    LL x, y;
    friend bool operator < (const fff &a, const fff &b) {
        if (a.x != b.x) return a.x < b.x;
        return a.y < b.y;
    }
};
map<fff, int> mp;
```

### 2.11 bitset

将数据转换为二进制，从高位到低位排序，以 $0$ 为最低位。当位数相同时支持全部的位运算。

```c++
// 如果输入的是01字符串，可以直接使用">>"读入
bitset<10> s;
cin >> s;

//使用只含01的字符串构造——bitset<容器长度>B (字符串)
string S; cin >> S;
bitset<32> B (S);

//使用整数构造（两种方式）
int x; cin >> x;
bitset<32> B1 (x);
bitset<32> B2 = x;

// 构造时，尖括号里的数字不能是变量
int x; cin >> x;
bitset<x> ans; // 错误构造

[] //随机访问
set(x) //将第x位置1，x省略时默认全部位置1
reset(x) //将第x位置0，x省略时默认全部位置0
flip(x) //将第x位取反，x省略时默认全部位取反
to_ullong() //重转换为ULL类型
to_string() //重转换为ULL类型
count() //返回1的个数
any() //判断是否至少有一个1
none() //判断是否全为0

_Find_fisrt() // 找到从低位到高位第一个1的位置
_Find_next(x) // 找到当前位置x的下一个1的位置，复杂度 O(n/w + count)

bitset<23> B1("11101001"), B2("11101000");
cout << (B1 ^ B2) << "\n";  //按位异或
cout << (B1 | B2) << "\n";  //按位或
cout << (B1 & B2) << "\n";  //按位与
cout << (B1 == B2) << "\n"; //比较是否相等
cout << B1 << " " << B2 << "\n"; //你可以直接使用cout输出
```

### 2.12 哈希系列 unordered

通常指代 unordered_map、unordered_set、unordered_multimap、unordered_multiset，与原版相比不进行排序。

如果将不支持哈希的类型作为 `key` 值代入，编译器就无法正常运行，这时需要我们为其手写哈希函数。而我们写的这个哈希函数的正确性其实并不是特别重要（但是不可以没有），当发生冲突时编译器会调用 `key` 的 `operator ==` 函数进行进一步判断。[参考](https://finixlei.blog.csdn.net/article/details/110267430?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-3-110267430-blog-101406104.topnsimilarv1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-3-110267430-blog-101406104.topnsimilarv1&utm_relevant_index=4)

### 2.13 对 pair、tuple 定义哈希

```c++
struct hash_pair { 
    template <class T1, class T2> 
    size_t operator()(const pair<T1, T2> &p) const { 
        return hash<T1>()(p.fi) ^ hash<T2>()(p.se); 
    } 
};
unordered_set<pair<int, int>, int, hash_pair> S;
unordered_map<tuple<int, int, int>, int, hash_pair> M;
```

### 2.14 对结构体定义哈希

需要两个条件，一个是在结构体中重载等于号（区别于非哈希容器需要重载小于号，如上所述，当冲突时编译器需要根据重载的等于号判断），第二是写一个哈希函数。注意 `hash<>()` 的尖括号中的类型匹配。

```c++
struct fff { 
    string x, y;
    int z;
    friend bool operator == (const fff &a, const fff &b) {
        return a.x == b.x || a.y == b.y || a.z == b.z;
    }
};
struct hash_fff { 
    size_t operator()(const fff &p) const { 
        return hash<string>()(p.x) ^ hash<string>()(p.y) ^ hash<int>()(p.z); 
    } 
};
unordered_map<fff, int, hash_fff> mp;
```

### 2.15 对 vector 定义哈希

以下两个方法均可。注意 `hash<>()` 的尖括号中的类型匹配。

```c++
struct hash_vector { 
    size_t operator()(const vector<int> &p) const {
        size_t seed = 0;
        for (auto it : p) {
            seed ^= hash<int>()(it);
        }
        return seed; 
    } 
};
unordered_map<vector<int>, int, hash_vector> mp;
```

```c++
namespace std {
    template<> struct hash<vector<int>> {
        size_t operator()(const vector<int> &p) const {
            size_t seed = 0;
            for (int i : p) {
                seed ^= hash<int>()(i) + 0x9e3779b9 + (seed << 6) + (seed >> 2);
            }
            return seed;
        }
    };
}
unordered_set<vector<int> > S;
```

## 3 pb_ds库探究

### 3.1 万能头文件

CodeForces在 $\tt C^{20(64)}_{++}$ 版本下无法使用 `bits`；如果需要使用 `priority_queue` 则无法使用 `using`（会和 `std` 撞名字）。

```c
#include <bits/extc.h>
using namespace __gnu_pbds;
```

***

### 3.2 优先队列（不常用）

#### 3.2.1 概述

一般使用 `pairing_heap_tag`，速度最快；`binary_heap_tag` 也大致能够接受。两者均拥有 `std::priority_queue` 的性质和函数，默认从大到小排序。

```c
__gnu_pbds::priority_queue<int> p; // 标准写法
__gnu_pbds::priority_queue<int, greater<int>, __gnu_pbds::pairing_heap_tag> q; // 转换为小根堆
```

#### 3.1.2 迭代器的使用

允许使用迭代器，输出的是在堆里的状态（注意，并不是默认的从小到大）。

```c
__gnu_pbds::priority_queue<int> p;
p.push(12);
p.push(14);
p.push(14);
for (auto it : p) {
    cout << it << " "; // 输出：14 12 14
}
```

#### 3.1.3 复杂度

结论：时间复杂度差于 `std::priority_queue`。

- CodeForces在 $\tt C^{20(64)}_{++}$ 版本下使用 `std::priority_queue` 优化 djikstra [耗时124ms](https://codeforces.com/contest/1433/submission/223590243)，使用 `__gnu_pbds::priority_queue` 则 [耗时156ms](https://codeforces.com/contest/1433/submission/223591776)；使用 $\tt C^{17(64)}_{++}$ 版本耗时更长，为 [171ms](https://codeforces.com/contest/1433/submission/223591492) 。
- Atcoder在 $\tt C^{20(gcc 12.2)}_{++}$ 版本下基本同上。

***

### 3.3 平衡树（功能最多）

#### 3.3.1 概述

一般使用 `rb_tree_tag`，拥有 `std::set` 的性质和函数，速度较快。

```c
tree<int, null_type, less<int>, rb_tree_tag> p; // 标准写法

p.insert(12);
p.insert(12);
p.insert(19);
p.insert(2);
for (auto it : p) {
    cout << it << " "; // 输出：2 12 19
}
```

#### 3.3.2 关于第二参数

其中第二个参数如果不写内置关键字 `null_type`，则能够实现 `std::map` 的功能，该参数即等同于 `std::map` 的第二参数（但是效率巨低无比）。

```c
tree<int, int, less<int>, rb_tree_tag> p;

p[12]++;
p[12]++;
p[19]++;
p[2]++;
for (auto [a, b] : p) {
    cout << a << " " << b << endl;
}
/* 输出：
2 1
12 2
19 1
*/
```

如果第二关键字为 `char` 等其他类型，则需要手动书写一个哈希，否则输出会出现问题。

#### 3.3.3 关于第三参数

第三个参数可以使用 `_equal` 后缀以允许插入重复元素，进而替代 `std::multiset`。

```c
tree<int, null_type, less<int>, rb_tree_tag> P;
tree<int, null_type, less_equal<int>, rb_tree_tag> p;

P.insert(12), p.insert(12);
P.insert(12), p.insert(12);
P.insert(19), p.insert(19);
P.insert(2), p.insert(2);
for (auto it : P) {
    cout << it << " ";
}
cout << endl;
for (auto it : p) {
    cout << it << " ";
}
/* 此时：
P = [2 12 19]
p = [2 12 12 19]
*/
```

在此操作之后，函数的性质发生了一些变化，`find` 函数不再能正确使用。

```c
if (P.find(12) != P.end()) {
    cout << "AC!\n";
}
if (p.find(12) == p.end()) {
    cout << "WA!\n";
}
/* 输出：
AC!
WA! 
*/
```

`lower_bound` 和 `upper_bound` 函数的实现将会被翻转：

- `lower_bound(x)`：找到 $> x$ 的第一个迭代器；
- `upper_bound(x)`：找到 $\ge x$ 的第一个迭代器。

```c
cout << *P.upper_bound(12) << " " << *P.lower_bound(12) << endl;
cout << *p.upper_bound(12) << " " << *p.lower_bound(12) << endl;
/* 输出：
19 12
12 19 
*/
```

#### 3.3.4 关于第五参数

其特殊之处在于最后一个可选参数类：内置关键字 `tree_order_statistics_node_update` 可以同步记录子树大小，使得你可以使用 `find_by_order()` 和 `order_of_key()` 函数。

```c
tree<int, null_type, less<int>, rb_tree_tag, tree_order_statistics_node_update> p;
```

#### 3.3.5 手写可选参数类

可以手写函数实现不同的功能，大模板如下：

```c
template<class Node_CItr, class Node_Itr, class Cmp_Fn, class _Alloc>
struct myNodeUpdate {
    virtual Node_CItr node_begin() const = 0;
    virtual Node_CItr node_end() const = 0;
    typedef int metadata_type;
};
```

可以重载 `operator()` 函数，将节点的信息更新为其两个子节点的信息之和。常用函数展示：

- `get_l_child`：获取左儿子；
- `get_r_child`：获取右儿子；
- `get_metadata`：获取节点额外信息。

下方展示一个计算子节点值之和的例子，可以类比于求解 `std::map` 前缀和、区间之和：

```c
template<class Node_CItr, class Node_Itr, class Cmp_Fn, class _Alloc>
struct myNodeUpdate {
    virtual Node_CItr node_begin() const = 0;
    virtual Node_CItr node_end() const = 0;
    typedef int metadata_type; // 声明节点上记录的额外信息的类型

    void operator()(Node_Itr it, Node_CItr end_it) {
        Node_Itr l = it.get_l_child(), r = it.get_r_child();
        int left = 0, right = 0;
        if (l != end_it) left = l.get_metadata();
        if (r != end_it) right = r.get_metadata();
        // (*it)->second 为当前节点 it 的 mapped_value
        const_cast<metadata_type &>(it.get_metadata()) = left + right + (*it)->second;
    }
    int prefix_sum(int x) {
        int ans = 0;
        Node_CItr it = node_begin();
        while (it != node_end()) {
            Node_CItr l = it.get_l_child(), r = it.get_r_child();
            if (Cmp_Fn()(x, (*it)->first)) {
                it = l;
            } else {
                ans += (*it)->second;
                if (l != node_end()) ans += l.get_metadata();
                it = r;
            }
        }
        return ans;
    }
    int interval_sum(int l, int r) {
        return prefix_sum(r) - prefix_sum(l - 1);
    }
};
```

测试结果如下：

```c
tree<int, int, less<int>, rb_tree_tag, myNodeUpdate> p;
p[2] = 100;
p[3] = 1111;
p[9] = 1;
cout << p.prefix_sum(-1) << endl;
cout << p.prefix_sum(8) << endl;
cout << p.interval_sum(-500, 99) << endl;
cout << p.size() << endl;
/* 输出：
0
1211
1212
3
*/
```

#### 3.3.6 复杂度

个人尚未做过相关测试，引用论文中图表：

<center>
    <img style="border-radius: 0.3125em; box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" src="https://s2.loli.net/2023/10/12/unZNEMyVbplDoKA.png" width="70%" />
    <div style="border-bottom: 1px solid #d9d9d9; display: inline-block; color: #999;">论文相关测试结果</div>
</center>


***

### 3.4 哈希表（优化较大）

#### 3.4.1 对应头文件

```c++
#include <ext/pb_ds/assoc_container.hpp>
#define mymap __gnu_pbds::gp_hash_table
```

#### 3.4.2 概述

一般使用 `gp_hash_table`，速度较快（使用查探法哈希）；`cc_hash_table` 速度稍慢（使用拉链法哈希），但均快于 `map` 和 `unordered_map`。

```c
gp_hash_table<int, int> dic; // 标准写法

struct myhash {
    static uint64_t splitmix64(uint64_t x) {
        x += 0x9e3779b97f4a7c15;
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9;
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb;
        return x ^ (x >> 31);
    }
    size_t operator()(uint64_t x) const {
        static const uint64_t FIXED_RANDOM =
            chrono::steady_clock::now().time_since_epoch().count();
        return splitmix64(x + FIXED_RANDOM);
    }
};
gp_hash_table<int, int, myhash> dic; // 支持自定义随机哈希种子
```

#### 3.4.3 复杂度

结论：时间复杂度优于 `map` 和 `unordered_map` 。

- CodeForces在 $\tt C^{20(64)}_{++}$ 版本下使用 `std::map` [耗时920ms](https://codeforces.com/contest/1225/submission/223593424)，使用自定义随机哈希种子 `std::unordered_map` [耗时842ms](https://codeforces.com/contest/1225/submission/223593693)；使用 `gp_hash_table` [耗时764ms](https://codeforces.com/contest/1225/submission/223594104)；使用自定义随机哈希种子 `gp_hash_table` [耗时780ms](https://codeforces.com/problemset/submission/1225/223594473) 。

<center>
    <img style="border-radius: 0.3125em; box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" src="https://s2.loli.net/2023/10/12/Vwegsz9aAFdKjyo.png" width="70%" />
    <div style="border-bottom: 1px solid #d9d9d9; display: inline-block; color: #999;">论文相关测试结果</div>
</center>


<center>
    <img style="border-radius: 0.3125em; box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" src="https://s2.loli.net/2023/10/12/i2DthXdVPJueRvE.png" width="70%" />
    <div style="border-bottom: 1px solid #d9d9d9; display: inline-block; color: #999;">CodeForces相关测试结果</div>
</center>

## 4 程序标准化

### 4.1 使用 Lambda 函数

- `function` 统一写法

需要注意的是，虽然 `function` 定义时已经声明了返回值类型了，但是有的时候会出错（例如，声明返回 `long long` 但是返回 `int` ，原因没去了解），所以推荐在后面使用 `->` 再行声明一遍。

```c++
function<void(int, int)> clac = [&](int x, int y) -> void {
};
clac(1, 2);

function<bool(int)> dfs = [&](int x) -> bool {
    return dfs(x + 1);
};
dfs(1);
```

- `auto` 非递归写法

不需要使用递归函数时，直接用 `auto` 替换 `function` 即可。

```c++
auto clac = [&](int x, int y) -> void {
};
```

- `auto` 递归写法

相较于 `function` 写法，需要额外引用一遍自身。

```c++
auto dfs = [&](auto self, int x) -> bool {
    return self(self, x + 1);
};
dfs(dfs, 1);
```

### 4.2 使用构造函数

可以将一些必要的声明和预处理放在构造函数，在编译时，无论放置在程序的哪个位置，都会先于主函数进行。下方是我将输入流控制声明的过程。

```c++
int __FAST_IO__ = []() { // 函数名称可以随意修改
    ios::sync_with_stdio(0), cin.tie(0);
    cout.tie(0);
    cout << fixed << setprecision(12);
    freopen("out.txt", "r", stdin);
    freopen("in.txt", "w", stdout);
    return 0;
}();
```
