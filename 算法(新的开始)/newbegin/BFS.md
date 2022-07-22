### BFS:

> 1. BFS 算法组成的 3 元素：队列，入队出队的节点，已访问的集合。
>
> - 队列：先入先出的容器；
> - 节点：最好写成单独的类，比如本例写成 (value,step) 元组。也可写成 (value,visited)，看自己喜好和题目；
> - 已访问集合：为了避免队列中插入重复的值
>
> 1. BFS算法组成的套路：
> 2. 初始化三元素：
>        `Node = node(n)`   `queue = [Node] visited = set([Node.value])`
> 3. 操作队列 —— 弹出队首节点：
>        `vertex = queue.pop(0)`
> 4. 操作弹出的节点 —— 根据业务生成子节点（一个或多个）：
>        [node(vertex.value - n*n, Node.step+1) for n in     range(1,int(vertex.value**.5)+1)]
> 5. 判断这些节点 ——     符合业务条件，则return，不符合业务条件，且不在已访问集合，则追加到队尾，并加入已访问集合：
>        这篇文章讲的很好，并且给出了具体如何套用BFS的模板。

模板：

```C++
int BFS(Node start, Node target) {
    Queue<Node> q; // 核心数据结构
    Set<Node> visited; // 避免走回头路
    
    q.push(start); // 将起点加入队列.用push也可以把。
    visited.add(start);
    int step = 0; // 记录扩散的步数

    while (!q.isempty()) {
        int sz = q.size();
        /* 将当前队列中的所有节点向四周扩散 */
        for (int i = 0; i < sz; i++) {
            Node cur = q.pop();		//用一个变量存储它。
            /* 划重点：这里判断是否到达终点 */
            if (cur is target)
                return step;
            /* 将 cur 的相邻节点加入队列 */
            for (Node x : cur.adj())
                if (x not in visited) {		//注意！无论dfs还是bfs图搜索的时候都要标记数组
                    q.offer(x);
                    visited.add(x);
                }
        }
        /* 划重点：更新步数在这里 */
        step++;
    }
}
```

