# Assignment #9: Huffman, BST & Heap

Updated 1834 GMT+8 Apr 15, 2025

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

### LC222.完全二叉树的节点个数

dfs, https://leetcode.cn/problems/count-complete-tree-nodes/

思路：

参考题解思路，对最后一层进行二分查找。

代码：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left),
 * right(right) {}
 * };
 */
class Solution {
public:
    int countNodes(TreeNode* root) {
        int depth = 0;
        TreeNode* ptr = root;
        while (ptr != nullptr) {
            ++depth;
            ptr = ptr->left;
        }
        if (depth == 0)
            return 0;
        if (depth == 1)
            return 1;
        int l = 1 << (depth - 1), r = (1 << depth) - 1;
        while (l < r) {
            int mid = (l + r + 1) >> 1, t = 1 << (depth - 2);
            ptr = root;
            while (t) {
                if (t & mid)
                    ptr = ptr->right;
                else
                    ptr = ptr->left;
                if (ptr == nullptr)
                    break;
                t >>= 1;
            }
            if (ptr == nullptr)
                r = mid - 1;
            else
                l = mid;
        }
        return l;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment9.assets/屏幕截图 2025-04-15 223238.png)



### LC103.二叉树的锯齿形层序遍历

bfs, https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/

思路：

在层序遍历的基础上修改即可。

代码：

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left),
 * right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        bool reversed = false;
        vector<vector<int>> ans;
        if (root == nullptr)
            return ans;
        unique_ptr<vector<TreeNode*>> ptr1(make_unique<vector<TreeNode*>>()),
            ptr2;
        ptr1->push_back(root);
        while (!ptr1->empty()) {
            ans.emplace_back();
            ptr2 = make_unique<vector<TreeNode*>>();
            for (auto i : *ptr1) {
                if (i->left != nullptr)
                    ptr2->push_back(i->left);
                if (i->right != nullptr)
                    ptr2->push_back(i->right);
                ans.back().push_back(i->val);
            }
            if (reversed)
                reverse(ans.back().begin(), ans.back().end());
            reversed = !reversed;
            ptr1 = move(ptr2);
        }
        return ans;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment9.assets/屏幕截图 2025-04-15 223521.png)



### M04080:Huffman编码树

greedy, http://cs101.openjudge.cn/practice/04080/

思路：

按照定义使用优先队列处理即可。

代码：

