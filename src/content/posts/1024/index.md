---
title: '记 2025 SCIE 1024 Coding Challenge'
published: 2025-10-28
updated: 2025-10-28
description: '也是完赛了呢'
image: 'Title.png'
tags: [Algorithm]
category: '算法杂谈'
draft: false 
lang: 'zh_CN'
---

# 关于比赛

本届 SCIE 1024 程序员节活动由 M&CS Club 和 SCIE MLxAI 两个社团联合举办，设有两条赛道，算法和机器学习。我参加了算法的比赛，AK并取得了第二的成绩~

![名次](rank_competition.jpg)

此次比赛从周五下午的八点开始，持续两天。可以说时间非常充足了

以下是我的解题思路和对题目评价/吐槽（有些比较简单的就写得比较少）

最近从夯到拉的排名很火，所以我也对Coding Challenge这12道题排了一下名：

![题目排名](rank_problem.png)

*排名为主观顺序，仅代表个人的偏好

# 题目思路

> CR(字典序) : Alex Liu, Allen Xu, CQR, Tiger Shu

## A~D

水题略

## E (T5)

结合了课内FDE Cycle的模拟题，让人眼前一亮。但测试样例有点少，对于循环次数和报错`CRASH`的说明不够清晰，debug到自闭😭

## F (T6)

一道搜索题。用双向BFS优化了下速度，写了个简单的哈希表来标记已访问的图，给到顶级

```c++ title="T6.cpp" collapse={1-8, 19-116, 122-169, 171-181}
#include<bits/stdc++.h>
using namespace std;

struct grid{
    int grid[3][3];
    bool side;
};

int prime[9] = {2,3,5,7,11,13,17,19,23}; // Hash
int hash_grid(grid x){
    unsigned long long res = 0;
    for(int i=0;i<3;i++){
        for(int j=0;j<3;j++){
            res += prime[i*3+j]*x.grid[i][j];
        }
    }
    return res % 114514;
}

struct node{
    grid x;
    vector<int> o;
};

vector<node> visited[114514];
queue<node> ss, ee;

bool comp(grid a, grid b){
    bool res = false;
    for(int i=0;i<3;i++){
        for(int j=0;j<3;j++){
            if(a.grid[i][j] != b.grid[i][j])return false;
        }
    }
    return true;
}

bool searched(node x){
    int hashed = hash_grid(x.x);
    if(empty(visited[hashed]))return false;
    for(int i=0;i<visited[hashed].size();i++){
        if(comp(x.x, visited[hashed][i].x)){
            return true;
        }
    }
    return false;
}

node searched_grid(node x){
    int hashed = hash_grid(x.x);
    for(int i=0;i<visited[hashed].size();i++){
        if(comp(x.x, visited[hashed][i].x)){
            return visited[hashed][i];
        }
    }
}

void search(node x){
    visited[hash_grid(x.x)].push_back(x);
}

int step[4][2] = {{0,0},{0,1},{1,0},{1,1}};
node spin(node x, int opt){
    int i = step[opt][0];
    int j = step[opt][1];
    int cache = x.x.grid[i][j];
    x.x.grid[i][j] = x.x.grid[i+1][j];
    x.x.grid[i+1][j] = x.x.grid[i+1][j+1];
    x.x.grid[i+1][j+1] = x.x.grid[i][j+1];
    x.x.grid[i][j+1] = cache;
    return x;
}

node spinrev(node x, int opt){
    int i = step[opt][0];
    int j = step[opt][1];
    int cache = x.x.grid[i][j];
    x.x.grid[i][j] = x.x.grid[i][j+1];
    x.x.grid[i][j+1] = x.x.grid[i+1][j+1];
    x.x.grid[i+1][j+1] = x.x.grid[i+1][j];
    x.x.grid[i+1][j] = cache;
    return x;
}

void output_node(node x){
    cout<<x.x.side<<":";
    for(int i=0;i<x.o.size();i++)cout<<x.o[i]<<',';
    cout<<endl;
    for(int i=0;i<3;i++){
        for(int j=0;j<3;j++){
            cout<<x.x.grid[i][j]<<' ';
        }
        cout<<endl;
    }
    cout<<"-------"<<endl;
}

string code = "ABCDXXXXXX";
int main(){
    node start;
    for(int i=0;i<3;i++){
        for(int j=0;j<3;j++){
            start.x.grid[i][j] = i*3+j+1;
        }
    }
    start.x.side = true;

    node end;
    for(int i=0;i<3;i++){
        for(int j=0;j<3;j++){
            cin>>end.x.grid[i][j];
        }
    }
    end.x.side = false;
    ss.push(start);ee.push(end);

    vector<node> ans;
    bool STOP = false;
    int count = 0;

    while(!STOP){ // BFS
        node s_top = ss.front(); // From the start
        ss.pop();
        for(int opt=0;opt<4;opt++){
            node cc = spin(s_top, opt);
            cc.o = s_top.o;
            cc.o.push_back(opt);

            if(searched(cc)){
                node awa = searched_grid(cc);
                if(awa.x.side == true){ // Same side
                    continue;
                }else{ /////ANSWER!!
                    ans.push_back(cc);
                    ans.push_back(awa);
                    STOP = true;
                    break;
                }
            }
            search(cc);
            ss.push(cc);
        }
        
        if(STOP)break;

        node e_top = ee.front(); // From the end
        ee.pop();

        for(int opt=0;opt<4;opt++){
            node cc = spinrev(e_top, opt); // Spin in reverse direction
            cc.o = e_top.o;
            cc.o.push_back(opt);

            if(searched(cc)){
                node awa = searched_grid(cc);
                if(awa.x.side == false){ // Same side
                    continue;
                }else{ /////ANSWER!!
                    ans.push_back(awa);
                    ans.push_back(cc);
                    STOP = true;
                    break;
                }
            }
            search(cc);
            ee.push(cc);
        }

        if(STOP)break;
    }

    cout<<"YES"<<endl;
    for(int i=0;i<ans[0].o.size();i++){
        cout<<code[ans[0].o[i]];
    }
    for(int i=ans[1].o.size()-1;i>=0;i--){
        cout<<code[ans[1].o[i]];
    }
    cout<<endl;

    return 0;
}
```
## G (T7)

