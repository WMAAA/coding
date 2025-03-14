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
   - 暴力：两个for循环，外层i从0，`内层`j从i，判断条件：求和大于目标，判断是否更新最小长度。
   - 

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

1. 基础

2. 递归遍历

3. 统一迭代法

4. 层序遍历

5. 翻转二叉树

6. 二叉树周末总结

7. 对称二叉树

8. 二叉树的最大深度

9. 二叉树的最小深度

10. 完全二叉树的节点个数

11. 平衡二叉树

    > 求深度适合用前序遍历，求高度适合用后序遍历

12. 二叉树的所有路径

    > - 使用`vector`结构来记录路径，方便来做回溯，再将`vector`结构的`path`转换为`string`格式
    > - 回溯要和递归永远在一起，世界上最遥远的距离是你在花括号里，而我在花括号外

13. 二叉树周末总结

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

1. 动规是由前一个状态推导出来的，而贪心是局部直接选最优的，对于刷题来说就够用了。

   > 1. 确定dp数组（dp table）以及下标的含义
   > 2. 确定递推公式
   > 3. dp数组如何初始化
   > 4. 确定遍历顺序
   > 5. 举例推导dp数组

2. 斐波那契数：人都把上面5个条件都写出来了，你直接按着写就行了，这还不行？

3. 爬楼梯： 你站到倒数一阶，只有一种走法，你站在倒数第二阶，除了上面那种走法，你还有另外一个选择，那就是直接越过。

   - `扩展没看`

4. 使用最小花费爬楼梯

5. 动归周总结

6. 背包理论基础一

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

7. 背包理论基础二

   - 

8. 分割等和子集

9. 最后一块石头的重量II

10. 动归周总结



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

2. dfs理论基础

3. 所有可达路径

4. bfs理论基础

5. 岛屿数量，dfs

6. 岛屿数量， bfs

7. 岛屿的最大面积

8. 沉没孤岛

9. 水流问题。

   - 暴力解法：遍历每个点，这样，每个点都深搜，然后，判断能不能同时到达左右边界。
   - 逆流而上，	然后记录  `上下` 边界开始`dfs`得到的标记，`左右`边界开始`dfs`得到的标记，然后判断两标记是否能同时访问到。

10. 建造最大工岛

    - 暴力解法：遍历地图尝试将每一个0改为1，然后去搜索地图中的最大岛屿面积
    - 用深度搜索。
      1. 遍历地图，找每个岛屿的面积，并做编号记录，更新每个岛屿的编号，`unordered_map`，`key`是岛屿编号，`value`是岛屿面积
      
         > unordered_map中
         >
         > mark标记时，从2开始记录的，，作为地图的标号。后面根据这个，可以从无序map中取相同类型的数量，还可以从无序set中添加唯一的标识、
      2. 遍历每个海水，，，，向四个方向，，，，
         - 每次遍历海水时，会把`VisitdGrid`清零。
         - 用`unordered_set`存访问过的岛屿的编号

11. 字符串接龙

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

12. 有向图的完全可达性

    > 有向图搜索全路径，只能用dfs或bfs
    >
    > 没有回溯是因为，本题不需要路径，只需要查看能否到达所有节点
    > 如何表示能否到达，用一个visited数组

13. 岛屿的周长

    > 本题都没有用到dfs或bfs，就是一个被海水围住的岛屿，只需要遍历每个位置，
    >
    > 若是岛屿
    >
    > - 四个方向延伸
    > - 判断x,y，海水。
    > - 则周长加一

14. 并查集理论基础

    - 基本功能

      - 将两个元素添加到一个集合中

      - 判断两个元素在不在同一个集合

    - 路径压缩：就是都指向一个根节点

      ```
      int find(int u) {
          return u == father[u] ? u : father[u] = find(father[u]);
      }
      ```

      

15. 寻找存在的路径

16. 冗余连接

17. 冗余连接II
