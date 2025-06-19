# Assignment #2: 深度学习与大语言模型

Updated 2204 GMT+8 Feb 25, 2025

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

### 18161: 矩阵运算

matrices, http://cs101.openjudge.cn/practice/18161



思路：

按运算规则直接计算即可。

代码：

```c++
#include <iostream>
#include <vector>

int main()
{
    long long row1, col1, row2, col2, row3, col3;
    std::cin >> row1 >> col1;
    std::vector<std::vector<long long> > A(row1, std::vector<long long>(col1));
    for (long long i = 0; i < row1; i++)
        for (long long j = 0; j < col1; j++)
            std::cin >> A[i][j];
    std::cin >> row2 >> col2;
    std::vector<std::vector<long long> > B(row2, std::vector<long long>(col2));
    for (long long i = 0; i < row2; i++)
        for (long long j = 0; j < col2; j++)
            std::cin >> B[i][j];
    std::cin >> row3 >> col3;
    std::vector<std::vector<long long> > C(row3, std::vector<long long>(col3));
    for (long long i = 0; i < row3; i++)
        for (long long j = 0; j < col3; j++)
            std::cin >> C[i][j];
    if (row2 != col1 || row1 != row3 || col2 != col3)
        std::cout << "Error!" << std::endl;
    else
    {
        std::vector<std::vector<long long> > Ans(row1, std::vector<long long>(col2));
        for (long long i = 0; i < row1; i++)
            for (long long j = 0; j < col2; j++)
                for (long long k = 0; k < col1; k++)
                    Ans[i][j] += A[i][k] * B[k][j];
        for (long long i = 0; i < row1; i++)
            for (long long j = 0; j < col2; j++)
                std::cout<< Ans[i][j] + C[i][j] << (j == col2 - 1 ? "\n" : " ");
    }
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment2.assets/屏幕截图 2025-03-01 192714.png)



### 19942: 二维矩阵上的卷积运算

matrices, http://cs101.openjudge.cn/practice/19942/




思路：

按照题意计算即可。

代码：

```c++
#include <cstddef>
#include <iostream>
#include <vector>

inline long long convolution(const std::vector<std::vector<long long>> &A, const std::vector<std::vector<long long>> &B, size_t x,  size_t y)
{
    long long ans = 0;
    for(size_t i = 0; i < B.size(); ++i)
        for(size_t j = 0; j < B[i].size(); ++j)
            ans += A[i + x][j + y] * B[i][j];
    return ans;
}
int main()
{
    size_t m, n, p, q;
    std::cin >> m >> n >> p >> q;
    std::vector<std::vector<long long>> A(m, std::vector<long long>(n)), B(p, std::vector<long long>(q));
    for(size_t i = 0; i < m; ++i)
        for(size_t j = 0; j < n; ++j)
            std::cin >> A[i][j];
    for(size_t i = 0; i < p; ++i)
        for(size_t j = 0; j < q; ++j)
            std::cin >> B[i][j];
    for(size_t i = 0; i < m - p + 1; ++i)
        for(size_t j = 0; j < n - q + 1; ++j)
            std::cout << convolution(A, B, i, j) << (j == n - q ? '\n' : ' ');
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment2.assets/屏幕截图 2025-03-01 192839.png)



### 04140: 方程求解

牛顿迭代法，http://cs101.openjudge.cn/practice/04140/

请用<mark>牛顿迭代法</mark>实现。

因为大语言模型的训练过程中涉及到了梯度下降（或其变种，如SGD、Adam等），用于优化模型参数以最小化损失函数。两种方法都是通过迭代的方式逐步接近最优解。每一次迭代都基于当前点的局部信息调整参数，试图找到一个比当前点更优的新点。理解牛顿迭代法有助于深入理解基于梯度的优化算法的工作原理，特别是它们如何利用导数信息进行决策。

