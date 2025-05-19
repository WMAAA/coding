# 万能输入

无符号

```
    vector<vector<int>> arr;
    string input;
    while(getline(cin, input)){
        if(input.size() > 0){
            stringstream ss(input);
            int num;
            vector<int> a;
            while(ss >> num){
                a.push_back(num);
            }
            arr.push_back(a);
        }
    }

或者
    
int n,m;
cin>>n>>m;
vector<vector<int>> grid(n, vector<int>(m,0));
for(int i=0; i<n; i++){
    for(int j=0;j<m;j++){
        cin>>grid[i][j];
    }
}

邻接表的输入
    int n, m, s, t;
    cin >> n >> m;

    vector<list<int>> graph(n + 1);
    while (m--) {
        cin >> s >> t;
        graph[s].push_back(t);

    }
```

有符号

```
    vector<vector<int>> res;
    string input;
    char *tok;
    while(getline(cin, input)){
        if(input.size() > 0){
            vector<int> a;
            tok = strtok((char*)input.c_str(), " ,[]");
            while(tok != NULL){
                a.push_back(stoi(tok));
                tok = strtok(NULL, " ,[]"); // 继续分割上一次的字符
            }
            res.push_back(a);
        }
    }
```

# coding

> 只说思路，不进行编码

# 数组

1. 基础

2. 二分查找：注意区间，我就写成左闭右闭，防止溢出，mid形式 `int middle = left + ((right - left) / 2);`，更新左右时，**注意区间**

3. 移除元素：双指针法，直接返回`slowIndex`

4. 有序数组的平方：
   - 暴力解法：直接每个元素平方后，`sort`排序，返回A
   - 双指针法：for循环不进行更新操作，在里面判断，k一直减减，根据情况判断是更新i还是更新j
   
5. 长度最小的子数组：
   - 暴力：两个for循环，外层i从0，`内层`j从i，判断条件：求和大于目标，判断是否更新最小长度。**若条件成立的话，就直接 `break` 内层for、**
   
     ```
     return result == INT32_MAX ? 0 : result;
     ```
   
   - 滑动窗口法
   
     - 窗口的起始位置如何移动：如果当前窗口的值大于等于s了，窗口就要向前移动了（也就是该缩小了）。
   
     - 窗口的结束位置如何移动：窗口的结束位置就是遍历数组的指针，也就是for循环里的索引。
   
     - 解题的关键在于 窗口的起始位置如何移动
   
       > 注意看while里的条件，和，while内的`sum-=nums[i++]`
   
   - ```
     class Solution{
     public:
         int minSubArrayLen(int s, vector<int>& nums){
             int result = INT32_MAX;
             int sum = 0;
             int subLength = 0;
             int i =0;
             for(int j=0; j<nums.size(); j++){
                 sum+=nums[j];
                 while(sum >= s){
                     subLength = j-i+1;
     
                     result = subLength < result ? subLength : result;
         
                     sum -= nums[i];
                     i++;
                 }
             }
             return result == INT32_MAX ? 0:result;
         }
     };
     ```
   

## 区间和

暴力解法，先往`vector<int>`中塞值，然后`while(cin>>a>>b)`，遍历求和即可，但是可能超时

**前缀和 在涉及计算区间和的问题时非常有用**！

C++ 代码 面对大量数据 读取 输出操作，最好用scanf 和 printf，耗时会小很多

`while (~scanf("%d%d", &a, &b))`

当 `scanf` 返回值不等于 `EOF`（即 `-1`）时，循环继续，更直观的写法

```
while (scanf("%d%d", &a, &b) != EOF)
```

`printf("%d\n", sum);`

# 栈与队列

1. 基础

2. 用栈实现队列

3. 用队列实现栈

4. 有效的括号：分三种情况，左边多，类型不对称，右边多

   - 判断类型，st进相反方向的括号。

5. 删除字符串中的所有相邻重复项。

   - 就是栈，一样的`pop`，不一样就`push`，最后从栈中取出，翻转一下。

   > 栈的目的就是存放遍历过的元素，当遍历当前元素时，去栈里看一下我们是不是遍历过相同数值的相邻元素。

