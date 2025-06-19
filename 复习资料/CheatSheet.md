## 调度场算法

算法原理

调度场算法基于栈的操作，模拟了铁路调度场的操作。它使用两个栈：一个用于保存操作符，另一个用于保存操作数和中间结果。算法通过依次读取每一个符号，根据符号的类型（操作数、操作符、括号等）分别进行处理。

处理步骤

1. **操作数**：直接推入输出队列。
2. **操作符**：根据优先级和结合性，将栈顶的操作符弹出并推入输出队列，然后将当前操作符压入操作符栈。
3. **左括号**：直接压入操作符栈。
4. **右括号**：弹出操作符栈中的操作符，直到遇到左括号为止。

算法实现

```cpp
#include <bits/stdc++.h>
using namespace std;
// 这里定义Token主要是为了使数字和运算符可以存到同一个容器里
// 其实op和num同时只用得上一个值，但用Union的话，判断类型反而麻烦。C++17的话可以考虑std::variant
struct Token
{
    char op;
    int num;
};
map<char, int> pre{{'+', 1}, {'-', 1}, {'*', 2}, {'/', 2}, {'^', 3}};
// 优先级
int main()
{
    string expr;
    vector<Token> infix, suffix;
    stack<Token> oper;
    cin >> expr;
    for (auto c : expr)
        // 将字符串解析成若干**Token**存入**vector
        if (isdigit(c))
            infix.push_back(Token{0, c - '0'});
        else
            infix.push_back(Token{c, 0});
    for (auto t : infix)
        if (!t.op)
            suffix.push_back(t);
        else if (t.op == '(')
            oper.push(t);
        else if (t.op == ')')
        {
            while (!oper.empty() && oper.top().op != '(')
            {
                suffix.push_back(oper.top());
                oper.pop();
            }
            oper.pop();
        }
        else
        {
            while (!oper.empty() && pre[oper.top().op] >= pre[t.op])
            {
                suffix.push_back(oper.top());
                oper.pop();
            }
            oper.push(t);
        }
    while (!oper.empty())
    {
        suffix.push_back(oper.top());
        oper.pop();
    }
    // 这里对后缀表达式的计算，因为要输出中间结果，就直接开二重循环了，没有使用栈
    while (suffix.size() > 1)
    {
        for (auto t : suffix)
            if (t.op)
                cout << t.op << " ";
            else
                cout << t.num << " ";
        cout << endl;
        for (int i = 2; i < suffix.size(); ++i)
        {
            int res;
            Token a = suffix[i - 2], b = suffix[i - 1];
            switch (suffix[i].op)
            {
                case 0:
                    continue;
                case '+':
                    res = a.num + b.num;
                    break;
                case '-':
                    res = a.num - b.num;
                    break;
                case '*':
                    res = a.num * b.num;
                    break;
                case '/':
                    res = a.num / b.num;
                    break;
                case '^':
                    res = pow(a.num, b.num);
                    break;
            }
            suffix.insert(suffix.begin() + i + 1, Token{0, res});
            suffix.erase(suffix.begin() + i - 2, suffix.begin() + i + 1);
            break;
        }
    }
    cout << suffix[0].num;
    // 输出最后结果
    return 0;
}
```



## KMP

计算前缀函数

```cpp
vector<int> prefix_function(string s)
{
    int n = (int)s.length();
    vector<int> pi(n);
    for (int i = 1; i < n; i++)
    {
        int j = pi[i - 1];
        while (j > 0 && s[i] != s[j])
            j = pi[j - 1];
        if (s[i] == s[j])
            j++;
        pi[i] = j;
    }
    return pi;
}
```



## 强连通分量 Tarjan算法

```cpp
int dfn[N], low[N], dfncnt, s[N], in_stack[N], tp;
int scc[N], sc;
// 结点i所在SCC的编号
int sz[N];
// 强连通i的大小
void tarjan(int u)
{
    low[u] = dfn[u] = ++dfncnt, s[++tp] = u, in_stack[u] = 1;
    for (int i = h[u]; i; i = e[i].nex)
    {
        const int &v = e[i].t;
        if (!dfn[v])
        {
            tarjan(v);
            low[u] = min(low[u], low[v]);
        }
        else if (in_stack[v])
        {
            low[u] = min(low[u], dfn[v]);
        }
    }
    if (dfn[u] == low[u])
    {
        ++sc;
        do
        {
            scc[s[tp]] = sc;
            sz[sc]++;
            in_stack[s[tp]] = 0;
        } while (s[tp--] != u);
    }
}
```



## Bellman-Ford

SPFA

```cpp
struct edge
{
    int v, w;
};

vector<edge> e[MAXN];
int dis[MAXN], cnt[MAXN], vis[MAXN];
queue<int> q;

bool spfa(int n, int s)
{
    memset(dis, 0x3f, (n + 1) * sizeof(int));
    dis[s] = 0, vis[s] = 1;
    q.push(s);
    while (!q.empty())
    {
        int u = q.front();
        q.pop(), vis[u] = 0;
        for (auto ed : e[u])
        {
            int v = ed.v, w = ed.w;
            if (dis[v] > dis[u] + w)
            {
                dis[v] = dis[u] + w;
                cnt[v] = cnt[u] + 1;
                * //* *记录最短路经过的边数*
                    if (cnt[v] >= n) return false;
                // 在不经过负环的情况下，最短路至多经过n - 1条边
                // 因此如果经过了多于n条边，一定说明经过了负环
                if (!vis[v])
                    q.push(v), vis[v] = 1;
            }
        }
    }
    return true;
}
```





 