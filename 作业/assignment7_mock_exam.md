# Assignment #7: 20250402 Mock Exam

Updated 1624 GMT+8 Apr 2, 2025

2025 spring, Complied by 周晟昱 工学院



> **说明：**
>
> 1. **⽉考**：AC6 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
>
> 2. **解题与记录：**
>
>    对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
> 3. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>
> 4. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
>
> 请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### E05344:最后的最后

http://cs101.openjudge.cn/practice/05344/



思路：

用链表模拟操作即可。

代码：

```c++
#include <iostream>
#include <list>
int main()
{
    int n, k, cnt = 1;
    std::cin >> n >> k;
    std::list<int> l;
    for (int i = 1; i <= n; ++i)
        l.push_back(i);
    auto it = l.begin();
    while (l.size() > 1)
    {
        if (cnt == k)
        {
            std::cout << *it << (l.size() == 2 ? '\n' : ' ');
            it = l.erase(it);
            cnt = 1;
        }
        else
        {
            ++it;
            ++cnt;
        }
        if (it == l.end())
            it = l.begin();
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment7_mock_exam.assets/屏幕截图 2025-04-05 091508.png)



### M02774: 木材加工

binary search, http://cs101.openjudge.cn/practice/02774/



思路：

二分查找小段木头的长度，检验其是否能满足题目要求。

代码：

```c++
#include <iostream>
#include <vector>
int main()
{
    int n, k, left = 1, right = 0;
    std::cin >> n >> k;
    std::vector<int> l(n);
    for (auto &i : l)
    {
        std::cin >> i;
        right += i;
    }
    right /= k;
    while (left < right)
    {
        int mid = (left + right + 1) / 2, cnt = 0;
        for (const auto &i : l)
            cnt += i / mid;
        if (cnt >= k)
            left = mid;
        else
            right = mid - 1;
    }
    std::cout << right << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment7_mock_exam.assets/屏幕截图 2025-04-05 091715.png)



### M07161:森林的带度数层次序列存储

tree, http://cs101.openjudge.cn/practice/07161/



思路：

先在列表中按照输入顺序存储元素，再从第二个元素开始依据出度依次分配子元素，最后直接遍历即可。

代码：

```python
class TreeNode:
    def __init__(self, name):
        self.name = name
        self.children = []


def traverse(root, ans):
    for i in root.children:
        traverse(i, ans)
    ans.append(root.name)

n = int(input())
ans = []
for _ in range(n):
    tree = []
    children_count = []
    idx = 0
    for i in input().split():
        if 'A' <= i <= 'Z':
            tree.append(TreeNode(i))
        else:
            children_count.append(int(i))
    for i in range(len(tree)):
        for _ in range(children_count[i]):
            idx += 1
            tree[i].children.append(tree[idx])
    traverse(tree[0], ans)
print(" ".join(ans))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment7_mock_exam.assets/屏幕截图 2025-04-05 091951.png)



### M18156:寻找离目标数最近的两数之和

two pointers, http://cs101.openjudge.cn/practice/18156/



思路：

双指针，将原序列排序后，初始状态下两指针分别指向首尾元素，若和大于目标则左移右指针，若和小于目标则右移左指针，直至两指针重合。记录过程中出现的最优解即可。

代码：

```python
t = int(input())
nums = list(map(int, input().split()))
nums.sort()
i, j = 0, len(nums) - 1
ans = nums[i] + nums[j]
while i < j:
    if abs(ans - t) > abs(nums[i]+nums[j]-t) or (abs(ans-t) == abs(nums[i]+nums[j]-t) and ans > nums[i]+nums[j]):
        ans = nums[i] + nums[j]
    if nums[i]+nums[j] < t:
        i += 1
    elif nums[i]+nums[j] > t:
        j -= 1
    else:
        break
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment7_mock_exam.assets/屏幕截图 2025-04-05 092333.png)



### M18159:个位为 1 的质数个数

sieve, http://cs101.openjudge.cn/practice/18159/



思路：

用筛法找出题目范围内所有个位为1的质数，随后对于每个查询直接输出即可。

代码：

```c++
#include <iostream>
#include <vector>
std::vector<int> primes;
std::vector<bool> is_prime;
void sieve(const int &n)
{
    is_prime.resize(n + 1, true);
    is_prime[0] = is_prime[1] = false;
    for (int i = 2; i <= n; ++i)
        if (is_prime[i])
        {
            if (i % 10 == 1)
                primes.push_back(i);
            for (int j = i * i; j <= n; j += i)
                is_prime[j] = false;
        }
}
int main()
{
    sieve(10001);
    int t, n;
    std::cin >> t;
    for (int i = 1; i <= t; ++i)
    {
        std::cin >> n;
        std::cout << "Case" << i << ":\n";
        if (n <= 11)
            std::cout << "NULL\n";
        for (int j = 0; j < primes.size(); ++j)
        {
            if (primes[j] >= n)
                break;
            std::cout << primes[j] << (j == primes.size() - 1 || primes[j + 1] >= n ? '\n' : ' ');
        }
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment7_mock_exam.assets/屏幕截图 2025-04-05 092509.png)



### M28127:北大夺冠

hash table, http://cs101.openjudge.cn/practice/28127/



思路：

用字典存储名称:列表对，列表中存储每个学校已经完成的题目和提交次数，边输入边统计，最后将其转化为列表排序输出。

代码：

```python
m = int(input())
rec = {}
for _ in range(m):
    name, problem, result = input().strip().split(',')
    if name not in rec:
        rec[name] = [set(), 0]
    if result[0] == 'y':
        rec[name][0].add(problem)
    rec[name][1] += 1
rec = list(map(lambda x:(-len(x[1][0]), x[1][1], x[0]), rec.items()))
rec.sort()
for i, t in enumerate(rec, 1):
    if i > 12:
        break
    print(i, t[2], -t[0], t[1])
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](./assignment7_mock_exam.assets/屏幕截图 2025-04-05 092736.png)



## 2. 学习总结和收获

这次考试总体难度不大，耗时较短，但在M07161处出现了卡壳，主要是一开始没有想出时间与空间开销较小的数据处理方法，可见目前做过题目的广度还不够。









