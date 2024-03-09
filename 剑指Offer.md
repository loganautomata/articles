# 剑指Offer

## 03. 数组中重复的数字

难度: 中等.

### Solution

先是常规解法, 使用hash_map, 已经出现过的数字以hash_map<int, bool>结构存储, 提高查找速度.

```cpp
int Jz03::findRepeatNumber(vector<int> &nums)
{
    unordered_map<int, bool> nums_map; //已经存储在hash表中的数字

    for(vector<int>::iterator it = nums.begin(); it < nums.end(); it++)
    {
        if(nums_map[*it]) return *it; //判断该值是否已在hash_map中
        else nums_map[*it] = true;
    }

    return -1;
}
```

下面应该是这道题的标准答案, 由于原题中限定了数组中的数小于数组长度, 所以可以将(数组下标, 数组中的数)作为hash_map<int, int>的映射, 这样就不用再新建hash表了.

```cpp
int Jz03::findRepeatNumber(vector<int> &nums)
{
    //将数组的下标和数组中的值映射为hash_map中的key和value
    for(vector<int>::iterator it = nums.begin(); it < nums.end(); it++)
    {
        if(*it == nums[*it]) //比较value值是否在hash_map中
        {
            if ((vector<int>::iterator)&nums[*it] == it) continue; //比较对应的key值是否与hash_map中的key相等
            else return (*it); 
        }
        else swap(*it, nums[*it]);
    }

    return -1;
}
```

## 04. 二维数组中的查找

难度: 简单

### solution

常规解法, 根据行和列的递增关系, 不断缩小遍历的行与列的长度.
```cpp
bool Jz04::findNumberIn2DArray(vector<vector<int>> &matrix, int target)
{
    if (matrix.size() == 0) return 0;     //矩阵为空的特殊情况

    int y_length = matrix.size();    //遍历列时最大的长度

    int x_length = matrix[0].size(); //遍历行时最大的长度

    for (int i = 0; i < x_length && i < y_length; i++)
    {
        //行遍历
        for (int j = i; j < x_length; j++)
        {
            if (matrix[i][j] == target) return true;
            else if (matrix[i][j] > target) x_length = j;
        }

        //列遍历
        for (int j = i + 1; j < y_length; j++)
        {
            if (matrix[j][i] == target) return true;
            else if (matrix[j][i] > target) y_length = j;
        }
    }

    return false;
}
```

二叉树遍历解法, 将矩阵向左左旋45度, 看作二叉树, 遍历该二叉树即可求出答案.

```cpp
bool Jz04::findNumberIn2DArray(vector<vector<int>>& matrix, int target)
{
    if(matrix.size() == 0) return false; // 矩阵为空时

    // 从右上角开始判断, 找到目标则返回, 如果大于目标则左移, 小于目标则下移.
    for(int i = 0, j = matrix[0].size() - 1; i < (int)matrix.size() && j > -1;)
    {
        if (matrix[i][j] == target) return true;
        else if (matrix[i][j] > target) j--;
        else i++;
    }

    return false;
}
```

## 05. 替换空格

难度: 简单

### solution

常规解法, 遍历字符串替换其中空格.

```cpp
string Jz05::replaceSpace(string s)
{
    string s_r = "%20";

    for (string::iterator i = s.begin(); i < s.end(); i++)
    {
        if (*i == ' ')
        {
            i = s.erase(i);                          // 删除i所指的元素且i指向改为删除的元素的后一位元素.
            i = s.insert(i, s_r.begin(), s_r.end()); // 在i指向的元素前插入字符串.
        }
    }

    return s;
}
```

## 06. 从尾到头打印链表

难度: 简单

### solution

常规解法, 将链表中所有数据放入动态数组然后反转数组.

```cpp
vector<int> Jz06::reversePrint(ListNode* head)
{
    vector<int> result;

    // 将链表中所有数放入动态数组
    for (ListNode* node = head; node != NULL; node = node->next)
    {
        result.push_back(node->val);
    }
    reverse(result.begin(), result.end()); // 反转动态数组

    return result;
}
```

用迭代的方式逆序将数据放入数组.

```cpp
vector<int> Jz06::reversePrint(ListNode *head)
{
    vector<int> result;

    // 通过迭代将数据以倒序的方式插入数组
    if (!pushVal(head, result))
        return result;
    else
        throw "reverse failed";
}

int Jz06::pushVal(ListNode *node, vector<int>& arr)
{
    if (node == NULL) // 迭代终止
        return 0;
    if (!pushVal(node->next, arr)) // 迭代返回值正常
    {
        arr.push_back(node->val);
        return 0;
    }
    else
        return -1;
}
```

先将链表中的数据放入栈再取出放入数组.

