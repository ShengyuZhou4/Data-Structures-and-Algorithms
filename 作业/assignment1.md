# Assignment #1: 虚拟机，Shell & 大语言模型

Updated 2309 GMT+8 Feb 20, 2025

2025 spring, Complied by 周晟昱 工学院



**作业的各项评分细则及对应的得分**

| 标准                                 | 等级                                                         | 得分 |
| ------------------------------------ | ------------------------------------------------------------ | ---- |
| 按时提交                             | 完全按时提交：1分<br/>提交有请假说明：0.5分<br/>未提交：0分  | 1 分 |
| 源码、耗时（可选）、解题思路（可选） | 提交了4个或更多题目且包含所有必要信息：1分<br/>提交了2个或以上题目但不足4个：0.5分<br/>少于2个：0分 | 1 分 |
| AC代码截图                           | 提交了4个或更多题目且包含所有必要信息：1分<br/>提交了2个或以上题目但不足4个：0.5分<br/>少于：0分 | 1 分 |
| 清晰头像、PDF文件、MD/DOC附件        | 包含清晰的Canvas头像、PDF文件以及MD或DOC格式的附件：1分<br/>缺少上述三项中的任意一项：0.5分<br/>缺失两项或以上：0分 | 1 分 |
| 学习总结和个人收获                   | 提交了学习总结和个人收获：1分<br/>未提交学习总结或内容不详：0分 | 1 分 |
| 总得分： 5                           | 总分满分：5分                                                |      |
>
> 
>
> **说明：**
>
> 1. **解题与记录：**
>       - 对于每一个题目，请提供其解题思路（可选），并附上使用Python或C++编写的源代码（确保已在OpenJudge， Codeforces，LeetCode等平台上获得Accepted）。请将这些信息连同显示“Accepted”的截图一起填写到下方的作业模板中。（推荐使用Typora https://typoraio.cn 进行编辑，当然你也可以选择Word。）无论题目是否已通过，请标明每个题目大致花费的时间。
>    
>2. **课程平台与提交安排：**
> 
>   - 我们的课程网站位于Canvas平台（https://pku.instructure.com ）。该平台将在第2周选课结束后正式启用。在平台启用前，请先完成作业并将作业妥善保存。待Canvas平台激活后，再上传你的作业。
> 
>       - 提交时，请首先上传PDF格式的文件，并将.md或.doc格式的文件作为附件上传至右侧的“作业评论”区。确保你的Canvas账户有一个清晰可见的头像，提交的文件为PDF格式，并且“作业评论”区包含上传的.md或.doc附件。
> 
>3. **延迟提交：**
> 
>   - 如果你预计无法在截止日期前提交作业，请提前告知具体原因。这有助于我们了解情况并可能为你提供适当的延期或其他帮助。 
> 
>请按照上述指导认真准备和提交作业，以保证顺利完成课程要求。



## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/practice/27653/



思路：

按照分数运算法则重载运算符即可。可以重载输入输出。在类内实现了最大公约数求解用于通/约分。

代码：

