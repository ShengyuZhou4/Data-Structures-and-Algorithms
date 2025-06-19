# Assignment #6: 回溯、树、双向链表和哈希表

Updated 1526 GMT+8 Mar 22, 2025

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

### LC46.全排列

backtracking, https://leetcode.cn/problems/permutations/

思路：

直接使用递归枚举即可。注意可以通过使用交换原地修改降低空间复杂度。

代码：

```c++
class Solution {
private:
    vector<vector<int>> ans;
    void solve(vector<int>& nums, const int& pos) {
        if (pos == nums.size()) {
            ans.push_back(nums);
            return;
        }
        for (int i = pos; i < nums.size(); ++i) {
            swap(nums[i], nums[pos]);
            solve(nums, pos + 1);
            swap(nums[i], nums[pos]);
        }
    }

public:
    vector<vector<int>> permute(vector<int>& nums) {
        solve(nums, 0);
        return ans;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment6.assets/屏幕截图 2025-03-25 164438.png)



### LC79: 单词搜索

backtracking, https://leetcode.cn/problems/word-search/

思路：

枚举可能的搜索起点进行DFS即可。

代码：

```c++
class Solution {
private:
    const array<int, 4> dx{-1, 0, 1, 0}, dy{0, -1, 0, 1};
    bool find(const vector<vector<char>>& board, vector<vector<bool>>& vis,
              const string& word, const int& pos, const int& x, const int& y) {
        if (pos == word.size())
            return true;
        vis[x][y] = true;
        for (int nx, ny, i = 0; i < 4; ++i) {
            nx = x + dx[i];
            ny = y + dy[i];
            if (nx < 0 || ny < 0 || nx >= board.size() ||
                ny >= board[0].size() || vis[nx][ny] ||
                board[nx][ny] != word[pos])
                continue;
            if (find(board, vis, word, pos + 1, nx, ny))
                return true;
        }
        return vis[x][y] = false;
    }

public:
    bool exist(vector<vector<char>>& board, string word) {
        vector<vector<bool>> vis(board.size(),
                                 vector<bool>(board[0].size(), false));
        for (int i = 0; i < board.size(); ++i)
            for (int j = 0; j < board[0].size(); ++j)
                if (board[i][j] == word[0]) {
                    vis[i][j] = true;
                    if (find(board, vis, word, 1, i, j))
                        return true;
                    vis[i][j] = false;
                }
        return false;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment6.assets/屏幕截图 2025-03-25 170106.png)



### LC94.二叉树的中序遍历

dfs, https://leetcode.cn/problems/binary-tree-inorder-traversal/

思路：

按照定义遍历即可。

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
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if (root == nullptr)
            return vector<int>();
        vector<int> l(inorderTraversal(root->left)), r(inorderTraversal(root->right));
        l.reserve(l.size() + 1 + r.size());
        l.push_back(root->val);
        l.insert(l.end(), r.begin(), r.end());
        return l;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment6.assets/屏幕截图 2025-03-25 170214.png)



### LC102.二叉树的层序遍历

bfs, https://leetcode.cn/problems/binary-tree-level-order-traversal/

思路：

每次遍历一层（第一次仅有树根），将该层中所有节点的子节点存入另一个列表中，如此循环并存储即可。

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
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if (root == nullptr)
            return ans;
        unique_ptr<vector<TreeNode*>> ptr1(make_unique<vector<TreeNode*>>()), ptr2;
        ptr1->push_back(root);
        while(!ptr1->empty())
        {
            ans.emplace_back();
            ptr2 = make_unique<vector<TreeNode*>>();
            for (auto i : *ptr1)
            {
                if(i->left != nullptr)
                    ptr2->push_back(i->left);
                if(i->right != nullptr)
                    ptr2->push_back(i->right);
                ans.back().push_back(i->val);
            }
            ptr1 = move(ptr2);
        }
        return ans;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment6.assets/屏幕截图 2025-03-25 172118.png)



### LC131.分割回文串

dp, backtracking, https://leetcode.cn/problems/palindrome-partitioning/

思路：

用dp预处理出$f_{i,j}$，表示$s_{i…j}$是否为回文串，随后用dfs搜索即可。

代码：

```c++
class Solution {
private:
    vector<vector<string>> ret;
    vector<string> ans;
    void solve(const string& s, const int& i,
                                 const vector<vector<bool>>& f) {
        if (i == s.size()) {
            ret.push_back(ans);
            return;
        }
        for (int j = i; j < s.size(); ++j) {
            if (f[i][j]) {
                ans.push_back(s.substr(i, j - i + 1));
                solve(s, j + 1, f);
                ans.pop_back();
            }
        }
        return;
    }

public:
    vector<vector<string>> partition(string s) {
        vector<vector<bool>> f(s.size(), vector<bool>(s.size(), true));
        for (int i = s.size() - 1; i >= 0; --i)
            for (int j = i + 1; j < s.size(); ++j)
                f[i][j] = s[i] == s[j] && f[i + 1][j - 1];
        solve(s, 0, f);
        return ret;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment6.assets/屏幕截图 2025-03-25 175549.png)



### LC146.LRU缓存

hash table, doubly-linked list, https://leetcode.cn/problems/lru-cache/

思路：

题目要求函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行，故考虑使用哈希表（unorederd_map）；又要求只保留最后使用的特定个关键字，故可以考虑使用链表。在每次访问时，用哈希表找到关键字在链表中的位置，将其移到队尾；在链表中元素数量超出上限时，移除队首元素即可。

代码：

```c++
class LRUCache {
public:
    LRUCache(int capacity) : size(capacity) {}

    int get(int key) {
        if (HashMap.find(key) == HashMap.end())
            return -1;
        auto it = HashMap[key];
        auto key_value = *it;
        HashMap[key] = cache.insert(cache.end(), key_value);
        cache.erase(it);
        return key_value.second;
    }

    void put(int key, int value) {
        if (HashMap.find(key) != HashMap.end())
            cache.erase(HashMap[key]);
        HashMap[key] = cache.insert(cache.end(), make_pair(key, value));
        while (cache.size() > size) {
            HashMap.erase(cache.front().first);
            cache.pop_front();
        }
    }

private:
    list<pair<int, int>> cache;
    unordered_map<int, list<pair<int, int>>::iterator> HashMap;
    const int size;
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment6.assets/屏幕截图 2025-03-25 180710.png)



## 2. 学习总结和收获

每日选做里的一些题目（尤其是搜索题）有很多容易忽略的细节与特殊情况，稍有不慎就会WA，耗时较多。做全排列时看到了C++ STL中next_permutation函数的一种实现，不需要使用递归，直接寻找末端最长的降序区间，设其长度为s，将其中所有大于倒数第s+1个元素的元素中最小者与之交换再将降序区间反转即可，感觉很巧妙。











