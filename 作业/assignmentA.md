# Assignment #A: Graph starts

Updated 1830 GMT+8 Apr 22, 2025

2025 spring, Complied by 周晟昱 工学院



> **说明：**
>
> 1. **解题与记录：**
>
>    对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>
> 2. **提交安排：**提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
>
> 3. **延迟提交：**如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
>
> 请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### M19943:图的拉普拉斯矩阵

OOP, implementation, http://cs101.openjudge.cn/practice/19943/

要求创建Graph, Vertex两个类，建图实现。

思路：

我自己原来的代码是直接处理的，这里的OOP写法参照了题解的思路，只是改为用C++实现，并去除了一些本题用不到的成员函数。

代码：

```c++
#include <iostream>
#include <unordered_map>
#include <unordered_set>
#include <vector>
class Vertex
{
public:
    int id;
    std::unordered_set<Vertex *> connectedTo;
    Vertex(int key) : id(key)
    {
    }
    void addNeighbor(Vertex *nbr)
    {
        connectedTo.insert(nbr);
    }
    const std::unordered_set<Vertex *> &getConnections() const
    {
        return connectedTo;
    }
    int getId() const
    {
        return id;
    }
};
class Graph
{
public:
    std::unordered_map<int, Vertex *> vertList;
    int numVertices;
    Graph() : numVertices(0)
    {
    }
    ~Graph()
    {
        for (auto &pair : vertList)
            delete pair.second;
    }
    Vertex *addVertex(int key)
    {
        if (vertList.find(key) == vertList.end())
        {
            Vertex *newVertex = new Vertex(key);
            vertList[key] = newVertex;
            numVertices++;
        }
        return vertList[key];
    }
    Vertex *getVertex(int n)
    {
        auto it = vertList.find(n);
        if (it != vertList.end())
            return it->second;
        return nullptr;
    }
    bool contains(int n)
    {
        return vertList.find(n) != vertList.end();
    }
    void addEdge(int f, int t)
    {
        Vertex *fv = addVertex(f);
        Vertex *tv = addVertex(t);
        fv->addNeighbor(tv);
    }
};
std::vector<std::vector<int>> constructLaplacianMatrix(int n, std::vector<std::pair<int, int>> &edges)
{
    Graph graph;
    for (int i = 0; i < n; ++i)
        graph.addVertex(i);
    for (auto &edge : edges)
    {
        int a = edge.first;
        int b = edge.second;
        graph.addEdge(a, b);
        graph.addEdge(b, a);
    }
    std::vector<std::vector<int>> laplacianMatrix(n, std::vector<int>(n, 0));
    for (int i = 0; i < n; ++i)
    {
        Vertex *vertex = graph.getVertex(i);
        int degree = vertex->getConnections().size();
        laplacianMatrix[i][i] = degree;
        for (Vertex *neighbor : vertex->getConnections())
        {
            int j = neighbor->getId();
            laplacianMatrix[i][j] = -1;
        }
    }
    return laplacianMatrix;
}
int main()
{
    int n, m;
    std::cin >> n >> m;
    std::vector<std::pair<int, int>> edges;
    for (int i = 0; i < m; ++i)
    {
        int a, b;
        std::cin >> a >> b;
        edges.emplace_back(a, b);
    }
    std::vector<std::vector<int>> matrix = constructLaplacianMatrix(n, edges);
    for (auto &row : matrix)
        for (size_t j = 0; j < row.size(); ++j)
            std::cout << row[j] << (j == row.size() - 1 ? '\n' : ' ');
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark> 

![](./assignmentA.assets/屏幕截图 2025-04-23 112927.png)



### LC78.子集

backtracking, https://leetcode.cn/problems/subsets/

思路：

利用位运算枚举每个元素是否在子集中，存储结果即可。

代码：

```c++
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;
        for (int i = 0; i < (1 << nums.size()); ++i) {
            ans.emplace_back();
            for (int j = 0; j < nums.size(); ++j)
                if ((1 << j) & i)
                    ans.back().push_back(nums[j]);
        }
        return ans;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentA.assets/屏幕截图 2025-04-23 113341.png)



### LC17.电话号码的字母组合

hash table, backtracking, https://leetcode.cn/problems/letter-combinations-of-a-phone-number/

思路：

递归处理，拼接字符串并存储。

代码：

