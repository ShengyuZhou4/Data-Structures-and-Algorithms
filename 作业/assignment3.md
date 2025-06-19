# Assignment #3: 惊蛰 Mock Exam

Updated 1641 GMT+8 Mar 5, 2025

2025 spring, Complied by 周晟昱 工学院



> **说明：**
>
> 1. **惊蛰⽉考**：AC5 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。
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

### E04015: 邮箱验证

strings, http://cs101.openjudge.cn/practice/04015



思路：

将邮箱按照‘@’分割，先检查是否只被划分成了两段，再检查每一段的首尾是否有‘.’即可。

也可以使用正则表达式判断。

代码：

使用正则表达式：

```c++
#include <iostream>
#include <regex>

int main()
{
    std::string s;
    const std::regex rex(R"(^(([^@.][^@]*[^@.])|([^@.]))@[^@.][^@]*\.[^@]*[^@.]$)");
    while (std::cin >> s)
        std::cout << (std::regex_match(s, rex) ? "YES" : "NO") << std::endl;
    return 0;
}
```

不使用正则表达式：

```python
while True:
    try:
        s = tuple(input().split('@'))
        print('YES' if len(s) == 2 and s[0][0] != '.' and s[0][-1] != '.' and s[1][0] != '.' and s[1][-1] != '.' and '.' in s[1] else 'NO')
    except EOFError:
        break
```





代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment3.assets/屏幕截图 2025-03-07 145509.png)

![](./assignment3.assets/屏幕截图 2025-03-07 145456.png)



### M02039: 反反复复

implementation, http://cs101.openjudge.cn/practice/02039/



思路：

按照题目描述还原即可。

代码：

```python
n = int(input())
s = input()
mat = [[] for _ in range(n)]
for i in range(len(s)):
    if (i // n) % 2 == 0:
        mat[i % n].append(s[i])
    else:
        mat[- (i % n + 1)].append(s[i])
for col in mat:
    print(''.join(col), end='')
print()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment3.assets/屏幕截图 2025-03-07 145632.png)



### M02092: Grandpa is Famous

implementation, http://cs101.openjudge.cn/practice/02092/



思路：

用字典统计之后转为列表并排序，再输出所有排在第二的选手即可。

代码：

```python
from collections import defaultdict


n, m = map(int, input().split())
while n != 0 or m != 0:
    cnt = defaultdict(int)
    for _ in range(n):
        for player in map(int, input().split()):
            cnt[player] += 1
    cnt = list(cnt.items())
    cnt.sort(key=lambda x: (-x[1], x[0]))
    score = cnt[1][1]
    print(cnt[1][0], end='')
    for i in range(2, len(cnt)):
        if cnt[i][1] != score:
            break
        print('', cnt[i][0], end='')
    print()
    n, m = map(int, input().split())

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment3.assets/屏幕截图 2025-03-07 145832.png)



### M04133: 垃圾炸弹

matrices, http://cs101.openjudge.cn/practice/04133/



思路：

用一个二维数组储存每个位置可以清除的垃圾数量，在每个屏幕周围的可被炸弹波及的所有位置加上该屏幕处的垃圾数量，最后统计即可。

代码：

