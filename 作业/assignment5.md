# Assignment #5: 链表、栈、队列和归并排序

Updated 1348 GMT+8 Mar 17, 2025

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

### LC21.合并两个有序链表

linked list, https://leetcode.cn/problems/merge-two-sorted-lists/

思路：

每次比较两链表首元素值的大小，将较小值加入队尾并从原链表删除即可。

代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if (list1 == nullptr)
            return list2;
        if (list2 == nullptr)
            return list1;
        ListNode *head, *tail;
        if (list1->val <= list2->val) {
            head = list1;
            list1 = list1->next;
        } else {
            head = list2;
            list2 = list2->next;
        }
        tail = head;
        while (list1 != nullptr && list2 != nullptr)
            if (list1->val <= list2->val) {
                tail->next = list1;
                list1 = list1->next;
                tail = tail->next;
            } else {
                tail->next = list2;
                list2 = list2->next;
                tail = tail->next;
            }
        if (list1 != nullptr)
            tail->next = list1;
        else
            tail->next = list2;
        return head;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment5.assets/屏幕截图 2025-03-18 151310.png)



### LC234.回文链表

linked list, https://leetcode.cn/problems/palindrome-linked-list/

<mark>请用快慢指针实现。</mark>



代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == nullptr)
            return nullptr;
        ListNode* prev = nullptr;
        ListNode* current = head;
        while(head->next != nullptr)
        {
            head = head->next;
            current->next = prev;
            prev = current;
            current = head;
        }
        current->next = prev;
        return head;
    }
    bool isPalindrome(ListNode* head) {
        if(head == nullptr)
            return true;
        ListNode *slow_ptr = head, *fast_ptr = head;
        while(fast_ptr->next != nullptr && fast_ptr->next->next != nullptr)
        {
            fast_ptr = fast_ptr->next->next;
            slow_ptr = slow_ptr->next;
        }
        ListNode* head2 = reverseList(slow_ptr->next);
        while(head != nullptr && head2 != nullptr)
        {
            if(head->val != head2->val)
                return false;
            head = head->next;
            head2 = head2->next;
        }
        return true;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment5.assets/屏幕截图 2025-03-18 151524.png)



### LC1472.设计浏览器历史记录

doubly-lined list, https://leetcode.cn/problems/design-browser-history/

<mark>请用双链表实现。</mark>



代码：

```c++
class BrowserHistory {
private:
    list<string> history;
    list<string>::iterator pos;

public:
    BrowserHistory(string homepage) {
        history.push_back(move(homepage));
        pos = history.begin();
    }

    void visit(string url) {
        history.erase(++pos, history.end());
        pos = history.insert(history.end(), move(url));
    }

    string back(int steps) {
        while (pos != history.begin() && steps--)
            --pos;
        return *pos;
    }

    string forward(int steps) {
        while (pos != history.end() && steps--)
            ++pos;
        if (pos == history.end())
            --pos;
        return *pos;
    }
};

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory* obj = new BrowserHistory(homepage);
 * obj->visit(url);
 * string param_2 = obj->back(steps);
 * string param_3 = obj->forward(steps);
 */
```

其实直接用vector模拟也可以？

```c++
class BrowserHistory {
private:
    vector<string> history;
    vector<string>::size_type pos;

public:
    BrowserHistory(string homepage) {
        history.push_back(move(homepage));
        pos = 0;
    }

    void visit(string url) {
        history.resize(++pos + 1);
        history.back() = move(url);
    }

    string back(int steps) {
        if (pos >= steps)
            pos -= steps;
        else
            pos = 0;
        return history[pos];
    }

    string forward(int steps) {
        if (pos + steps < history.size())
            pos += steps;
        else
            pos = history.size() - 1;
        return history[pos];
    }
};

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory* obj = new BrowserHistory(homepage);
 * obj->visit(url);
 * string param_2 = obj->back(steps);
 * string param_3 = obj->forward(steps);
 */
```

通过提前分配空间优化时间表现：

