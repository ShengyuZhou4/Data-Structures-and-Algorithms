# Assignment #B: 图为主

Updated 2223 GMT+8 Apr 29, 2025

2025 spring, Complied by <mark>同学的姓名、院系</mark>



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

### E07218:献给阿尔吉侬的花束

bfs, http://cs101.openjudge.cn/practice/07218/

思路：

标准的BFS。

代码：

```c++
#include <iostream>
#include <map>
#include <queue>
#include <utility>
#include <vector>
int main()
{
    int t;
    std::cin >> t;
    while (t--)
    {
        int r, c, x1, y1, x2, y2;
        bool flag = true;
        std::cin >> r >> c;
        std::vector<std::vector<bool>> wall(r + 2, std::vector<bool>(c + 2, true));
        char temp;
        for (int i = 1; i <= r; ++i)
            for (int j = 1; j <= c; ++j)
            {
                std::cin >> temp;
                if (temp == '#')
                    continue;
                wall[i][j] = false;
                if (temp == 'S')
                {
                    x1 = i;
                    y1 = j;
                }
                else if (temp == 'E')
                {
                    x2 = i;
                    y2 = j;
                }
            }
        std::queue<std::pair<int, int>> q;
        std::map<std::pair<int, int>, int> rec;
        q.push({x1, y1});
        rec[{x1, y1}] = 0;
        while (!q.empty())
        {
            auto [x, y] = q.front();
            q.pop();
            int dist = rec[{x, y}];
            if (x == x2 && y == y2)
            {
                flag = false;
                std::cout << dist << std::endl;
                break;
            }
            if (!wall[x - 1][y] && rec.find({x - 1, y}) == rec.end())
            {
                q.push({x - 1, y});
                rec[{x - 1, y}] = dist + 1;
            }
            if (!wall[x + 1][y] && rec.find({x + 1, y}) == rec.end())
            {
                q.push({x + 1, y});
                rec[{x + 1, y}] = dist + 1;
            }
            if (!wall[x][y - 1] && rec.find({x, y - 1}) == rec.end())
            {
                q.push({x, y - 1});
                rec[{x, y - 1}] = dist + 1;
            }
            if (!wall[x][y + 1] && rec.find({x, y + 1}) == rec.end())
            {
                q.push({x, y + 1});
                rec[{x, y + 1}] = dist + 1;
            }
        }
        if (flag)
            std::cout << "oop!" << std::endl;
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentB.assets/屏幕截图 2025-05-01 162450.png)



### M3532.针对图的路径存在性查询I

disjoint set, https://leetcode.cn/problems/path-existence-queries-in-a-graph-i/

思路：

显然相连的一组点一定相邻，直接依据距离进行分组标记即可。

代码：

```c++
class Solution {
public:
    vector<bool> pathExistenceQueries(int n, vector<int>& nums, int maxDiff,
                                      vector<vector<int>>& queries) {
        vector<int> group(n);
        vector<bool> answer(queries.size());
        group[0] = 0;
        for (int i = 1; i < n; ++i)
            group[i] = nums[i] - nums[i - 1] <= maxDiff ? group[i - 1]
                                                        : group[i - 1] + 1;
        for (int i = 0; i < queries.size(); ++i)
            answer[i] = group[queries[i][0]] == group[queries[i][1]];
        return answer;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentB.assets/屏幕截图 2025-05-01 224257.png)



### M22528:厚道的调分方法

binary search, http://cs101.openjudge.cn/practice/22528/

思路：

二分答案，对每个答案判断优秀率是否达到了60%即可。

代码：

```python
from typing import List


def main():
    scores: List[float] = list(map(float, input().split()))
    l: int = 1
    r: int = 1000000000
    while l < r:
        mid: int = (l + r) // 2
        cnt: int = 0
        a: float = mid / 1000000000
        for score in scores:
            if a * score + 1.1 ** (a * score) >= 85:
                cnt += 1
        if 5 * cnt >= 3 * len(scores):
            r = mid
        else:
            l = mid + 1
    print(l)


if __name__ == "__main__":
    main()

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentB.assets/屏幕截图 2025-05-01 230125.png)



### Msy382: 有向图判环 

dfs, https://sunnywhy.com/sfbj/10/3/382

思路：

找到所有入度为零的点并放入队列，将其中点的所有出边删除，在此过程中将新的入度为零的点加入队列。若有节点不能通过这种方式被遍历到则图中有环，否则没有。

代码：

```python
from collections import deque
from typing import Deque, List, Set


def main():
    n, m = map(int, input().split())
    in_degree: List[int] = [0] * n
    edge: List[List[int]] = [[] for _ in range(n)]
    heads: Deque[int] = deque()
    for _ in range(m):
        u, v = map(int, input().split())
        in_degree[v] += 1
        edge[u].append(v)
    for i in range(n):
        if in_degree[i] == 0:
            heads.append(i)
    cnt: int = 0
    while heads:
        u = heads.popleft()
        cnt += 1
        for v in edge[u]:
            in_degree[v] -= 1
            if in_degree[v] == 0:
                heads.append(v)
    print("Yes" if cnt != n else "No")


if __name__ == "__main__":
    main()

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentB.assets/屏幕截图 2025-05-01 231856.png)



