### DFS:

> 整体思路：
>
> **很多同学都被递归绕进去了，其实把递归函数当做普通函数就好了。**
>
> - **重点：我们在调用递归函数的时候，把递归函数当做普通函数（黑箱）来调用，即明白该函数的输入输出是什么，而不用管此函数内部在做什么。**
>
> 1. **递归最基本的是记住递归函数的含义（务必牢记函数定义）**：本题的 longestSubstring(s, k) 函数表示的就是题意，即求一个最长的子字符串的长度，该子字符串中每个字符出现的次数都最少为 k。函数入参 s 是表示源字符串；k 是限制条件，即子字符串中每个字符最少出现的次数；函数**返回结果是满足题意的最长子字符串长度（输出！牢记输入输出是什么）**。
>
> 2. **递归的终止条件**（**能直接写出的最简单 case**）：如果字符串 s 的长度少于 k，那么一定不存在满足题意的子字符串，返回 0；
>
> 3. **调用递归（重点）**：如果一个字符 c 在 s 中出现的次数少于 k 次，那么 s 中所有的包含 c 的子字符串都不能满足题意。所以，应该在 s 的所有不包含 c 的子字符串中继续寻找结果：把 s 按照 c 分割（分割后每个子串都不包含 c），得到很多子字符串 t；下一步要求 t 作为源字符串的时候，它的最长的满足题意的子字符串长度（到现在为止，我们把**大问题分割为了小问题(s → t)**）。此时我们发现，**恰好已经定义了函数 longestSubstring(s, k) 就是来解决这个问题的！**所以**直接**把 **longestSubstring(s, k) 函数拿来用**，于是形成了递归。
>
> 4. **未进入递归时的返回结果**：如果 s 中的每个字符出现的次数都大于 k 次，那么 s 就是我们要求的字符串，直接返回该字符串的长度。
>
> 5. **理解：**总之，通过上面的分析，我们看出了：**我们不是为了递归而递归。而是因为我们把大问题拆解成了小问题**，**恰好有函数可以解决小问题**，所以直接用这个函数。**由于这个函数正好是本身**，**所以我们把此现象叫做递归**。小问题是原因，递归是结果。**而递归函数到底怎么一层层展开与终止的，不要用大脑去想，这是计算机干的事。**我们只用把递归函数当做一个能解决问题的黑箱就够了，**把更多的注意力放在1-拆解子问题、2-递归终止条件、3-递归函数的正确性上来。**

---

1. **递归：因为能解决小问题的这个函数恰好是本身**:比如dfs，可以看作是把搜索图分成了搜索若干子图。最终分解为结点问题

2. 硬式记忆：好像递归总是这种固定格式：**调用自身**+**for循环或若干个调用自身**的函数的**形式**。

3. 或者递归可以这样写：如果我们想在leetcode给的原函数增加参数的话。

```c++
int longestSubstring(string s, int k) {
    int n = s.length();
    return dfs(s, 0, n - 1, k);
}

总结递归模板：
    终止条件。能直接写出的最简单 case  /   以及复合题意得情况
    调用递归。常常用下面的res=max(,)的形式来记录子函数的返回值~!!要用好递归的结果！
    返回结果。区分好每个return为谁服务，如何服务。基本上按普通函数的写法不会错。。不过记得调用递归时，函数的返回值要记录一下，最后一起return。
    比如：res=max(res,longestSubstring(s.substr(start,i-start),k));
```

4. 一定要分清楚主函数和子函数的return的作用~是不是期望的那样！！！这点在写递归的时候尤为重要！
   并且子函数的返回值我们需要合理利用~比如:

   ````c++
   temp=longestNiceSubstring(s.substr(start,i-start));//理解这里的子函数返回值为何要保存！
   或：res=max(res,longestSubstring(s.substr(start,i-start),k));
   ````

---

**启示：**

> 1. 和之前**递归的模板很像**~都有    **终止条件+收敛条件+若干个dfs**（化大问题为小问题）
>
> 2. **传入局部变量真**的是一个好方法，以前总结的，这样就不用判断是否用全局变量了。