6. 逆波兰表达式求值：是一种后缀表达式，就是指运算符写在后面。

   - 其实就是遇到数字就push，遇到运算符就取出，计算完后再push进去。
   - 最后，取出栈中最后一个元素，返回。

7. 滑动窗口最大值：**就是实现一个单调队列**

   - pop：
   - push


# 二叉树

## 基础

二叉树的定义

```
struct TreeNode
{
	int val;
	TreeNode *left;
	TreeNode *right;
	TreeNode(int x) : val(x), left(NULL), right(NULL);
};
```

## 递归遍历

```
class Solution {
public:
    void traversal(TreeNode* cur, vector<int>& vec) {
        if (cur == NULL) return;
        vec.push_back(cur->val);    // 中
        traversal(cur->left, vec);  // 左
        traversal(cur->right, vec); // 右
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```

## 统一迭代法

**处理的节点放入栈之后，紧接着放入一个空指针作为标记**

注意出栈的顺序，比如前序遍历，就是判断

```
			if(node != NULL){
				st.pop();
				if(node->right) st.push(node->right);
				if(node->left) st.push(node->left);
				st.push(node);
				st.push(NULL);
```

## 层序遍历

```
vector<vector<int>> levelOrder(TreeNode* root){
	queue<TreeNode*> que;
	if(root != NULL) que.push(root);
	vector<vector<int>> result;
	while(!que.empty()){
		int size = que.size();
		vector<int> vec;
		for(int i = 0; i<size; i++){
			TreeNode* node = que.front();
			que.pop();
			vec.push_back(node->val);
			if(node->left) que.push(node->left);
			if(node->right) que.push(node->right);
		}
		result.push_back(vec);
	}
	return result;
}
```





1. 翻转二叉树

2. 二叉树周末总结

## 对称二叉树

> 注意，传入参数或者存入队列的节点的顺序。

递归

队列

栈

## 二叉树的最大深度

**递归法，后序遍历**

```
class Solution {
public:
    int getdepth(TreeNode* node){
        if(node == NULL) return 0;
        int leftdepth = getdepth(node->left);
        int rightdepth = getdepth(node->right);
        int depth = 1+max(leftdepth, rightdepth);
        return depth;
    }
    int maxDepth(TreeNode* root) {
        return getdepth(root);
    }
};
```

**迭代法**

```
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == NULL) return 0;
        int depth = 0;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()) {
            int size = que.size();
            depth++; // 记录深度
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
        }
        return depth;
    }
};
```

n叉树的

```
            for (int i = 0; i < size; i++) {
                Node* node = que.front();
                que.pop();
                for (int j = 0; j < node->children.size(); j++) {
                    if (node->children[j]) que.push(node->children[j]);
                }
            }
```











1. 二叉树的最小深度

2. 完全二叉树的节点个数

3. 平衡二叉树

   > 求深度适合用前序遍历，求高度适合用后序遍历

4. 二叉树的所有路径

   > - 使用`vector`结构来记录路径，方便来做回溯，再将`vector`结构的`path`转换为`string`格式
   > - 回溯要和递归永远在一起，世界上最遥远的距离是你在花括号里，而我在花括号外

5. 二叉树周末总结

# 回溯算法

1. 基础

2. 组合问题

3. 组合（优化）

4. 组合总和III

   - 确定递归函数参数
   - 终止条件
   - 处理过程和回溯过程，**回溯要减掉自身，`pop`掉这个元素，然后此循环结束，i+1到下一个循环**

5. 电话号码的字母组合

   - 需要注意的是，我们需要把输入的数字string，转换成字符串。

     ```c++
     const string letterMap[10] = {. . . . . . . . .}
     ```

6. 回溯周末总结

7. 组合总和

   > 如果剪枝优化，需要先排序，然后再for循环中加入直接跳出的判断条件

8. 组合综合III

9. 分割回文串

10. 复原IP地址

11. 子集问题

12. 回溯周总结

# **贪心算法**