```c++
#include <iostream>
template <typename T = int>
class Fraction
{
public:
    Fraction(T numerator, T denominator) :
        numerator(numerator), denominator(denominator)
    {
        Reduction();
    }
    Fraction(Fraction<> const &other) :
        numerator(other.numerator), denominator(other.denominator)
    {
    }
    Fraction(Fraction<> &&other) :
        numerator(other.numerator), denominator(other.denominator)
    {
    }
    Fraction &operator=(Fraction<> const &other)
    {
        numerator = other.numerator;
        denominator = other.denominator;
        return *this;
    }
    Fraction &operator=(Fraction<> &&other)
    {
        numerator = other.numerator;
        denominator = other.denominator;
        return *this;
    }
    Fraction() :
        numerator(0), denominator(1)
    {
    }
    friend std::istream &operator>>(std::istream &is, Fraction<> &fraction);
    friend std::ostream &operator<<(std::ostream &os, const Fraction<> &fraction);
    Fraction operator-() const
    {
        return Fraction(-numerator, denominator);
    }
    Fraction operator+(const Fraction &other) const
    {
        T l = lcm(denominator, other.denominator);
        return Fraction(numerator * (l / denominator) + other.numerator * (l / other.denominator), l);
    }
    Fraction operator-(const Fraction &other) const
    {
        T l = lcm(denominator, other.denominator);
        return Fraction(numerator * (l / denominator) - other.numerator * (l / other.denominator), l);
    }
    Fraction operator*(const Fraction &other) const
    {
        return Fraction(numerator * other.numerator, denominator * other.denominator);
    }
    Fraction operator/(const Fraction &other) const
    {
        return Fraction(numerator * other.denominator, denominator * other.numerator);
    }
    bool operator==(const Fraction &other) const
    {
        return numerator == other.numerator && denominator == other.denominator;
    }

private:
    T numerator, denominator;
    T gcd(T a, T b) const
    {
        T c;
        while (b != 0)
        {
            c = a % b;
            a = b;
            b = c;
        }
        return a;
    }
    T lcm(T a, T b) const
    {
        return (a * b) / gcd(a, b);
    }
    void Reduction()
    {
        if (denominator < 0)
        {
            numerator = -numerator;
            denominator = -denominator;
        }
        if (denominator == 0)
        {
            throw std::invalid_argument("Denominator cannot be zero");
        }
        if (numerator == 0)
        {
            denominator = 1;
            return;
        }
        T g = gcd(numerator, denominator);
        numerator /= g;
        denominator /= g;
    }
};
std::istream &operator>>(std::istream &is, Fraction<> &fraction)
{
    is >> fraction.numerator >> fraction.denominator;
    fraction.Reduction();
    return is;
}
std::ostream &operator<<(std::ostream &os, const Fraction<> &fraction)
{
    os << fraction.numerator << "/" << fraction.denominator;
    return os;
}

int main()
{
    Fraction<int> f1, f2;
    std::cin >> f1 >> f2;
    std::cout << f1 + f2 << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment1.assets/屏幕截图 2025-03-01 184225.png)



### 1760.袋子里最少数目的球

 https://leetcode.cn/problems/minimum-limit-of-balls-in-a-bag/




思路：

二分答案。搜索最小代价，并检测是否可行。

代码：

```c++
class Solution {
public:
    bool check(const vector<int>& nums, int maxOperations, const int& cost) {
        int cnt = 0;
        for (const auto& num : nums)
            cnt += num / cost + (num % cost ? 0 : -1);
        return cnt <= maxOperations;
    }
    int minimumSize(vector<int>& nums, int maxOperations) {
        int max_num = 0;
        long long sum = 0;
        for (const auto& num : nums) {
            max_num = max(max_num, num);
            sum += num;
        }
        int l = max(1ull, sum / (nums.size() + maxOperations)), r = max_num;
        while (l < r) {
            int mid = (l + r) / 2;
            if (check(nums, maxOperations, mid))
                r = mid;
            else
                l = mid + 1;
        }
        return l;
    }
};
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment1.assets/屏幕截图 2025-03-01 184456.png)



### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135



思路：

同样是二分答案。

代码：

```c++
#include <algorithm>
#include <iostream>
#include <vector>
int check(const std::vector<int> costs, const int &limit)
{
    int sum = 0, cnt = 0;
    for (const auto &cost : costs)
        if (sum + cost > limit)
        {
            ++cnt;
            sum = cost;
        }
        else
            sum += cost;
    return cnt + 1;
}
int main()
{
    int n, m, max_cost = 0, sum = 0;
    std::cin >> n >> m;
    std::vector<int> costs(n);
    for (auto &cost : costs)
    {
        std::cin >> cost;
        max_cost = std::max(max_cost, cost);
        sum += cost;
    }
    int l = max_cost, r = sum;
    while (l < r)
    {
        int mid = (l + r) / 2;
        if (check(costs, mid) <= m)
            r = mid;
        else
            l = mid + 1;
    }
    std::cout << l << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment1.assets/屏幕截图 2025-03-01 184621.png)



### 27300: 模型整理

http://cs101.openjudge.cn/practice/27300/



思路：

利用内置函数分割模型名称和参数大小，进行排序即可。由于不知道Python中是否存在类似于C++中的map的有序字典，使用了另外的链表存储名称并排序。

代码：

```python
from shlex import join


def main():
    n = int(input())
    model_list = []
    model_parameter = {}
    for _ in range(n):
        name, parameter = input().strip().split("-")
        if name in model_parameter:
            model_parameter[name].append(parameter)
        else:
            model_list.append(name)
            model_parameter[name] = [parameter]
    model_list.sort()
    for name in model_list:
        model_parameter[name].sort(
            key=lambda x: float(x[:-1]) * (1000 if x[-1] == "B" else 1)
        )
        print(f"{name}: " + ", ".join(model_parameter[name]))


if __name__ == "__main__":
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment1.assets/屏幕截图 2025-03-01 184900.png)



### Q5. 大语言模型（LLM）部署与测试

本任务旨在本地环境或通过云虚拟机（如 https://clab.pku.edu.cn/ 提供的资源）部署大语言模型（LLM）并进行测试。用户界面方面，可以选择使用图形界面工具如 https://lmstudio.ai 或命令行界面如 https://www.ollama.com 来完成部署工作。

测试内容包括选择若干编程题目，确保这些题目能够在所部署的LLM上得到正确解答，并通过所有相关的测试用例（即状态为Accepted）。选题应来源于在线判题平台，例如 OpenJudge、Codeforces、LeetCode 或洛谷等，同时需注意避免与已找到的AI接受题目重复。已有的AI接受题目列表可参考以下链接：
https://github.com/GMyhf/2025spring-cs201/blob/main/AI_accepted_locally.md