```c++
class Solution {
private:
    const vector<vector<string>> dict{{"a", "b", "c"}, {"d", "e", "f"},
                                      {"g", "h", "i"}, {"j", "k", "l"},
                                      {"m", "n", "o"}, {"p", "q", "r", "s"},
                                      {"t", "u", "v"}, {"w", "x", "y", "z"}};

public:
    vector<string> letterCombinations(string digits) {
        if (digits.empty())
            return vector<string>();
        if (digits.size() == 1)
            return dict[digits[0] - '2'];
        auto&& arr = letterCombinations(digits.substr(1));
        vector<string> ans;
        for (const auto& i : dict[digits[0] - '2'])
            for (const auto& j : arr)
                ans.push_back(i + j);
        return ans;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentA.assets/屏幕截图 2025-04-23 124236.png)



### M04089:电话号码

trie, http://cs101.openjudge.cn/practice/04089/

思路：

用Trie存储并在添加过程中判断即可。

代码：

```python
class TrieNode:
    def __init__(self):
        self.children = [None] * 10
        self.is_end = False

def is_consistent(phones):
    phones.sort(key=lambda x: -len(x))
    root = TrieNode()
    for num in phones:
        current = root
        is_prefix = True
        for digit in num:
            idx = int(digit)
            if not current.children[idx]:
                current.children[idx] = TrieNode()
                is_prefix = False
            current = current.children[idx]
            if current.is_end:
                return False
        if is_prefix:
            return False
        current.is_end = True
    return True

t = int(input())
for _ in range(t):
    n = int(input())
    phones = [input().strip() for _ in range(n)]
    print("YES" if is_consistent(phones) else "NO")
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentA.assets/屏幕截图 2025-04-23 144108.png)



### T28046:词梯

bfs, http://cs101.openjudge.cn/practice/28046/

思路：

建图后计算最短路即可。

代码：

```c++
#include <climits>
#include <functional>
#include <iostream>
#include <queue>
#include <string>
#include <unordered_map>
#include <utility>
#include <vector>
int main()
{
    int n;
    std::cin >> n;
    std::vector<std::string> dictionary(n);
    std::unordered_map<std::string, int> number;
    for (int i = 0; i < n; ++i)
    {
        std::cin >> dictionary[i];
        number[dictionary[i]] = i;
    }
    std::vector<std::vector<int>> edge(n);
    for (int i = 0; i < n - 1; ++i)
        for (int j = i + 1; j < n; ++j)
            if ((dictionary[i][0] != dictionary[j][0]) + (dictionary[i][1] != dictionary[j][1]) + (dictionary[i][2] != dictionary[j][2]) + (dictionary[i][3] != dictionary[j][3]) == 1)
            {
                edge[i].push_back(j);
                edge[j].push_back(i);
            }
    std::string start, end;
    std::cin >> start >> end;
    std::vector<std::pair<int, int>> dist(n, {INT_MAX, -1});
    dist[number[start]] = {0, -1};
    std::priority_queue<std::pair<int, int>, std::vector<std::pair<int, int>>, std::greater<std::pair<int, int>>> q;
    q.push({0, number[start]});
    while (!q.empty())
    {
        auto [d, u] = q.top();
        q.pop();
        if (d > dist[u].first)
            continue;
        for (int v : edge[u])
            if (dist[v].first > d + 1)
            {
                dist[v] = {d + 1, u};
                q.push({d + 1, v});
            }
    }
    if (dist[number[end]].first == INT_MAX)
        std::cout << "NO\n";
    else
    {
        std::vector<int> path;
        for (int u = number[end]; u != -1; u = dist[u].second)
            path.push_back(u);
        for (int i = path.size() - 1; i >= 0; --i)
            std::cout << dictionary[path[i]] << (i ? ' ' : '\n');
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentA.assets/屏幕截图 2025-04-23 144325.png)



### T51.N皇后

backtracking, https://leetcode.cn/problems/n-queens/

思路：

DFS搜索即可。使用了位运算节省空间。

代码：

```c++
class Solution {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> res;
        vector<int> q(n, -1);
        dfs(res, q, n, 0, 0, 0, 0);
        return res;
    }

private:
    void dfs(auto& res, auto& q, int n, int row, int cols, int ld, int rd) {
        if (row == n) {
            vector<string> board;
            for (const auto &c : q)
                board.push_back(string(c, '.') + 'Q' + string(n - c - 1, '.'));
            res.push_back(board);
            return;
        }
        for (int avail = ~(cols | ld | rd) & ((1 << n) - 1); avail;
             avail &= avail - 1) {
            int pos = avail & -avail;
            q[row] = __builtin_ctz(pos);
            dfs(res, q, n, row + 1, cols | pos, (ld | pos) >> 1,
                (rd | pos) << 1);
            q[row] = -1;
        }
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentA.assets/屏幕截图 2025-04-23 152600.png)



## 2. 学习总结和收获

最近一部分每日选做题的难度较高，提交后经常出现难以发现的问题，需要大量时间寻找corner case，本次作业中也有这样的题目。