```python
d = int(input())
n = int(input())
cnt = [[0] * 1025 for _ in range(1025)]
for _ in range(n):
    x, y, i = map(int, input().split())
    lx, rx = max(0, x - d), min(1024, x + d)
    ly, ry = max(0, y - d), min(1024, y + d)
    for k in range(lx, rx + 1):
        for l in range(ly, ry + 1):
            cnt[k][l] += i
max_garbage, ans = 0, 0
for k in range(1025):
    for l in range(1025):
        if cnt[k][l] == max_garbage:
            ans += 1
        elif cnt[k][l] > max_garbage:
            ans = 1
            max_garbage = cnt[k][l]
print(ans, max_garbage)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment3.assets/屏幕截图 2025-03-07 150105.png)



### T02488: A Knight's Journey

backtracking, http://cs101.openjudge.cn/practice/02488/



思路：

用深度优先搜索寻找可行路线。注意题目要求输出字典序最小的结果，这对起点的选择和搜索过程之中方案选取的顺序有要求。

代码：

```c++
#include <array>
#include <iostream>
#include <string>
#include <vector>
bool dfs(const int &x, const int &y, std::vector<std::vector<bool>> &vis, int &cnt, std::string &ans)
{
    static std::array<int, 8> dx{-1, 1, -2, 2, -2, 2, -1, 1}, dy{-2, -2, -1, -1, 1, 1, 2, 2};
    vis[x][y] = true;
    ++cnt;
    ans.push_back('A' + y);
    ans += std::to_string(1 + x);
    if (cnt == vis.size() * vis[0].size())
        return true;
    for (int nx, ny, i = 0; i < 8; ++i)
    {
        nx = x + dx[i];
        ny = y + dy[i];
        if (nx < 0 || nx >= vis.size() || ny < 0 || ny >= vis[0].size() || vis[nx][ny])
            continue;
        if (dfs(nx, ny, vis, cnt, ans))
            return true;
    }
    vis[x][y] = false;
    --cnt;
    while(std::isdigit(ans.back()))
        ans.pop_back();
    ans.pop_back();
    return false;
}
int main()
{
    int n, p, q;
    std::cin >> n;
    for (int i = 1; i <= n; ++i)
    {
        std::cin >> p >> q;
        std::vector<std::vector<bool>> vis(p, std::vector<bool>(q, false));
        int cnt = 0;
        std::string ans;
        std::cout << "Scenario #" << i << ":" << std::endl;
        for (int y = 0; y < q; ++y)
            for (int x = 0; x < p; ++x)
                if (dfs(x, y, vis, cnt, ans))
                {
                    std::cout << ans;
                    goto next;
                }
        std::cout << "impossible";
    next:
        std::cout << std::endl
                  << std::endl;
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment3.assets/屏幕截图 2025-03-07 150309.png)



### T06648: Sequence

heap, http://cs101.openjudge.cn/practice/06648/



思路：

可以递推处理此题。如果已经知道了前$$m-1$$个序列的最小的$$n$$个可能的和，则答案一定在这$$n$$个数与最后一个序列可以形成的$$n^2$$个数之中，可以用堆处理这一问题。以此类推即可。

代码：

```c++
#include <algorithm>
#include <iostream>
#include <memory>
#include <queue>
#include <utility>
#include <vector>
std::unique_ptr<std::vector<int>> solve(std::unique_ptr<std::vector<int>> a, std::unique_ptr<std::vector<int>> b)
{
    static std::priority_queue<int> q;
    for (const auto &i : *a)
    {
        for (const auto &j : *b)
            if (q.size() < a->size())
                q.push(i + j);
            else if (i + j < q.top())
            {
                q.pop();
                q.push(i + j);
            }
            else
                break;
        if (i + (*b)[0] >= q.top())
            break;
    }
    std::unique_ptr<std::vector<int>> c = std::make_unique<std::vector<int>>(a->size());
    for (auto it = c->rbegin(); it != c->rend(); ++it)
    {
        *it = q.top();
        q.pop();
    }
    return c;
}
int main()
{
    int T;
    std::cin >> T;
    while (T--)
    {
        int m, n;
        std::cin >> m >> n;
        std::unique_ptr<std::vector<int>> a = std::make_unique<std::vector<int>>(n), b;
        for (auto &i : *a)
            std::cin >> i;
        std::sort(a->begin(), a->end());
        for (int j = 1; j < m; ++j)
        {
            b = std::make_unique<std::vector<int>>(n);
            for (auto &i : *b)
                std::cin >> i;
            std::sort(b->begin(), b->end());
            a = solve(std::move(a), std::move(b));
        }
        for (int i = 0; i < n; ++i)
            std::cout << (*a)[i] << (i == n - 1 ? '\n' : ' ');
    }
    return 0;
}
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](./assignment3.assets/屏幕截图 2025-03-07 150654.png)



## 2. 学习总结和收获

最后一道题在考场上没有想出解法，其实最后的答案要求输出$$n$$个数已经有了一定提示作用。倒数第二题一开始没有注意到“the lexicographically first path”（其实自己对字典序的英文也不是很熟悉）导致调试了很久，读题还是要更加仔细。