1. 理论基础：常识性推导加举反例。
2. 分发饼干：饼干小孩排序。一层遍历加判断，能吃饱，好，那就加一 。`从大到小遍历`饼干。
3. 摆动序列：将数字写成山坡，把上山下山遇到的，全扔掉，那么我们记录的就是山峰。
   - 单调中间有平坡，上下中间有平坡
   - `动态规划还没看`
4. 最大子序列和
   - 暴力解法
   - 贪心算法，只有大于result时，才更新，否则一直加，小于等于0时就置为0
     - 这里是因为一直记录着最大值，所以一旦小于等于0，前面的那一部分就没有收益了。

5. 贪心周总结
6. 

****

# **动态规划**

> 法典
>
> 1. 确定dp数组（dp table）以及下标的含义
> 2. 确定递推公式
> 3. dp数组如何初始化
> 4. 确定遍历顺序
> 5. 举例推导dp数组

## 斐波那契数

[力扣题目链接(opens new window)](https://leetcode.cn/problems/fibonacci-number/)

斐波那契数，通常用 F(n) 表示，形成的序列称为 斐波那契数列 。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是： F(0) = 0，F(1) = 1 F(n) = F(n - 1) + F(n - 2)，其中 n > 1 给你n ，请计算 F(n) 。

```cpp
class Solution {
public:
    int fib(int N) {
        if (N <= 1) return N;
        vector<int> dp(N + 1);
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= N; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[N];
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(n)

当然可以发现，我们只需要维护两个数值就可以了，不需要记录整个序列。

代码如下：

```cpp
class Solution {
public:
    int fib(int N) {
        if (N <= 1) return N;
        int dp[2];
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= N; i++) {
            int sum = dp[0] + dp[1];
            dp[0] = dp[1];
            dp[1] = sum;
        }
        return dp[1];
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(1)

## 爬楼梯

[力扣题目链接(opens new window)](https://leetcode.cn/problems/climbing-stairs/)

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1：

- 输入： 2
- 输出： 2
- 解释： 有两种方法可以爬到楼顶。
  - 1 阶 + 1 阶
  - 2 阶

示例 2：

- 输入： 3
- 输出： 3
- 解释： 有三种方法可以爬到楼顶。
  - 1 阶 + 1 阶 + 1 阶
  - 1 阶 + 2 阶
  - 2 阶 + 1 阶

```
// 版本一
class Solution {
public:
    int climbStairs(int n) {
        if (n <= 1) return n; // 因为下面直接对dp[2]操作了，防止空指针
        vector<int> dp(n + 1);
        dp[1] = 1;
        dp[2] = 2;
        for (int i = 3; i <= n; i++) { // 注意i是从3开始的
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};
```

时间复杂度：O(n)
空间复杂度：O(n)

## 使用最小花费爬楼梯

给你一个整数数组 cost ，其中 cost[i] 是从楼梯第 i 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

你可以选择从下标为 0 或下标为 1 的台阶开始爬楼梯。

请你计算并返回达到楼梯顶部的最低花费。



```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        vector<int> dp(cost.size() + 1);
        dp[0] = 0; // 默认第一步都是不花费体力的
        dp[1] = 0;
        for (int i = 2; i <= cost.size(); i++) {
            dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }
        return dp[cost.size()];
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(n)

还可以优化空间复杂度，因为dp[i]就是由前两位推出来的，那么也不用dp数组了，C++代码如下：

```cpp
// 版本二
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int dp0 = 0;
        int dp1 = 0;
        for (int i = 2; i <= cost.size(); i++) {
            int dpi = min(dp1 + cost[i - 1], dp0 + cost[i - 2]);
            dp0 = dp1; // 记录一下前两位
            dp1 = dpi;
        }
        return dp1;
    }
};
```

- 时间复杂度：O(n)
- 空间复杂度：O(1)





1. 动归周总结

2. 背包理论基础一

   > **`dp[i][j]`表示从下表为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少。**

   - 其实理解以后，就这几个步骤
     - 数组下标代表啥
     - 递推公式咋整
     - dp数组初始化
     - 遍历顺序
     - 举例推导dp
   - 这道题，遍历背包重量，比较背包和物品谁重量大
     - 物品大，就用同一列上方元素进行赋值，就是同重量，装轻的
     - 背包大，就比较只装轻的，或找那个剩余容量对应的上一行的`value`,求`max`

3. 背包理论基础二

   - 

4. 分割等和子集

5. 最后一块石头的重量II

6. 动归周总结



# **单调栈**

1. 每日温度：要观测到更高气温要等待的天数

   - 两层`for`循环多简单呀，自讨苦吃

   - 单调栈的本质是 `空间换时间`，直接在栈中放元素索引。

     > 三个判断条件
     >
     > - 当前遍历的元素T[i]小于栈顶元素T[st.top()]的情况
     > - 当前遍历的元素T[i]等于栈顶元素T[st.top()]的情况
     > - 当前遍历的元素T[i]大于栈顶元素T[st.top()]的情况
     >
     > 前两个都是直接往里面塞，最后大于的时候才更新栈顶的索引对应的result处的值。**弹出的都知道距离下一个是多远了**

# 图论

1. 理论基础
   - 邻接表，从边的角度考虑。有多少边申请对应大小的链表。
   - 邻接矩阵，n个节点，n*n，矩阵的元素是权重。**缺点是稀疏图**
   - 图的遍历方式
     - 深度优先搜索dfs
     - 广度优先搜索bfs


## dfs理论基础

dfs代码框架

```
void dfs(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本节点所连接的其他节点) {
        处理节点;
        dfs(图，选择的节点); // 递归
        回溯，撤销处理结果
    }
}
```

注意处理完后的撤销。

## 所有可达路径

【题目描述】

给定一个有 n 个节点的有向无环图，节点编号从 1 到 n。请编写一个程序，找出并返回所有从节点 1 到节点 n 的路径。每条路径应以节点编号的列表形式表示。

【输入描述】

第一行包含两个整数 N，M，表示图中拥有 N 个节点，M 条边

后续 M 行，每行包含两个整数 s 和 t，表示图中的 s 节点与 t 节点中有一条路径

【输出描述】

输出所有的可达路径，路径中所有节点的后面跟一个空格，每条路径独占一行，存在多条路径，路径输出的顺序可任意。

如果不存在任何一条路径，则输出 -1。

注意输出的序列中，最后一个节点后面没有空格！ 例如正确的答案是 `1 3 5`,而不是 `1 3 5`， 5后面没有空格！

## bfs理论基础

```
int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 表示四个方向
// grid 是地图，也就是一个二维数组
// visited标记访问过的节点，不要重复访问
// x,y 表示开始搜索节点的下标
void bfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int x, int y) {
    queue<pair<int, int>> que; // 定义队列
    que.push({x, y}); // 起始节点加入队列
    visited[x][y] = true; // 只要加入队列，立刻标记为访问过的节点
    while(!que.empty()) { // 开始遍历队列里的元素
        pair<int ,int> cur = que.front(); que.pop(); // 从队列取元素
        int curx = cur.first;
        int cury = cur.second; // 当前节点坐标
        for (int i = 0; i < 4; i++) { // 开始想当前节点的四个方向左右上下去遍历
            int nextx = curx + dir[i][0];
            int nexty = cury + dir[i][1]; // 获取周边四个方向的坐标
            if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;  // 坐标越界了，直接跳过
            if (!visited[nextx][nexty]) { // 如果节点没被访问过
                que.push({nextx, nexty});  // 队列添加该节点为下一轮要遍历的节点
                visited[nextx][nexty] = true; // 只要加入队列立刻标记，避免重复访问
            }
        }
    }

}
```

## 岛屿数量

题目描述：

给定一个由 1（陆地）和 0（水）组成的矩阵，你需要计算岛屿的数量。岛屿由水平方向或垂直方向上相邻的陆地连接而成，并且四周都是水域。你可以假设矩阵外均被水包围。

输入描述：

第一行包含两个整数 N, M，表示矩阵的行数和列数。

后续 N 行，每行包含 M 个数字，数字为 1 或者 0。

输出描述：

输出一个整数，表示岛屿的数量。如果不存在岛屿，则输出 0。

### dfs

```
// 版本一 
#include <iostream>
#include <vector>
using namespace std;

