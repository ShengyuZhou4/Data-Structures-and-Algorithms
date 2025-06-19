# Assignment #8: 树为主

Updated 1704 GMT+8 Apr 8, 2025

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

### LC108.将有序数组转换为二叉树

dfs, https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/

思路：

以中心节点为根，递归建树即可。

代码：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        return TreeNode(nums[len(nums) // 2], self.sortedArrayToBST(nums[:len(nums) // 2]), self.sortedArrayToBST(nums[len(nums) // 2 + 1:])) if nums else None
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment8.assets/屏幕截图 2025-04-08 175456.png)



### M27928:遍历树

 adjacency list, dfs, http://cs101.openjudge.cn/practice/27928/

思路：

建树之后按照要求遍历即可。建树的主要难点在于输入时并不一定会先输入父节点，我使用dict存储节点，解决了这个问题。

代码：

```python
from platform import node
from typing import Dict, List, Set


class TreeNode:
    def __init__(self, val: int):
        self.val: int = val
        self.children: List[TreeNode] = []


def traverse(root: TreeNode) -> None:
    flag: bool = True
    root.children.sort(key=lambda x: x.val)
    for child in root.children:
        if flag and child.val > root.val:
            print(root.val)
            flag = False
        traverse(child)
    if flag:
        print(root.val)
    return


def main():
    n: int = int(input())
    tree: Dict[int, TreeNode] = {}
    root: Set[TreeNode] = set()
    for _ in range(n):
        nodes: List[int] = list(map(int, input().split()))
        if nodes[0] not in tree:
            tree[nodes[0]] = TreeNode(nodes[0])
            root.add(tree[nodes[0]])
        for child_val in nodes[1:]:
            if child_val not in tree:
                tree[child_val] = TreeNode(child_val)
            tree[nodes[0]].children.append(tree[child_val])
            if tree[child_val] in root:
                root.remove(tree[child_val])
    traverse(root.pop())


if __name__ == "__main__":
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment8.assets/屏幕截图 2025-04-08 175823.png)



### LC129.求根节点到叶节点数字之和

dfs, https://leetcode.cn/problems/sum-root-to-leaf-numbers/

思路：

用dfs搜索所有可能的数字并累加即可。

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
private:
    int ans = 0;
    void solve(TreeNode* root, const int& rec) {
        if (root->left == root->right)
            ans += rec * 10 + root->val;
        else {
            if (root->left)
                solve(root->left, rec * 10 + root->val);
            if (root->right)
                solve(root->right, rec * 10 + root->val);
        }
    }

public:
    int sumNumbers(TreeNode* root) {
        solve(root, 0);
        return ans;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment8.assets/屏幕截图 2025-04-08 180002.png)



### M22158:根据二叉树前中序序列建树

tree, http://cs101.openjudge.cn/practice/24729/

思路：

用前序序列可以找出树根，随后用中序序列可以找出左右子树的前序与中序序列，于是可以递归建树。

代码：

```c++
#include <iostream>
#include <memory>
#include <string>
#include <utility>
struct TreeNode
{
    char val;
    std::unique_ptr<TreeNode> left, right;
    TreeNode() :
        val(0), left(nullptr), right(nullptr)
    {
    }
    TreeNode(const char &ch) :
        val(ch), left(nullptr), right(nullptr)
    {
    }
    TreeNode(const char &ch, std::unique_ptr<TreeNode> &&l, std::unique_ptr<TreeNode> &&r) :
        val(ch), left(std::move(l)), right(std::move(r))
    {
    }
};
std::unique_ptr<TreeNode> build_tree(std::string &&pre, std::string &&in)
{
    if (pre.size() == 0)
        return nullptr;
    auto pos = in.find(pre[0]);
    std::unique_ptr<TreeNode> root = std::make_unique<TreeNode>(pre[0], build_tree(pre.substr(1, pos), in.substr(0, pos)), build_tree(pre.substr(pos + 1), in.substr(pos + 1)));
    return root;
}
std::unique_ptr<TreeNode> post_order_traversal(std::unique_ptr<TreeNode> root)
{
    if (root == nullptr)
        return nullptr;
    root->left = post_order_traversal(std::move(root->left));
    root->right = post_order_traversal(std::move(root->right));
    std::cout << root->val;
    return root;
}
int main()
{
    std::string preOrder, inOrder;
    while (std::cin >> preOrder >> inOrder)
    {
        post_order_traversal(build_tree(std::move(preOrder), std::move(inOrder)));
        std::cout << std::endl;
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment8.assets/屏幕截图 2025-04-08 191333.png)



### T24729:括号嵌套树

dfs, stack, http://cs101.openjudge.cn/practice/24729/

思路：

根据题目要求递归建树，随后进行遍历即可。

代码：

```c++
#include <iostream>
#include <memory>
#include <string>
#include <utility>
#include <vector>
struct TreeNode
{
    const char val;
    std::vector<std::unique_ptr<TreeNode>> children;
    TreeNode() :
        val(0), children()
    {
    }
    TreeNode(const char &ch) :
        val(ch), children()
    {
    }
};
std::unique_ptr<TreeNode> build_tree(std::string &&tree)
{
    std::unique_ptr<TreeNode> root = std::make_unique<TreeNode>(tree[0]);
    if (tree.size() == 1)
        return root;
    std::string::size_type l = 2, r, cnt = 0;
    for (std::string::size_type i = 1; i != tree.size(); ++i)
        switch (tree[i])
        {
            case '(':
            {
                ++cnt;
                break;
            }
            case ')':
            {
                --cnt;
                if (cnt == 0)
                {
                    r = i;
                    root->children.emplace_back(build_tree(tree.substr(l, r - l)));
                }
                break;
            }
            case ',':
            {
                if (cnt == 1)
                {
                    r = i;
                    root->children.emplace_back(build_tree(tree.substr(l, r - l)));
                    l = i + 1;
                }
                break;
            }
        }
    return root;
}
std::unique_ptr<TreeNode> preorder_traversal(std::unique_ptr<TreeNode> root)
{
    if (root == nullptr)
        return nullptr;
    std::cout << root->val;
    for (auto &i : root->children)
        i = preorder_traversal(std::move(i));
    return root;
}
std::unique_ptr<TreeNode> postorder_traversal(std::unique_ptr<TreeNode> root)
{
    if (root == nullptr)
        return nullptr;
    for (auto &i : root->children)
        i = postorder_traversal(std::move(i));
    std::cout << root->val;
    return root;
}
int main()
{
    std::string input;
    std::cin >> input;
    auto ptr = build_tree(std::move(input));
    ptr = preorder_traversal(std::move(ptr));
    std::cout << std::endl;
    ptr = postorder_traversal(std::move(ptr));
    std::cout << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment8.assets/屏幕截图 2025-04-08 191441.png)



### LC3510.移除最小数对使数组有序II

doubly-linked list + heap, https://leetcode.cn/problems/minimum-pair-removal-to-sort-array-ii/

思路：

此题一个比较自然的思路是用堆维护目前序列中的和最小的相邻的两个数字的位置，用链表存储序列进行模拟操作，但由于std::list<T>::iterator不能比较大小，故使用std::vector进行模拟。

代码：

```c++
class Solution {
private:
    struct node {
        long long val, prev, next;
    };

public:
    int minimumPairRemoval(vector<int>& nums) {
        long long cnt = 0, ans = 0;
        vector<node> arr(nums.size());
        priority_queue<pair<long long, long long>,
                       vector<pair<long long, long long>>,
                       greater<pair<long long, long long>>>
            q;
        for (long long i = 0; i < nums.size() - 1; ++i) {
            arr[i] = {(long long)nums[i], i - 1, i + 1};
            q.push({(long long)(nums[i] + nums[i + 1]), i});
            cnt += (long long)(nums[i] > nums[i + 1]);
        }
        arr[nums.size() - 1] = {(long long)nums.back(),
                                (long long)nums.size() - 2,
                                (long long)nums.size()};
        while (cnt) {
            if (arr[q.top().second].next == nums.size() ||
                arr[q.top().second].val + arr[arr[q.top().second].next].val !=
                    q.top().first) {
                q.pop();
                continue;
            }
            auto top = q.top();
            q.pop();
            ++ans;
            if (arr[top.second].prev >= 0) {
                cnt += -(long long)(arr[arr[top.second].prev].val >
                                    arr[top.second].val) +
                       (long long)(arr[arr[top.second].prev].val >
                                   arr[top.second].val +
                                       arr[arr[top.second].next].val);
                q.push({arr[arr[top.second].prev].val + arr[top.second].val +
                            arr[arr[top.second].next].val,
                        arr[top.second].prev});
            }
            cnt += -(long long)(arr[arr[top.second].next].val <
                                arr[top.second].val);
            if (arr[arr[top.second].next].next < nums.size()) {
                cnt += -(long long)(arr[arr[top.second].next].val >
                                    arr[arr[arr[top.second].next].next].val) +
                       (long long)(arr[top.second].val +
                                       arr[arr[top.second].next].val >
                                   arr[arr[arr[top.second].next].next].val);
                q.push({arr[top.second].val + arr[arr[top.second].next].val +
                            arr[arr[arr[top.second].next].next].val,
                        top.second});
                arr[arr[arr[top.second].next].next].prev = top.second;
            }
            arr[top.second].val += arr[arr[top.second].next].val;
            auto temp = arr[arr[top.second].next].next;
            arr[arr[top.second].next].next = nums.size();
            arr[top.second].next = temp;
        }
        return ans;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment8.assets/屏幕截图 2025-04-08 195527.png)



## 2. 学习总结和收获

最后一题的实现较为复杂，调试花了很多时间。看到题解中有使用list的写法，代码相对较为简短，但时间较慢。











