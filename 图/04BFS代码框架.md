



## BFS广度搜索就是确定起点确定终点   查找两个顶点之间的最值问题  以及一般的抽象问题一步步推导答案的过程

![微信图片_20220608105643](C:\Users\14493\Desktop\img\微信图片_20220608105643.png) 



**while循环就是不断向外扩散的循环   直到找到终点** (不能确定它需要走多少步可以到达终点  因此需要使用while  然后用队列来判断)

**里面第一层for循环是为了是所有边缘顶点一起向外扩散   如果不加for循环的话  那么它是一个一个顶点进行扩散的**  ==>  因此如果需要记录层数等问题就需要该for循环  如果不需要边缘同时向外扩散就不需要该for循环



**里面第二层for循环是因为每一个边缘顶点与它相邻的点都不止一个  即一个边缘顶点它扩散可以有很多子顶点  因此需要for循环**  ==>  如果边缘顶点的子顶点只有一个就不需要该for循环   有多个就需要        二叉树没有这层for循环是因为每一个顶点最多只有两个子顶点 所以手动实现了



## BFS解决树的最小深度

```c++
    int minDepth(TreeNode* root) {
        if(!root) return 0;

        int depth = 0;
        bfs(root, depth);
        return depth;
    }

    void bfs (TreeNode *root, int &depth) {
        queue<TreeNode *> q;
        q.emplace(root);

        while(q.empty() == false) {
            ++depth;
            int n = q.size();
            for(int i = 0; i < n; ++i) {
                TreeNode *cur = q.front(); q.pop();
                if(!cur->left && !cur->right) return;
                if(cur->left != nullptr) q.emplace(cur->left);
                if(cur->right != nullptr) q.emplace(cur->right);
            }
        }
    }
```



## 转盘锁 ==>  确定了起点"0000"   终点target

单向BFS：

```c++
    string UpOne (string cur, int i) {
        if(cur[i] == '9') cur[i] = '0';
        else cur[i] = ((cur[i] - '0') + 1) + '0';

        return cur;
    }

    string DownOne (string cur, int i) {
        if(cur[i] == '0') cur[i] = '9';
        else cur[i] = ((cur[i] - '0') - 1) + '0';

        return cur;
    }

    int openLock(vector<string>& deadends, string target) {
        
        //存储deadends中的字符和已经遍历过的字符
        unordered_set<string> visited;
        for(auto &item : deadends) visited.insert(item);

        if(visited.count("0000")) return -1;

        int step = 0;
        queue<string> q;
        q.emplace("0000");
        visited.insert("0000");
        
        while(q.empty() == false) {
            int n = q.size();
            for(int i = 0; i < n; ++i) {
                string cur = q.front(); q.pop();

                //如果到达终点则结束
                if(cur == target) return step;

                //然后将与它相邻的节点都添加到队列中 由于一共有八种情况 所以for循环
                for(int j = 0; j < 4; ++j) {
                    //将该位向上拨动
                    string up = UpOne(cur, j);
                    //如果是非法密码或者已经走过的 则不加入队列
                    if(!visited.count(up)) {
                        q.emplace(up);
                        visited.insert(up);
                    }
                    //将该位向下拨动
                    string down = DownOne(cur, j);
                    if(!visited.count(down)) {
                        q.emplace(down);
                        visited.insert(down);
                    }
                }
            }
            ++step;
        }

        return -1;
    }
```



双向BFS： 只有在确定了具体的起点和终点时  并且路径是双向的才能使用

```c++
    int openLock(vector<string>& deadends, string target) {
        unordered_set<string> q1;
        unordered_set<string> q2;

        unordered_set<string> visited;
        for(auto &item : deadends) visited.insert(item);
        if(visited.count("0000")) return -1;

        int step = 0;
        q1.insert("0000");
        q2.insert(target);

        while(!q1.empty() && !q2.empty()) {
            //每次都让存有顶点数量较少的那一端进行扩散
            if(q1.size() > q2.size()) {
                swap(q1, q2);
            }
            unordered_set<string> temp;

            for(auto &cur : q1) {
                
                if(q2.count(cur)) return step;
                visited.insert(cur);

                for(int i = 0; i < 4; ++i) {
                    string up = UpOne(cur, i);
                    if(!visited.count(up)) {
                        temp.insert(up);
                    }
                    string down = DownOne(cur, i);
                    if(!visited.count(down)) {
                        temp.insert(down);
                    }
                }
            }
            ++step;

            q1 = q2;
            q2 = temp;
        }

        return -1;
    }
```

![微信图片_20220608131230](C:\Users\14493\Desktop\img\微信图片_20220608131230.png) 

因为从起点开始进行扩散    队列中包含的顶点数目一般都会越来越多(并且其中无用的顶点也很多)    所以随着扩散的进行 它的耗时就会越来越大 

因此可以从起点和终点两端进行扩散    如果碰到了交集的顶点   就说明起点与终点之间有路径     这样可以在一定程度上减少无效的时间消耗(即那些无法到达终点的顶点)      

还有一个小优化：每次要进行扩散的时候      判断起点维护顶点数与终点维护的顶点数哪边少就扩散哪边   



**这题不能够使用DFS 因为需要最小步数**





## 数字华容道   由于它是求起点的棋盘走到终点棋盘的最小步数  所以可以使用BFS搜索

将棋盘抽象成字符串   每次都让空出来的位置与周围的棋子进行交换   ==》  因此要找到每一个字符串中的 0   (遍历一遍即可)      还要找到与0相邻的那些棋子(由于该题目仅仅只是 2 × 3 的棋盘  所以可以事先将索引对应相邻的关系保存起来)

```c++
    int slidingPuzzle(vector<vector<int>>& board) {
        
        vector<vector<int>> neighbor = {
            {1, 3},
            {0, 2, 4},
            {1, 5},
            {0, 4},
            {1, 3, 5},
            {2, 4}
        };

        string start;
        for(int i = 0; i < 2; ++i) {
            for(int j = 0; j < 3; ++j) {
                start.push_back((board[i][j] + '0'));
            }
        }

        string target = "123450";
        unordered_set<string> visited; visited.insert(start);

        queue<string> q;
        q.emplace(start);
        int step = 0;
        while(q.empty() == false) {
            int n = q.size();
            for(int i = 0; i < n; ++i) {
                string cur = q.front(); q.pop();

                if(cur == target) return step;

                //找到0所在的索引
                int idx = 0;
                for(; cur[idx] != '0'; ++idx);

                //然后将该位置能够移动的所有可能的情况加入队列
                for(int item : neighbor[idx]) {
                    //与0交换位置
                    string newBoard = swapChar(item, idx, cur);
                    //判断是否已经遍历过
                    if(!visited.count(newBoard)) {
                        q.emplace(newBoard);
                        visited.insert(newBoard);
                    }
                }
            }
            ++step;
        }

        return -1;
    }

    string swapChar (int index1, int index2, string cur) {
        char temp = cur[index1];
        cur[index1] = cur[index2];
        cur[index2] = temp;

        return cur;
    }
```