int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 四个方向
void dfs(const vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y) {
    for (int i = 0; i < 4; i++) {
        int nextx = x + dir[i][0];
        int nexty = y + dir[i][1];
        if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;  // 越界了，直接跳过
        if (!visited[nextx][nexty] && grid[nextx][nexty] == 1) { // 没有访问过的 同时 是陆地的

            visited[nextx][nexty] = true;
            dfs(grid, visited, nextx, nexty);
        }
    }
}

int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }

    vector<vector<bool>> visited(n, vector<bool>(m, false));

    int result = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (!visited[i][j] && grid[i][j] == 1) {
                visited[i][j] = true;
                result++; // 遇到没访问过的陆地，+1
                dfs(grid, visited, i, j); // 将与其链接的陆地都标记上 true
            }
        }
    }

    cout << result << endl;
}
```

### bfs

```
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 四个方向
void bfs(const vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y) {
    queue<pair<int, int>> que;
    que.push({x, y});
    visited[x][y] = true; // 只要加入队列，立刻标记
    while(!que.empty()) {
        pair<int ,int> cur = que.front(); que.pop();
        int curx = cur.first;
        int cury = cur.second;
        for (int i = 0; i < 4; i++) {
            int nextx = curx + dir[i][0];
            int nexty = cury + dir[i][1];
            if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;  // 越界了，直接跳过
            if (!visited[nextx][nexty] && grid[nextx][nexty] == 1) {
                que.push({nextx, nexty});
                visited[nextx][nexty] = true; // 只要加入队列立刻标记
            }
        }
    }
}

