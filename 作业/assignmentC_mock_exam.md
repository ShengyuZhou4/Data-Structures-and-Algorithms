# Assignment #C: 202505114 Mock Exam

Updated 1518 GMT+8 May 14, 2025

2025 spring, Complied by 周晟昱 工学院



> **说明：**
>
> 1. **⽉考**：AC5 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
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

### E06364: 牛的选举

http://cs101.openjudge.cn/practice/06364/

思路：

两次排序即可。

代码：

```c++
#include <algorithm>
#include <iostream>
#include <utility>
#include <vector>

int main()
{
    int n, k;
    std::cin >> n >> k;
    std::vector<std::pair<int, std::pair<int, int>>> v(n);
    for (int i = 0; i < n; ++i)
    {
        v[i].first = i + 1;
        std::cin >> v[i].second.first >> v[i].second.second;
    }
    auto cmp1 = [](const std::pair<int, std::pair<int, int>> &a, const std::pair<int, std::pair<int, int>> &b) -> bool
    { return a.second.first > b.second.first; };
    auto cmp2 = [](const std::pair<int, std::pair<int, int>> &a, const std::pair<int, std::pair<int, int>> &b) -> bool
    { return a.second.second > b.second.second; };
    std::sort(v.begin(), v.end(), cmp1);
    std::sort(v.begin(), v.begin() + k, cmp2);
    std::cout << v[0].first << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentC_mock_exam.assets/屏幕截图 2025-05-14 222256.png)



### M04077: 出栈序列统计

http://cs101.openjudge.cn/practice/04077/

思路：

记忆化搜索，显然每种出栈序列可以用一个有效的出栈-入栈操作序列唯一表示，以此为思路进行搜索即可。

代码：

```c++
#include <iostream>

int n, mem[15][15];
int calc(const int &current, const int &remain)
{
    if(mem[current][remain])
        return mem[current][remain];
    if(current == n)
        return mem[current][remain] = 1;
    return mem[current][remain] = calc(current + 1, remain + 1) + (remain ? calc(current, remain - 1) : 0);
}
int main()
{
    std::cin >> n;
    std::cout << calc(0, 0) << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentC_mock_exam.assets/屏幕截图 2025-05-14 222753.png)



### M05343:用队列对扑克牌排序

http://cs101.openjudge.cn/practice/05343/

思路：

按照题目要求操作即可。

代码：

```c++
#include <array>
#include <iostream>
#include <queue>
#include <utility>

int main()
{
    int n;
    std::cin >> n;
    std::pair<char, char> temp;
    std::queue<std::pair<char, char>> mem;
    std::array<std::queue<std::pair<char, char>>, 9> q;
    for (int i = 0; i < n; ++i)
    {
        std::cin >> temp.first >> temp.second;
        q[temp.second - '1'].push(temp);
    }
    for (int i = 0; i < 9; ++i)
    {
        std::cout << "Queue" << i + 1 << ':';
        if (q[i].empty())
            std::cout << '\n';
        while (!q[i].empty())
        {
            std::cout << q[i].front().first << q[i].front().second << (q[i].size() == 1 ? '\n' : ' ');
            mem.push(q[i].front());
            q[i].pop();
        }
    }
    for (int i = 0; i < n; ++i)
    {
        q[mem.front().first - 'A'].push(mem.front());
        mem.pop();
    }
    for (int i = 0; i < 4; ++i)
    {
        std::cout << "Queue" << static_cast<char>('A' + i) << ':';
        if (q[i].empty())
            std::cout << '\n';
        while (!q[i].empty())
        {
            std::cout << q[i].front().first << q[i].front().second << (q[i].size() == 1 ? '\n' : ' ');
            mem.push(q[i].front());
            q[i].pop();
        }
    }
    while (!mem.empty())
    {
        std::cout << mem.front().first << mem.front().second << (mem.size() == 1 ? '\n' : ' ');
        mem.pop();
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentC_mock_exam.assets/屏幕截图 2025-05-14 223027.png)



### M04084: 拓扑排序

http://cs101.openjudge.cn/practice/04084/

思路：

Kahn 算法，但将队列换成了优先队列以满足题目要求。

代码：

```c++
#include <iostream>
#include <vector>
#include <queue>