刚开始看到有图以为这也是一道搜索题，写了个深搜，TLE之后就去做别的题了。后面才发现其实有更巧妙的解法


## H (T8)

一道很板子的树形dp

对于每棵子树，我们都可以分为两种情况：它的祖先没有进行翻转和进行了翻转（翻转：指操作2，翻转子树）（如果偶数个祖先都翻转了，则视为没有翻转，奇数同理）。所以我们可以在每个节点维护两个值 `ans[0]` 和 `ans[1]` ，分别代表在它的祖先不翻转和翻转的情况下，这棵子树需要多少次操作才能让这个子树都变为正常

也就是说：

对于**叶子节点**来说，如果祖先没有翻转 `ans[0]` ，那么当它被损坏时操作次数为1，否则为0. 如果祖先翻转了 `ans[1]` ，那么就反过来

对于**非叶子节点**来说，我们可以分类讨论一下

记我自身的状态为 `status`（损坏为0，正常为1）,也就是说我没被祖先翻转的操作次数为 `!status` ，被翻转后的操作次数为 `status` ，则

- 祖先**没有翻转**（求 `ans[0]` ）：
  - 我**不翻转**，结果为我改变自己的操作次数 `!status` ，加上子节点们不翻转 `ans[0]` 的操作次数
  - 我**翻转**，结果为我翻转后改变自己的操作次数 `status` ，加上子节点们翻转 `ans[1]` 的操作次数，再加上我翻转产生的操作次数1
  - 最后， `ans[0]` 为两者的**最小值**
- 祖先**翻转**了（求 `ans[1]` ）：
  - 我**不翻转**，结果为祖先翻转后我改变自己的操作次数 `status` ，加上子节点们翻转 `ans[1]` 的操作次数
  - 我**翻转**，两个翻转产生的效果抵消，结果为我改变自己的操作次数 `!status` ，加上子节点们不翻转 `ans[0]` 的操作次数，再加上我翻转产生的操作次数1
  - 最后， `ans[1]` 为两者的**最小值**


后序遍历整个树，从下到上更新每个节点的`ans`，最后的答案就为根节点不翻转 `ans[0]` 和根节点翻转 `ans[1]` 加1的最小值

> Talk is cheap, show me the code.

```c++ title="T8.cpp" collapse={1-9, 29-41} {14-15, 26-27, 45}
#include<bits/stdc++.h>
using namespace std;
#define MAX 100009

struct node{
    vector<int> sons;
    int ans[2];
}tree[MAX];

bool status[MAX];
void dp(int k){
    // Leaf
    if(tree[k].sons.empty()){
        tree[k].ans[0] = !status[k];
        tree[k].ans[1] = status[k];
        return;
    }
    // Not Leaf
    int tot = 0;
    int tot_inv = 0;
    for(int i = 0; i < tree[k].sons.size(); i ++){
        dp(tree[k].sons[i]);
        tot += tree[tree[k].sons[i]].ans[0];
        tot_inv += tree[tree[k].sons[i]].ans[1];
    }
    tree[k].ans[0] = min(!status[k] + tot, status[k] + tot_inv + 1);
    tree[k].ans[1] = min(status[k] + tot_inv, !status[k] + tot + 1);
}

int main(){
    int n;
    cin>>n;
    for(int i = 1;i <= n; i++){
        cin>>status[i];
    }

    for(int i = 1; i < n; i ++){
        int a, b;
        cin>>a>>b;
        tree[a].sons.push_back(b);
    }

    dp(1);

    cout<<min(tree[1].ans[0], tree[1].ans[1]+1); // Answer
    return 0;
}
```

## I (T9)



## J (T10)

博弈论，在 $m\geq 32$ 后开始以八行为一组循环。有一个20以后的组合比较难想，最后写了个模拟把这个爆出来了。给到人上人，主要是因为代码实现比较简单

不过多评价，这种题就要享受自己构建可行方案解题的乐趣（嗯嗯）

```c++ title="T10.cpp" {4, 12}
#include<bits/stdc++.h>
using namespace std;
#define int long long
int remainder[9] = {0, 0, 0, 4, 4, 4, 6, 7};

signed main(){
    int T;
    cin>>T;
    while(T--){
        int a;
        cin>>a;
        cout<<(remainder[a%8] + (a / 8) * 4  - 2*(a%8==7&&a>10) - (a>=30&&a%8==6))<<endl;
    }
    return 0;
}
```

## K (T11)



## L (T12)



# 题外话

2024的Coding Challenge活到了第二天的下午，这次第一名的大佬熬到第二天凌晨6点左右就完赛了，:spoiler[快进到2026年的1024比赛开赛当天就被AK(]

主播这次就比较摆了，磨蹭到结束前两个小时才AK（

每次打OI竞赛的时候都很感慨自己当初为什么要来国交，不继续学体制内信息学奥赛（哭）

总之非常感谢出题组的付出，使这个活动延续下去，也提供了一个认识各位大佬的平台~