int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }

    vector<vector<bool>> visited(n, vector<bool>(m, false));

    int result = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (!visited[i][j] && grid[i][j] == 1) {
                result++; // 遇到没访问过的陆地，+1
                bfs(grid, visited, i, j); // 将与其链接的陆地都标记上 true
            }
        }
    }


    cout << result << endl;
}
```

## 孤岛的总面积

题目描述

给定一个由 1（陆地）和 0（水）组成的矩阵，岛屿指的是由水平或垂直方向上相邻的陆地单元格组成的区域，且完全被水域单元格包围。孤岛是那些位于矩阵内部、所有单元格都不接触边缘的岛屿。

现在你需要计算所有孤岛的总面积，岛屿面积的计算方式为组成岛屿的陆地的总数。

输入描述

第一行包含两个整数 N, M，表示矩阵的行数和列数。之后 N 行，每行包含 M 个数字，数字为 1 或者 0。

输出描述

输出一个整数，表示所有孤岛的总面积，如果不存在孤岛，则输出 0。

### dfs

```
#include <iostream>
#include <vector>
using namespace std;
int dir[4][2] = {-1, 0, 0, -1, 1, 0, 0, 1}; // 保存四个方向
void dfs(vector<vector<int>>& grid, int x, int y) {
    grid[x][y] = 0;
    for (int i = 0; i < 4; i++) { // 向四个方向遍历
        int nextx = x + dir[i][0];
        int nexty = y + dir[i][1];
        // 超过边界
        if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
        // 不符合条件，不继续遍历
        if (grid[nextx][nexty] == 0) continue;

        dfs (grid, nextx, nexty);
    }
    return;
}

int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }

    // 从左侧边，和右侧边 向中间遍历
    for (int i = 0; i < n; i++) {
        if (grid[i][0] == 1) dfs(grid, i, 0);
        if (grid[i][m - 1] == 1) dfs(grid, i, m - 1);
    }
    // 从上边和下边 向中间遍历
    for (int j = 0; j < m; j++) {
        if (grid[0][j] == 1) dfs(grid, 0, j);
        if (grid[n - 1][j] == 1) dfs(grid, n - 1, j);
    }
    int count = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) count++;
        }
    }
    cout << count << endl;
}
```

### bfs

```
#include <iostream>
#include <vector>
#include <queue>
using namespace std;
int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1}; // 四个方向
void bfs(vector<vector<int>>& grid, int x, int y) {
    queue<pair<int, int>> que;
    que.push({x, y});
    grid[x][y] = 0; // 只要加入队列，立刻标记
    while(!que.empty()) {
        pair<int ,int> cur = que.front(); que.pop();
        int curx = cur.first;
        int cury = cur.second;
        for (int i = 0; i < 4; i++) {
            int nextx = curx + dir[i][0];
            int nexty = cury + dir[i][1];
            if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;  // 越界了，直接跳过
            if (grid[nextx][nexty] == 1) {
                que.push({nextx, nexty});
                grid[nextx][nexty] = 0; // 只要加入队列立刻标记
            }
        }
    }
}