int main()
{
    int v, a;
    std::cin >> v >> a;
    std::vector<std::vector<int>> edge(v + 1);
    std::vector<int> in_degree(v + 1, 0);
    for (int u, v, i = 0; i < a; ++i)
    {
        std::cin >> u >> v;
        ++in_degree[v];
        edge[u].push_back(v);
    }
    std::priority_queue<int, std::vector<int>, std::greater<int>> q;
    for (int i = 1; i <= v; ++i)
        if (in_degree[i] == 0)
            q.push(i);
    while (!q.empty())
    {
        int u = q.top();
        q.pop();
        for (const auto &v : edge[u])
            if (--in_degree[v] == 0)
                q.push(v);
        std::cout << 'v' << u << (--v == 0 ? '\n' : ' ');
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentC_mock_exam.assets/屏幕截图 2025-05-14 221904.png)



### M07735:道路

Dijkstra, http://cs101.openjudge.cn/practice/07735/

思路：

大致是Dijkstra，但$dist$数组增加了费用维度。考试时没有想到只要维护可达的费用即可，最终超时。

代码：

```c++
#include <climits>
#include <iostream>
#include <queue>
#include <vector>
struct Edge
{
    int d, l, t;
};
struct State
{
    int position, distance, cost;
    bool operator<(const State &other) const
    {
        return distance > other.distance;
    }
};
int main()
{
    int k, n, r;
    std::cin >> k >> n >> r;
    std::vector<std::vector<int>> dist(k + 1, std::vector<int>(n + 1, INT_MAX));
    std::vector<std::vector<Edge>> edges(n + 1);
    for (int s, d, l, t, i = 0; i < r; ++i)
    {
        std::cin >> s >> d >> l >> t;
        edges[s].push_back({d, l, t});
    }
    dist[0][1] = 0;
    std::priority_queue<State> q;
    q.push({1, 0, 0});
    while (!q.empty())
    {
        State curr = q.top();
        q.pop();
        if (curr.position == n)
        {
            std::cout << curr.distance << std::endl;
            return 0;
        }
        for (const Edge &edge : edges[curr.position])
        {
            int nc = curr.cost + edge.t;
            if (nc <= k)
            {
                int nd = curr.distance + edge.l;
                if (nd < dist[nc][edge.d])
                {
                    dist[nc][edge.d] = nd;
                    q.push({edge.d, nd, nc});
                }
            }
        }
    }
    std::cout << -1 << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentC_mock_exam.assets/屏幕截图 2025-05-14 222003.png)



### T24637:宝藏二叉树

dp, http://cs101.openjudge.cn/practice/24637/

思路：

用记忆化搜索，自下而上计算每个子树可能获得的最大收益。对每个节点，考虑取自身和不取自身两种情况取最大值即可。

代码：

```c++
#include <algorithm>
#include <iostream>
#include <vector>

int n;
std::vector<int> tree, mem;
int calc(const int &pos)
{
    if (pos > n)
        return 0;
    if(mem[pos])
        return mem[pos];
    return mem[pos] = std::max(tree[pos] + calc(pos * 4) + calc(pos * 4 + 1) + calc(pos * 4 + 2) + calc(pos * 4 + 3), calc(pos * 2) + calc(pos * 2 + 1));
}
int main()
{
    std::cin >> n;
    tree.resize(n + 1);
    mem.resize(n + 1, 0);
    for (int i = 1; i <= n; ++i)
        std::cin >> tree[i];
    std::cout << calc(1) << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentC_mock_exam.assets/屏幕截图 2025-05-14 222205.png)



## 2. 学习总结和收获

第五题在考场上做了很久，最后超时了，现在看来把问题想得过于复杂了。查阅了Qwen3的官方文档，在System Prompt里面输入/no_think，成功关闭了思维链。