> **牛顿迭代法**
>
> - **目的**：主要用于寻找一个函数 $f(x)$ 的根，即找到满足 $f(x)=0$ 的 $x$ 值。不过，通过适当变换目标函数，它也可以用于寻找函数的极值。
> - **方法基础**：利用泰勒级数的一阶和二阶项来近似目标函数，在每次迭代中使用目标函数及其导数的信息来计算下一步的方向和步长。
> - **迭代公式**：$ x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)} $ 对于求极值问题，这可以转化为$ x_{n+1} = x_n - \frac{f'(x_n)}{f''(x_n)} $，这里 $f'(x)$ 和 $f''(x)$ 分别是目标函数的一阶导数和二阶导数。
> - **特点**：牛顿法通常具有更快的收敛速度（尤其是对于二次可微函数），但是需要计算目标函数的二阶导数（Hessian矩阵在多维情况下），并且对初始点的选择较为敏感。
>
> **梯度下降法**
>
> - **目的**：直接用于寻找函数的最小值（也可以通过取负寻找最大值），尤其在机器学习领域应用广泛。
> - **方法基础**：仅依赖于目标函数的一阶导数信息（即梯度），沿着梯度的反方向移动以达到减少函数值的目的。
> - **迭代公式**：$ x_{n+1} = x_n - \alpha \cdot \nabla f(x_n) $ 这里 $\alpha$ 是学习率，$\nabla f(x_n)$ 表示目标函数在 $x_n$ 点的梯度。
> - **特点**：梯度下降不需要计算复杂的二阶导数，因此在高维空间中相对容易实现。然而，它的收敛速度通常较慢，特别是当目标函数的等高线呈现出椭圆而非圆形时（即存在条件数大的情况）。
>
> **相同与不同**
>
> - **相同点**：两者都可用于优化问题，试图找到函数的极小值点；都需要目标函数至少一阶可导。
> - **不同点**：
>   - 牛顿法使用了更多的局部信息（即二阶导数），因此理论上收敛速度更快，但在实际应用中可能会遇到计算成本高、难以处理大规模数据集等问题。
>   - 梯度下降则更为简单，易于实现，特别是在高维空间中，但由于只使用了一阶导数信息，其收敛速度可能较慢，尤其是在接近极值点时。
>



代码：

```c++
#include <cstdlib>
#include <iomanip>
#include <ios>
#include <iostream>
inline long double f(long double x)
{
    return x * x * x - 5 * x * x + 10 * x - 80;
}
inline long double diff_f(long double x)
{
    return 3 * x * x - 10 * x + 10;
}
int main()
{
    long double x = 0, y;
    y = f(x);
    while (std::abs(y) >= 1e-9)
    {
        x -= y / diff_f(x);
        y = f(x);
    }
    std::cout << std::fixed << std::setprecision(9) << x << std::endl;
    return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment2.assets/屏幕截图 2025-03-01 192952.png)



### 06640: 倒排索引

data structures, http://cs101.openjudge.cn/practice/06640/



思路：

用字典与集合联合存储即可。由于不知道Python中是否存在类似于C++中的set的有序集合，最后将set转成了链表再排序输出。

代码：

```python
def main():
    n = int(input())
    documents = [set() for _ in range(n)]
    for i in range(n):
        temp = input().split()
        for j in range(1, len(temp)):
            documents[i].add(temp[j])
    m = int(input())
    for _ in range(m):
        temp = input()
        result = []
        for i in range(n):
            if temp in documents[i]:
                result.append(str(i + 1))
        print(" ".join(result) if len(result) != 0 else "NOT FOUND")


if __name__ == "__main__":
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment2.assets/屏幕截图 2025-03-01 193114.png)



### 04093: 倒排索引查询

data structures, http://cs101.openjudge.cn/practice/04093/



思路：

根据每次请求逐文档检查即可。

代码：

```python
def main():
    n = int(input())
    documents = set()
    table = [set() for _ in range(n)]
    for i in range(n):
        temp = input().split()
        for j in range(1, len(temp)):
            table[i].add(int(temp[j]))
            documents.add(int(temp[j]))
    documents = list(documents)
    documents.sort()
    m = int(input())
    for _ in range(m):
        request = tuple(map(int, input().split()))
        ans = []
        for document in documents:
            flag = True
            for i in range(n):
                if request[i] == -1:
                    if document in table[i]:
                        flag = False
                        break
                elif request[i] == 1:
                    if document not in table[i]:
                        flag = False
                        break
            if flag:
                ans.append(str(document))
        print(" ".join(ans) if ans else "NOT FOUND")


if __name__ == "__main__":
    main()
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](./assignment2.assets/屏幕截图 2025-03-01 193627.png)



### Q6. Neural Network实现鸢尾花卉数据分类

在http://clab.pku.edu.cn 云端虚拟机，用Neural Network实现鸢尾花卉数据分类。

参考链接，https://github.com/GMyhf/2025spring-cs201/blob/main/LLM/iris_neural_network.md

Ubuntu环境下貌似不允许使用pip安装Python包，但是使用apt又找不到PyTorch，不知有何解决方案。



## 2. 学习总结和个人收获

神经网络相关的内容很有趣，目前正在阅读相关书籍。有的