int main() {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> grid(n, vector<int>(m, 0));
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }
    // 从左侧边，和右侧边 向中间遍历
    for (int i = 0; i < n; i++) {
        if (grid[i][0] == 1) bfs(grid, i, 0);
        if (grid[i][m - 1] == 1) bfs(grid, i, m - 1);
    }
    // 从上边和下边 向中间遍历
    for (int j = 0; j < m; j++) {
        if (grid[0][j] == 1) bfs(grid, 0, j);
        if (grid[n - 1][j] == 1) bfs(grid, n - 1, j);
    }
    int count = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) count++;
        }
    }

    cout << count << endl;
}
```

1. 

2. 

3. 

4. 

5. 沉没孤岛

6. 水流问题。

   - 暴力解法：遍历每个点，这样，每个点都深搜，然后，判断能不能同时到达左右边界。
   - 逆流而上，	然后记录  `上下` 边界开始`dfs`得到的标记，`左右`边界开始`dfs`得到的标记，然后判断两标记是否能同时访问到。

7. 建造最大工岛

   - 暴力解法：遍历地图尝试将每一个0改为1，然后去搜索地图中的最大岛屿面积
   - 用深度搜索。
     1. 遍历地图，找每个岛屿的面积，并做编号记录，更新每个岛屿的编号，`unordered_map`，`key`是岛屿编号，`value`是岛屿面积
     
        > unordered_map中
        >
        > mark标记时，从2开始记录的，，作为地图的标号。后面根据这个，可以从无序map中取相同类型的数量，还可以从无序set中添加唯一的标识、
     2. 遍历每个海水，，，，向四个方向，，，，
        - 每次遍历海水时，会把`VisitdGrid`清零。
        - 用`unordered_set`存访问过的岛屿的编号

8. 字符串接龙

   - 无向图求最短路径，用BFS最合适，找的的直接是最短路径

   - 需要注意的点

     - 无向图，需要用标记位，标记着节点是否走过，否则死循环

     - 使用set来检查字符串是否出现在字符串集合里更快一些

   - 思路

     1. 只要队列不为空，就弹出第一个元素，然后

     2. for循环，挨个替换每个字母，`newWord[i] = j + 'a';`

        - if找到endStr，return

        - if符串集合里出现了newWord，并且newWord没有被访问过

        - > 因为，只是newWord的话，不记录进visitMap中。

9. 有向图的完全可达性

   > 有向图搜索全路径，只能用dfs或bfs
   >
   > 没有回溯是因为，本题不需要路径，只需要查看能否到达所有节点
   > 如何表示能否到达，用一个visited数组

10. 岛屿的周长

    > 本题都没有用到dfs或bfs，就是一个被海水围住的岛屿，只需要遍历每个位置，
    >
    > 若是岛屿
    >
    > - 四个方向延伸
    > - 判断x,y，海水。
    > - 则周长加一

11. 并查集理论基础

    - 基本功能

      - 将两个元素添加到一个集合中

      - 判断两个元素在不在同一个集合

    - 路径压缩：就是都指向一个根节点

      ```
      int find(int u) {
          return u == father[u] ? u : father[u] = find(father[u]);
      }
      ```

      

12. 寻找存在的路径

    > 并查集的三个基本功能：
    >
    > - 寻找根节点，函数：find(int u)，也就是判断这个节点的祖先节点是哪个
    > - 将两个节点接入到同一个集合，函数：join(int u, int v)，将两个节点连在同一个根节点上
    > - 判断两个节点是否在同一个集合，函数：isSame(int u, int v)，就是判断两个节点是不是同一个根节点

    主要是路径压缩步骤，判断是否没有father了(所以这里初始化时是初始化为 father[i] = i)

13. 冗余连接

    > 跟基础并查集思路一样，判断有没有同环，没有的话就join
    > 因为是从前向后遍历，题目是加入一条边变成环，所以这样遍历到 最后就是冗余

14. 冗余连接二

    > 有向图变有向树，只有一个根节点，只删除一个边，删最后的那个边

    - 记录节点的入度

      ```
          int s, t;
          vector<vector<int>> edges;
          cin >> n;
          vector<int> inDegree(n + 1, 0); // 记录节点入度
          for (int i = 0; i < n; i++) {
              cin >> s >> t;
              inDegree[t]++;			// 主要是这一行，t不相同时，加的也不同
              edges.push_back({s, t});// 将每个边加入
          }
      ```

    - 好好想想以下两种情况的不同

      - `isTreeAfterRemoveEdge()` 判断删一个边之后是不是有向树
      - `getRemoveEdge()`确定图中一定有了有向环，那么要找到需要删除的那条边

    - 先执行入度为2的两种情况，然后顺序这运行`getRemoveEdge()`，将边的两端点加入并查集，如果已有，则说明出现了环。


## 最小生成树prim

> prim三部曲
>
> - 第一步，选距离生成树最近节点
> - 第二步，最近节点加入生成树
> - 第三步，更新非生成树节点到生成树的距离（即更新minDist数组）
>
> **minDist数组用来记录每一个节点距离最小生成树的最近距离**

1. 初始化`minDist`数组为最大值，这样以后更小就更新

   ```
   vector<int> minDist(v + 1, 10001);
   ```

2. 初始化双向图

   ```
       vector<vector<int>> grid(v + 1, vector<int>(v + 1, 10001));
       while (e--) {
           cin >> x >> y >> k;
           // 因为是双向图，所以两个方向都要填上
           grid[x][y] = k;
           grid[y][x] = k;
   
       }
   ```

3. 循环n-1次，因为只需要构建n-1个边就行啦

   - 每次开头吧`minVal=INT_MAX`，为了比较与他相连的最小的，选择没在树中，距离最小的。

## dijkstra朴素版

最短路是图论中的经典问题即：给出一个有向图，一个起点，一个终点，问起点到终点的最短路径。

三部曲

1. 第一步，选源点到哪个节点近且该节点未被访问过
2. 第二步，该最近节点被标记访问过
3. 第三步，更新非访问节点到源点的距离（即更新minDist数组）

**minDist数组 用来记录 每一个节点距离源点的最小距离**。

## dijkstra堆优化版

邻接表用 数组+链表 来表示

在`vector<list<int>> grid(n + 1);` 中 就不能使用int了，而是需要一个键值对 来存两个数字，一个数表示节点，一个数表示 指向该节点的这条边的权值。

```
vector<list<pair<int,int>>> grid(n + 1);
```

但是代码可读性差，所以定义一个类取代`pair<int, int>`

要选择距离源点近的节点（即：该边的权值最小），所以 我们需要一个 小顶堆 来帮我们对边的权值排序，**每次从小顶堆堆顶 取边就是权值最小的边**。

```
// 小顶堆
class mycomparison {
public:
    bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs) {
        return lhs.second > rhs.second;
    }
};
// 优先队列中存放 pair<节点编号，源点到该节点的权值> 
priority_queue<pair<int, int>, vector<pair<int, int>>, mycomparison> pq;
```

# Floyd算法

经典的多源最短路问题

**Floyd 算法对边的权值正负没有要求，都可以处理**。

Floyd算法核心思想是动态规划

**动规五部曲：**

- 确定dp数组（dp table）以及下标的含义

  - grid[i] [j] [k] = m，表示 **节点i 到 节点j 以[1...k] 集合中的一个节点为中间节点的最短距离为m**

- 确定递推公式

  ```
  grid[i][j][k] = min(grid[i][k][k - 1] + grid[k][j][k - 1]， grid[i][j][k - 1])
  ```

- dp数组如何初始化

  - 三维数组初始化

- 确定遍历顺序

  - 想象三维数组遍历顺序，可以得到谁在最外层

- 举例推导dp数组