````C++
	    /**
	    * dfs 模板.
	    * input 输入数据指针
	    * cur or gap 标记当前位置或距离目标的距离
	    * path 当前路径，也是中间结果
	    * result 存放最终结果
	    * @return 路径长度，如果是求路径本身，则不需要返回长度
	    */
		const vector<vector<int>>ds{{1,0},{0,1},{-1,0},{0,-1}};//放到public的下面。
	    void dfs(type *input, type *path, int cur or gap, type *result) {
	      if (数据非法) return 0; // 终止条件
	      if (cur == input.size( or gap == 0)) { // 收敛条件
	        将 path 放入 result
	      }
	      if (可以剪枝) return;
	      for(...) { // 执行所有可能的扩展动作
	         执行动作，修改 path
	         dfs(input, step + 1 or gap--, result);
	         恢复 path？有时候不需要恢复pa't'y'h
	      }
    	}
````

模板：

```C++
		/**
	    * dfs 模板.
	    * input 输入数据指针
	    * cur or gap 标记当前位置或距离目标的距离
	    * path 当前路径，也是中间结果
	    * result 存放最终结果
	    * @return 路径长度，如果是求路径本身，则不需要返回长度
	    */
	    void dfs(type *input, type *path, int cur or gap, type *result) {
	      if (数据非法) return 0; // 终止条件
	      if (cur == input.size( or gap == 0)) { // 收敛条件
	        将 path 放入 result
	      }
	      if (可以剪枝) return;
	      for(...) { // 执行所有可能的扩展动作
	         执行动作，修改 path
	         dfs(input, step + 1 or gap--, result);
	         恢复 path？
	      }
    }
```



#### DFS例题：

#### [1020. 飞地的数量](https://leetcode-cn.com/problems/number-of-enclaves/)

> 启示：C++里面如何写DFS
>
> 1. vector<vector<int>> dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};   //方向数组放在public的下面。
>
> 2. vector<vector<bool>> (m, vector<bool>(n, false));  //m 行 n 列 的标记数组初始化方法，表明来过。
>    定义成全局变量比较好。但是这个不能在public下面定义，不知道为什么。**因此：用 bool visited[] []**
>    
> 3. 如果是不连续的点。主函数里面还需要一个双重for循环。以确保遍历到所有的点。
>
> 4. 二维数组vector的大小:int n=grid.size(),m=grid[0].size();    
>
> 5. 重要！！！**DFS模板虽然掌握~但很多时候在模板的基础上改变！比如这道题。是通过遍历所有和边相连的陆地，然后ans=陆地总面积-和边相连的陆地！要会变通！**
>
> 6. ```C++
>    很多时候栈溢出都是因为终止条件写错了！导致反复进入函数。比如这里如果漏写了这个：
>    						&& visited[x][y]==false
>        就会导致栈溢出的现象发生~因为可能会反复进入visited=true的点。
>                             
>    if(0<=x && x<n && 0<=y && y<m && visited[x][y]==false && grid[x][y]==1)
>        vistied[x][y]=true;
>    所以有visited最好在开头写上：if(visited)	return ;
>    ```

````c++
class Solution {
public:
    vector<vector<int> > dirs ={{-1,0},{1,0},{0,-1},{0,1}};
    int ans=0;
    //vector<vector<bool> > visited(502,vector<bool>(502,false));  不能再在public下面写。
    bool visited[501][501]={0};
    int numEnclaves(vector<vector<int>>& grid) {
        int n=grid.size(),m=grid[0].size();
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if((i==0 || i==n-1 || j==0 ||j ==m-1) && grid[i][j]==1)//只遍历边界的情况。
                {
                    dfs(grid,n,m,i,j);
                }
            }
        }
        for(int i=0;i<n;i++){
            for(int j=0;j<m;j++){
                if(grid[i][j]==1 && !visited[i][j])   ans+=1;
            }
        }
        return ans;
    }
    void dfs(vector<vector<int> > &grid,int &n,int &m,int x,int y){
        if(0<=x && x<n && 0<=y && y<m && visited[x][y]==false && grid[x][y]==1)//复合题意的情况。
            visited[x][y]=true;
        else return;
        for(auto&d:dirs) dfs(grid,n,m,x+d[0],y+d[1]);
        }
    };
````



> dfs ：
>
> 1. 迷宫题目的模板可以看作是。需要一个`used数组`用来回溯，还有一个相当于方向的数组。如果不用`used`数组一定会陷入死循环呢。
>
> 2. 如果走过了就不需要再走了！这样可以节省时间，而且不标记的话会有死循环！

<img src="C:\Users\QWE\AppData\Roaming\Typora\typora-user-images\image-20220407004746885.png" alt="image-20220407004746885" style="zoom:80%;" />