```c++
class BrowserHistory {
private:
    vector<string> history;
    vector<string>::size_type pos;

public:
    BrowserHistory(string homepage) {
        history.reserve(5000);
        history.push_back(move(homepage));
        pos = 0;
    }

    void visit(string url) {
        history.resize(++pos + 1);
        history.back() = move(url);
    }

    string back(int steps) {
        if (pos >= steps)
            pos -= steps;
        else
            pos = 0;
        return history[pos];
    }

    string forward(int steps) {
        if (pos + steps < history.size())
            pos += steps;
        else
            pos = history.size() - 1;
        return history[pos];
    }
};

/**
 * Your BrowserHistory object will be instantiated and called as such:
 * BrowserHistory* obj = new BrowserHistory(homepage);
 * obj->visit(url);
 * string param_2 = obj->back(steps);
 * string param_3 = obj->forward(steps);
 */
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment5.assets/屏幕截图 2025-03-18 154650.png)

![](./assignment5.assets/屏幕截图 2025-03-18 153515.png)

![](./assignment5.assets/屏幕截图 2025-03-18 155117.png)



### 24591: 中序表达式转后序表达式

stack, http://cs101.openjudge.cn/practice/24591/

思路：

使用两个栈，从左到右读取数字/符号，遇到数字则压入$nums$，遇到符号则将$ops$中所有优先级高于该符号的符号弹出压入$nums$中。符号优先级如下定义：在同一括号中，乘除的优先级高于加减，乘除或加减相比左侧的符号优先级更高。遇到右括号时则弹出直至遇到左括号。注意括号不会被压入$nums$。最终nums中存储的就是后序表达式。

代码：

```python
n = int(input())
for _ in range(n):
    ops = []
    nums = []
    expr = input().strip()
    a, b = 0, 0
    for b in range(len(expr)):
        if expr[b] in '+-*/()':
            if a != b:
                nums.append(expr[a:b])
            a = b + 1
            if expr[b] == '(':
                ops.append(expr[b])
            elif expr[b] == ')':
                while ops[-1] != '(':
                    nums.append(ops.pop())
                ops.pop()
            else:
                while ops and ops[-1] != '(' and not (ops[-1] in '+-' and expr[b] in '*/'):
                    nums.append(ops.pop())
                ops.append(expr[b])
    if a != len(expr):
        nums.append(expr[a:])
    while ops:
        nums.append(ops.pop())
    print(' '.join(nums))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment5.assets/屏幕截图 2025-03-18 160345.png)



### 03253: 约瑟夫问题No.2

queue, http://cs101.openjudge.cn/practice/03253/

<mark>请用队列实现。</mark>



代码：

```python
from collections import deque


def main():
    n, p, m = map(int, input().split())
    while n + p + m != 0:
        q = deque()
        for i in range(n):
            q.append((p + i - 1) % n + 1)
        cnt = 1
        ans = []
        while q:
            if cnt == m:
                ans.append(str(q.popleft()))
                cnt = 1
            else:
                q.append(q.popleft())
                cnt += 1
        print(",".join(ans))
        n, p, m = map(int, input().split())


if __name__ == "__main__":
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment5.assets/屏幕截图 2025-03-18 161458.png)



### 20018: 蚂蚁王国的越野跑

merge sort, http://cs101.openjudge.cn/practice/20018/

思路：

相当于统计“正序对”数量，使用用归并排序统计逆序对数量的算法即可。

代码：

```python
def merge_sort(a, l, r, cnt):
    if l >= r:
        return
    mid = (l + r) // 2
    merge_sort(a, l, mid, cnt)
    merge_sort(a, mid + 1, r, cnt)
    tmp = []
    i, j = l, mid + 1
    while i <= mid and j <= r:
        if a[i] <= a[j]:
            tmp.append(a[i])
            i += 1
        else:
            tmp.append(a[j])
            j += 1
            cnt[0] += mid - i + 1
    tmp.extend(a[i : mid + 1])
    tmp.extend(a[j : r + 1])
    a[l : r + 1] = tmp


def main():
    n = int(input())
    a = []
    for _ in range(n):
        a.append(-int(input()))
    cnt = [0]
    merge_sort(a, 0, n - 1, cnt)
    print(cnt[0])


if __name__ == "__main__":
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment5.assets/屏幕截图 2025-03-18 161711.png)



## 2. 学习总结和收获

设计浏览器历史记录中使用vector时如果不预先分配空间速度会比使用双向链表更慢，可见重新分配空间的时间开销还是比较大的；然而却更加节省空间，这可能是因为链表的每个节点还需要存储前序和后继元素的地址，造成了额外的空间使用。分配空间之后速度更快而空间耗费更大倒是符合预期。