### M05443:兔子与樱花

Dijkstra, http://cs101.openjudge.cn/practice/05443/

思路：

标准的最短路。使用堆优化的Dijkstra算法。

代码：

```python
from heapq import heappop, heappush


def main():
    p = int(input())
    id = {}
    name = ["" for _ in range(p)]
    for i in range(p):
        name[i] = input()
        id[name[i]] = i
    edge = [[] for _ in range(p)]
    q = int(input())
    for _ in range(q):
        a, b, w = input().split()
        a, b, w = id[a], id[b], int(w)
        edge[a].append((b, w))
        edge[b].append((a, w))
    r = int(input())
    for _ in range(r):
        st, ed = input().split()
        st, ed = id[st], id[ed]
        q = []
        heappush(q, (0, st))
        dist = [(-1, (-1, 0))] * p
        dist[st] = (0, (-1, 0))
        while q:
            d, u = heappop(q)
            if u == ed:
                break
            if d > dist[u][0]:
                continue
            for v, w in edge[u]:
                if dist[v][0] == -1 or dist[v][0] > d + w:
                    dist[v] = (d + w, (u, w))
                    heappush(q, (d + w, v))
        path = []
        u = ed
        while dist[u][1][0] != -1:
            path.append(dist[u][1])
            u = dist[u][1][0]
        path.reverse()
        for u, w in path:
            print(f"{name[u]}->({w})->", end="")
        print(name[ed])


if __name__ == "__main__":
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentB.assets/屏幕截图 2025-05-01 232229.png)



### T28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/

思路：

直接遍历判断。

代码：

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

const int dx[] = {2, 1, -1, -2, -2, -1, 1, 2};
const int dy[] = {1, 2, 2, 1, -1, -2, -2, -1};

int n;
vector<vector<bool>> visited;

bool backtrack(int x, int y, int step) {
    if (step == n * n) {
        return true;
    }
    vector<pair<int, int>> next_moves;
    for (int i = 0; i < 8; ++i) {
        int nx = x + dx[i];
        int ny = y + dy[i];
        if (nx >= 0 && nx < n && ny >= 0 && ny < n && !visited[nx][ny]) {
            next_moves.emplace_back(nx, ny);
        }
    }
    vector<pair<int, pair<int, int>>> moves_with_count;
    for (auto &pos : next_moves) {
        int nx = pos.first, ny = pos.second;
        visited[nx][ny] = true;
        int count = 0;
        for (int dir = 0; dir < 8; ++dir) {
            int nnx = nx + dx[dir], nny = ny + dy[dir];
            if (nnx >= 0 && nnx < n && nny >= 0 && nny < n && !visited[nnx][nny]) {
                count++;
            }
        }
        visited[nx][ny] = false;
        moves_with_count.emplace_back(count, make_pair(nx, ny));
    }
    sort(moves_with_count.begin(), moves_with_count.end());
    for (auto &item : moves_with_count) {
        int nx = item.second.first, ny = item.second.second;
        visited[nx][ny] = true;
        if (backtrack(nx, ny, step + 1)) {
            return true;
        }
        visited[nx][ny] = false;
    }
    return false;
}

int main() {
    cin >> n;
    int sr, sc;
    cin >> sr >> sc;
    if (n % 2 != 0) {
        if ((sr + sc) % 2 != 0) {
            cout << "fail" << endl;
            return 0;
        }
    }
    visited.resize(n, vector<bool>(n, false));
    visited[sr][sc] = true;
    if (backtrack(sr, sc, 1)) {
        cout << "success" << endl;
    } else {
        cout << "fail" << endl;
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignmentB.assets/屏幕截图 2025-05-01 232334.png)



## 2. 学习总结和收获

期中考试终于结束了，可以有更多时间用于每日选做。试着在本地部署了Qwen3-30B-A3B，发现MoE模型在本地运行的速度确实比相似规模的Dense模型快很多，但实在太耗内存了，运行时基本不能再使用其他程序，否则就会卡死；此外部分Qwen3系列的模型很容易过度思考不输出结果，官方文档中提到Qwen3系列可以手动关闭思维链，但在LM Studio上没有找到相应开关。