```c++
#include <climits>
#include <iostream>
#include <memory>
#include <queue>
#include <utility>
struct TreeNode
{
    int weight;
    std::unique_ptr<TreeNode> left, right;
    TreeNode(const int &w, std::unique_ptr<TreeNode> &&l, std::unique_ptr<TreeNode> &&r) :
        weight(w), left(std::move(l)), right(std::move(r))
    {
    }
};
bool operator<(const std::unique_ptr<TreeNode> &a, const std::unique_ptr<TreeNode> &b)
{
    return a->weight > b->weight;
}
int ans = 0;
void solve(std::unique_ptr<TreeNode> &&root, const int &depth = 0)
{
    if (root->left == nullptr && root->right == nullptr)
    {
        ans += depth * root->weight;
        return;
    }
    solve(std::move(root->left), depth + 1);
    solve(std::move(root->right), depth + 1);
}
int main()
{
    int n;
    std::cin >> n;
    std::priority_queue<std::unique_ptr<TreeNode>> q;
    while (n--)
    {
        int t;
        std::cin >> t;
        q.emplace(std::make_unique<TreeNode>(t, nullptr, nullptr));
    }
    while (q.size() > 1)
    {
        auto a = std::move(const_cast<std::unique_ptr<TreeNode> &>(q.top()));
        q.pop();
        auto b = std::move(const_cast<std::unique_ptr<TreeNode> &>(q.top()));
        q.pop();
        q.emplace(std::make_unique<TreeNode>(a->weight + b->weight, std::move(a), std::move(b)));
    }
    solve(std::move(const_cast<std::unique_ptr<TreeNode> &>(q.top())));
    std::cout << ans << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment9.assets/屏幕截图 2025-04-15 223710.png)



### M05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/

思路：

按照BST的定义建树并遍历即可。

代码：

```c++
#include <iostream>
#include <memory>
#include <queue>
#include <utility>
struct TreeNode
{
    int val;
    std::unique_ptr<TreeNode> left, right;
    TreeNode() :
        val(0), left(nullptr), right(nullptr)
    {
    }
    TreeNode(const int &value) :
        val(value), left(nullptr), right(nullptr)
    {
    }
    TreeNode(const int &value, std::unique_ptr<TreeNode> &&l, std::unique_ptr<TreeNode> &&r) :
        val(value), left(std::move(l)), right(std::move(r))
    {
    }
};
std::unique_ptr<TreeNode> add_to_tree(std::unique_ptr<TreeNode> root, const int &value)
{
    if (value == root->val)
        return root;
    if (value < root->val)
    {
        if (root->left == nullptr)
            root->left = std::make_unique<TreeNode>(value);
        else
            root->left = add_to_tree(std::move(root->left), value);
    }
    else
    {
        if (root->right == nullptr)
            root->right = std::make_unique<TreeNode>(value);
        else
            root->right = add_to_tree(std::move(root->right), value);
    }
    return root;
}
int main()
{
    int temp;
    std::cin >> temp;
    std::cout << temp;
    std::unique_ptr<TreeNode> root(std::make_unique<TreeNode>(temp));
    while (std::cin >> temp)
        root = add_to_tree(std::move(root), temp);
    std::queue<std::unique_ptr<TreeNode>> q;
    if (root->left != nullptr)
        q.emplace(std::move(root->left));
    if (root->right != nullptr)
        q.emplace(std::move(root->right));
    while (!q.empty())
    {
        root = std::move(q.front());
        q.pop();
        std::cout << ' ' << root->val;
        if (root->left != nullptr)
            q.emplace(std::move(root->left));
        if (root->right != nullptr)
            q.emplace(std::move(root->right));
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment9.assets/屏幕截图 2025-04-15 223756.png)



### M04078: 实现堆结构

手搓实现，http://cs101.openjudge.cn/practice/04078/

类似的题目是 晴问9.7: 向下调整构建大顶堆，https://sunnywhy.com/sfbj/9/7

思路：

按照堆的维护方法定义成员函数即可。

代码：

```c++
#include <iostream>
#include <stdexcept>
#include <utility>
#include <vector>
template <typename _Tp>
class Heap
{
private:
    std::vector<_Tp> arr;
    void up()
    {
        for (typename std::vector<_Tp>::size_type i = arr.size() - 1; i > 1; i >>= 1)
            if (arr[i] < arr[i >> 1])
                std::swap(arr[i], arr[i >> 1]);
            else
                break;
    }
    void down()
    {
        for (typename std::vector<_Tp>::size_type i = 1; (i << 1) < arr.size();)
        {
            auto temp = i << 1;
            if (temp + 1 < arr.size() && arr[temp + 1] < arr[temp])
                temp += 1;
            if (arr[temp] < arr[i])
            {
                std::swap(arr[temp], arr[i]);
                i = temp;
            }
            else
                break;
        }
    }

public:
    Heap() :
        arr{0}
    {
    }
    void push(const _Tp &x)
    {
        arr.push_back(x);
        up();
    }
    void pop()
    {
        if (arr.size() == 1)
            throw std::runtime_error("Heap Already Empty");
        std::swap(arr.back(), arr[1]);
        arr.pop_back();
        down();
    }
    const _Tp &top()
    {
        if (arr.size() == 1)
            throw std::runtime_error("Heap Already Empty");
        return arr[1];
    }
};
int main()
{
    Heap<int> heap;
    int n, type, u;
    std::cin >> n;
    while (n--)
    {
        std::cin >> type;
        if (type == 1)
        {
            std::cin >> u;
            heap.push(u);
        }
        else
        {
            std::cout << heap.top() << std::endl;
            heap.pop();
        }
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment9.assets/屏幕截图 2025-04-16 105532.png)



### T22161: 哈夫曼编码树

greedy, http://cs101.openjudge.cn/practice/22161/

思路：

与第四题类似。

代码：

```c++
#include <cctype>
#include <iostream>
#include <map>
#include <memory>
#include <ostream>
#include <queue>
#include <string>
#include <utility>
struct Node
{
    char min_character;
    int weight;
    std::shared_ptr<Node> left, right;
    Node(const char &min_character, const int &weight) :
        min_character(min_character), weight(weight), left(nullptr), right(nullptr)
    {
    }
    Node(const char &min_character, const int &weight, std::shared_ptr<Node> &&left, std::shared_ptr<Node> &&right) :
        min_character(min_character), weight(weight), left(std::move(left)), right(std::move(right))
    {
    }
};
bool operator<(const std::shared_ptr<Node> &a, const std::shared_ptr<Node> &b)
{
    return a->weight > b->weight || (a->weight == b->weight && a->min_character > b->min_character);
}
std::map<char, std::string> get_encoder(std::shared_ptr<Node> root, std::string rec = "")
{
    static std::map<char, std::string> encoder;
    if (root->left == nullptr && root->right == nullptr)
        encoder[root->min_character] = rec;
    if (root->left != nullptr)
        get_encoder(root->left, rec + "0");
    if (root->right != nullptr)
        get_encoder(root->right, rec + "1");
    return encoder;
}
int main()
{
    int n;
    std::cin >> n;
    std::priority_queue<std::shared_ptr<Node>> q;
    while (n--)
    {
        char ch;
        int w;
        std::cin >> ch >> w;
        q.emplace(std::make_shared<Node>(ch, w));
    }
    while (q.size() > 1)
    {
        std::shared_ptr<Node> temp1(q.top());
        q.pop();
        std::shared_ptr<Node> temp2(q.top());
        q.pop();
        q.emplace(std::make_shared<Node>(std::min(temp1->min_character, temp2->min_character), temp1->weight + temp2->weight, std::move(temp1), std::move(temp2)));
    }
    std::shared_ptr<Node> root(q.top());
    q.pop();
    auto encoder = get_encoder(root);
    std::string input;
    std::getline(std::cin, input);
    while (std::getline(std::cin, input))
        if (std::isalpha(input[0]))
        {
            for (const auto &i : input)
                std::cout << encoder[i];
            std::cout << std::endl;
        }
        else
        {
            std::shared_ptr<Node> temp = root;
            for (const auto &i : input)
            {
                if (i == '0')
                    temp = temp->left;
                else
                    temp = temp->right;
                if (temp->left == nullptr && temp->right == nullptr)
                {
                    std::cout << temp->min_character;
                    temp = root;
                }
            }
            std::cout << std::endl;
        }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment9.assets/屏幕截图 2025-04-15 223939.png)



## 2. 学习总结和收获

最近继续跟进每日选做。在课上学习了AVL树后，自己查阅了红黑树的实现方法，了解了C++ STL中map与set的底层实现。