请提供你的最新进展情况，包括任何关键步骤的截图以及遇到的问题和解决方案。这将有助于全面了解项目的推进状态，并为进一步的工作提供参考。

由于我的笔记本没有独立显卡，官方版本的ollama无法调用核显（我使用的是Intel® Core™ Ultra 7 258V），只能使用CPU推理，故使用了Intel®提供的[ipex-cpp](https://github.com/intel/ipex-llm/blob/main/docs/mddocs/Quickstart/ollama_quickstart.zh-CN.md)调用核显部署大模型。由于核显拥有17.9GB的共享显存，实测可以运行deepseek-r1:14b，但输出速度较慢，目测只有3~5tokens/s。然而很不幸，在我的机器上思维链较长时模型会产生幻觉，在中途开始输出跟我的问题毫不相干的内容，最后还是使用qwen2.5-coder进行测试。

解决了该问题：[27310:积木](http://cs101.openjudge.cn/practice/27310/)，输出如下：

> 要解决这个问题，我们可以使用回溯算法来尝试所有可能的组合来拼写单词。具体步骤如下：
>
> 1. 读取输入数据，包括四块积木上的字母和想要拼写的单词列表。
> 2. 对于每个单词，尝试使用回溯法检查是否可以通过积木拼写出该单词。
> 3. 在回溯过程中，确保每块积木的字母只能使用一次。
>
> 下面是实现这个算法的C++代码：
>
> ```cpp
> #include <iostream>
> #include <vector>
> #include <string>
> 
> using namespace std;
> 
> // 检查当前单词是否可以用四块积木拼出
> bool canSpell(const vector<string>& blocks, const string& word, int blockIndex, vector<bool> usedBlocks) {
>     if (blockIndex == word.size()) {
>         return true;
>     }
> 
>     for (int i = 0; i < 4; ++i) {
>         if (!usedBlocks[i] && blocks[i].find(word[blockIndex]) != string::npos) {
>             // 使用第i块积木
>             usedBlocks[i] = true;
>             // 尝试拼写下一个字母
>             if (canSpell(blocks, word, blockIndex + 1, usedBlocks)) {
>                 return true;
>             }
>             // 回溯，取消使用第i块积木
>             usedBlocks[i] = false;
>         }
>     }
> 
>     return false;
> }
> 
> int main() {
>     int N;
>     cin >> N;
> 
>     vector<string> blocks(4);
>     for (int i = 0; i < 4; ++i) {
>         cin >> blocks[i];
>     }
> 
>     while (N--) {
>         string word;
>         cin >> word;
> 
>         if (canSpell(blocks, word, 0, vector<bool>(4, false))) {
>             cout << "YES" << endl;
>         } else {
>             cout << "NO" << endl;
>         }
>     }
> 
>     return 0;
> }
> ```
>
> ### 代码解释
>
> 1. **输入部分**：
>    - 首先读取单词的数量 `N`。
>    - 然后读取四块积木上的字母，存储在 `blocks` 数组中。
>
> 2. **拼写检查函数 `canSpell`**：
>    - 该函数使用递归和回溯来尝试拼写单词。
>    - `blockIndex` 表示当前正在处理的单词中的字母索引。
>    - `usedBlocks` 是一个布尔数组，用于记录每块积木是否已经被使用。
>    - 如果所有字母都成功匹配，则返回 `true`。
>
> 3. **主函数**：
>    - 对于每个单词，调用 `canSpell` 函数进行检查，并输出结果。
>
> 这个算法通过回溯尝试所有可能的组合来判断是否可以拼写出每个单词，确保每块积木的字母只能使用一次。



### Q6. 阅读《Build a Large Language Model (From Scratch)》第一章

作者：Sebastian Raschka

请整理你的学习笔记。这应该包括但不限于对第一章核心概念的理解、重要术语的解释、你认为特别有趣或具有挑战性的内容，以及任何你可能有的疑问或反思。通过这种方式，不仅能巩固你自己的学习成果，也能帮助他人更好地理解这一部分内容。

计算机对NLP任务的处理方式非常有意思，即通过前方的所有文本计算下一词的概率分布，这与人类的思维过程有较大的区别。未来的模型能否对自身生成的内容进行反复迭代计算以优化最终结果？或者目前的思维链模型已经在一定程度上起到了这种作用？



## 2. 学习总结和个人收获 

部署LLM时曾经尝试使用OpenVINO调用NPU，然而方法太繁琐，限于水平没有成功。目前来看笔记本上的NPU只有在开视频会议使用背景虚化时有用，绝大部分时候都是空闲状态，有些可惜。