```cpp
vector<int> Jz06::reversePrint(ListNode *head)
{
    vector<int> result;
    stack<int> tmp_s;

    // 将链表入栈
    for (ListNode *node = head; node != NULL; node = node->next)
    {
        tmp_s.push(node->val);
    }

    // 数据出栈置入动态数组
    for (; !tmp_s.empty(); tmp_s.pop())
    {
        result.push_back(tmp_s.top());
    }

    return result;
}
```

##  07. 重建二叉树

难度: 中等.

### solution

常规解法, 递归求根节点的左右子树.

```cpp
TreeNode *Jz07::buildTree(vector<int> &preorder, vector<int> &inorder)
{
    if (preorder.empty())
        return nullptr; // 迭代终止条件

    TreeNode *root = new TreeNode(preorder[0]);
    int left_tree_num = 0;  // 左子树的节点个数
    int right_tree_num = 0; // 右子树的节点个数

    // 遍历中序获得左右子树节点个数
    for (int i = 0; i < inorder.size(); i++)
    {
        if (inorder[i] == preorder[0])
        {
            left_tree_num = i;
            right_tree_num = inorder.size() - i - 1;
            break;
        };
    }

    // 构造左子树
    vector<int> left_pre(preorder.begin() + 1, preorder.begin() + left_tree_num + 1);
    vector<int> left_in(inorder.begin(), inorder.begin() + left_tree_num);
    root->left = buildTree(left_pre, left_in);

    // 构造右子树
    vector<int> right_pre(preorder.begin() + left_tree_num + 1, preorder.begin() + left_tree_num + 1 + right_tree_num);
    vector<int> right_in(inorder.begin() + left_tree_num + 1, inorder.begin() + left_tree_num + 1 + right_tree_num);
    root->right = buildTree(right_pre, right_in);

    return root;
}
```

## 09. 用两个栈实现队列

难度: 简单.

### solution

常规解法, 一个栈顶端为最先放入的数据为出栈, 另一个为最后放入的数据为入栈.

```cpp
Jz09::Jz09()
{
}

void Jz09::appendTail(int value)
{
    in.push(value);
}

int Jz09::deleteHead()
{
    // 判断出栈是否为空, 若为空, 先从入栈放入数据到出栈.
    if (out.empty())
    {
        while (!in.empty())
        {
            out.push(in.top());
            in.pop();
        }
    }

    // 如果两个栈都为空则返回-1, 防止删除出栈顶端数据然后返回该数据.
    if (out.empty())
    {
        return -1;
    }
    else
    {
        int top = out.top();
        out.pop();
        return top;
    }
}
```

## 10-I. 斐波那契数列

难度: 简单.

### solution

常规解法, 动态规划.

```cpp
int Jz10_1::fib(int n)
{
    int dp[n + 1]; // 存储动态规划各状态的数组

    for (int i = 0; i <= n; i++)
    {
        if (i < 2)
            dp[i] = i; // 初始状态的值
        else
            dp[i] = (dp[i - 1] + dp[i - 2]) % 1000000007; // 状态转移方程
    }

    return dp[n];
}
```

## 10-2. 青蛙跳台阶问题

难度: 简单.

### solution

常规解法, 动态规划.

```cpp
int Jz10_2::numWays(int n)
{
    int dp[n + 1];

    for (int i = 0; i <= n; i++)
    {
        if (i < 2)
            dp[i] = 1; // 给动态规划初始状态赋值
        else
            dp[i] = (dp[i - 1] + dp[i - 2]) % 1000000007; // 每级台阶的跳法等于前两阶之和.
    }

    return dp[n];
}
```

## 11. 旋转数组的最小数字

难度: 简单.

### solution

常规解法, 动态规划.

```cpp
int Jz11::minArray(vector<int> &numbers)
{
    int min = numbers[0]; // 最小值, 排除numbers只有一个值时的特殊情况.

    // 使用二分法寻找最小值
    for (vector<int>::iterator left = numbers.begin(), right = numbers.end() - 1, middle; left < right;)
    {
        middle = left + distance(left, right) / 2;
        if (*middle == *right)
            right--;
        else if (*middle > *right)
            left = middle + 1;
        else
            right = middle;

        if (left >= right)
            min = *left;
    }

    return min;
}
```

## 12. 矩阵中的路径

难度: 

## 注意事项

### CPP相关

1. vector用iterator初始化时左闭右开, 右开, 右开.

### 测试样例

1. 题中样例.
2. 空值.
3. 插入, 删除等操作后原迭代器失效.

### Class和Struct的区别

1. C++中class可以作为模板参数而struct不可以.
2. class默认访问权限为private而struct为public.
