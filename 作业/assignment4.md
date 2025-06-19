# Assignment #4: 位操作、栈、链表、堆和NN

Updated 1203 GMT+8 Mar 10, 2025

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

### 136.只出现一次的数字

bit manipulation, https://leetcode.cn/problems/single-number/



<mark>请用位操作来实现，并且只使用常量额外空间。</mark>



代码：

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (const auto& num : nums)
            ans ^= num;
        return ans;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment4.assets/屏幕截图 2025-03-12 101022.png)



### 20140:今日化学论文

stack, http://cs101.openjudge.cn/practice/20140/



思路：

类似[LeetCode 394. 字符串编码](https://leetcode.cn/problems/decode-string/description/)，只不过数字的位置稍有变化，故复用了该题代码。思路就是使用栈处理。

代码：

```python
def decodeString(s: str) -> str:
    stack, res, multi = [], "", 0
    for c in s:
        if c == "[":
            stack.append([multi, res])
            res, multi = "", 0
        elif c == "]":
            cur_multi, last_res = stack.pop()
            res = last_res + cur_multi * res
        elif "0" <= c <= "9":
            multi = multi * 10 + int(c)
        else:
            res += c
    return res


s = list(input())
for i in range(len(s) - 1):
    if s[i] == "[" and "0" <= s[i + 1] <= "9":
        s[i], s[i + 1] = s[i + 1], s[i]
print(decodeString("".join(s)))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment4.assets/屏幕截图 2025-03-12 101401.png)



### 160.相交链表

linked list, https://leetcode.cn/problems/intersection-of-two-linked-lists/



思路：

双指针，初始时两指针分别指向两列表首节点，每次循环中向后移动一位，若移动至末尾则跳转至另一首节点，直至两指针指向相同的元素（即公共节点）或均为空指针。

代码：

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getIntersectionNode(ListNode* headA, ListNode* headB) {
        if (headA == NULL || headB == NULL)
            return NULL;
        ListNode *ptr1 = headA, *ptr2 = headB;
        while (ptr1 != ptr2) {
            ptr1 = ptr1 == NULL ? headB : ptr1->next;
            ptr2 = ptr2 == NULL ? headA : ptr2->next;
        }
        return ptr1;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment4.assets/屏幕截图 2025-03-12 105352.png)



### 206.反转链表

linked list, https://leetcode.cn/problems/reverse-linked-list/



思路：

通过连续的两个指针将所有节点指向其之前的元素即可。

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
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment4.assets/屏幕截图 2025-03-12 105638.png)



### 3478.选出和最大的K个元素

heap, https://leetcode.cn/problems/choose-k-elements-with-maximum-sum/



思路：

将两数组按照nums1升序排序，随后进行遍历并运用堆维护当前最大的k个数即可。

代码：

```c++
class Solution {
public:
    vector<long long> findMaxSum(vector<int>& nums1, vector<int>& nums2,
                                 int k) {
        vector<array<int, 3>> nums12;
        for (int i = 0; i < nums1.size(); ++i)
            nums12.push_back({nums1[i], nums2[i], i});
        sort(nums12.begin(), nums12.end());
        long long sum = 0;
        vector<long long> ans(nums12.size());
        priority_queue<int, vector<int>, greater<int>> q;
        for (int i = 0, j = 0; i < nums12.size(); ++i) {
            while (nums12[j][0] < nums12[i][0]) {
                if (q.size() < k) {
                    sum += nums12[j][1];
                    q.push(nums12[j++][1]);
                }
                else
                    if(q.top() < nums12[j][1])
                    {
                        sum += nums12[j][1] - q.top();
                        q.push(nums12[j++][1]);
                        q.pop();
                    }
                    else
                        ++j;
            }
            ans[nums12[i][2]] = sum;
        }
        return ans;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment4.assets/屏幕截图 2025-03-12 105852.png)



### Q6.交互可视化neural network

https://developers.google.com/machine-learning/crash-course/neural-networks/interactive-exercises

**Your task:** configure a neural network that can separate the orange dots from the blue dots in the diagram, achieving a loss of less than 0.2 on both the training and test data.

**Instructions:**

In the interactive widget:

1. Modify the neural network hyperparameters by experimenting with some of the following config settings:
   - Add or remove hidden layers by clicking the **+** and **-** buttons to the left of the **HIDDEN LAYERS** heading in the network diagram.
   - Add or remove neurons from a hidden layer by clicking the **+** and **-** buttons above a hidden-layer column.
   - Change the learning rate by choosing a new value from the **Learning rate** drop-down above the diagram.
   - Change the activation function by choosing a new value from the **Activation** drop-down above the diagram.
2. Click the Play button above the diagram to train the neural network model using the specified parameters.
3. Observe the visualization of the model fitting the data as training progresses, as well as the **Test loss** and **Training loss** values in the **Output** section.
4. If the model does not achieve loss below 0.2 on the test and training data, click reset, and repeat steps 1–3 with a different set of configuration settings. Repeat this process until you achieve the preferred results.

给出满足约束条件的<mark>截图</mark>，并说明学习到的概念和原理。

![](./assignment4.assets/屏幕截图 2025-03-15 150704.png)

了解到了神经网络的基本工作原理，还知道了正则化在防止过拟合方面的应用。在这个任务里神经元与隐藏层的数量并不是越多越好，事实上即便加入了大量的神经元，训练后期其中的大部分都处于近似无效的状态。



## 2. 学习总结和收获

数据结构的大部分题目难度主要在于如何正确实现程序，而不是如何想出解题思路。Q6的网址是一个很好的学习机器学习的平台，在上面了解了很多基础知识。

