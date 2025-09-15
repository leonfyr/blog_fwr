---
title: 'AC自动机(Aho-Corasick Algorithm)'
published: 2023-11-19
updated: 2025-09-02
description: 'AC Algorithm'
image: 'https://image.haoye.plus/title/ac.png'
tags: [Algorithm, String]
category: '[Series] 字符串算法'
draft: false 
lang: 'zh_CN'
---

~~自动AC机~~

前置知识：kmp算法

beta版本，还没加图片qwq（不过也能看）

# 引入

kmp算法处理了一个模式串在一个文本串中匹配的场景。如果是多个模式串在一个文本串匹配呢？

# Trie树

由于我们在[上一章](https://blog.haoye.plus/post/trie)详细讨论了字典树，这章就不做赘述

# Fail指针

如同kmp算法一样，AC自动机高效的原因也是**在失配的时候可以及时调整，避免重复运算**，用一个 `fail` 数组实现这一流程。

`fail[i]` 所储存的值 `u` 是存在于字典树中的 `i` 的最长后缀。

`fail[i]` 的值可以由比点 `i` 辈分还高的点的 `fail` 值得到。我们先来看一下过程

考虑计算 `fail[i]` 的值：

让 `u = fa[i]`，也就是i的父节点

1. 如果 `fail[u]` 的子节点中有 `i` 的值，那么 `fail[i] = fail[u]的那个子节点`
2. 如果 `fail[u]` 的子节点中没有 `i` 的值，那么 `u = fail[u]` ，从头继续
3. 如果 `u` 已经是根节点了，那么停止循环， `fail[u] = 0`

为什么这样可行呢？

因为 `i` 的父节点已经帮 `i` 找好了 `i` 前一个字符串的最长公共后缀的元素在哪里。对于 `i` 来说，只要“子承父业”，继承父亲所找到的那个元素就好了，如果“父业”（也就是 `fail[fa[i]]` 节点）下有他儿子就好。（对应步骤1）

如果没有也没关系。继承不了父亲的也可以沿着父亲找到的那个元素的链条往上爬，再往上找，直到找到某个元素使这个元素的子节点下面有 `i` 就行。（对应步骤2）如果实在没有直接 `fail[i] = 0`（对应步骤3）

# Fail 指针优化

看起来很美好，可是如果一直向上追溯肯定时间不够。

我们可以给每个元素都加上“虚拟儿子”，凑齐二十六个字母（如果题目有数字那也要包括）。这样子元素就可以直接“子承父业”， `fail[i] = fail[fa[i]].son[i对应的字符的值]`（不用上一节一直往上爬（步骤2）了，因为“父业”下面已经有这个儿子了~~（虽然可能是虚拟的）~~）。i的“虚拟儿子” `c` （也就是 `i` 原本没有 `c` 这个儿子）是`i.son[c] = fail[i].son[c]`。

前面一句话好理解。那么最后一句话呢？

其实“虚拟儿子”就保存着我们上一节循环后的答案。这里“真实儿子”对应着步骤1找到了元素，“虚拟儿子”对应着步骤2没找到元素。因为 `i` 的虚拟儿子 `c` 是由  `fail[i]` 的儿子 `c` 得到的。如果它是真实的那么最终 `i` 的虚拟儿子 `c` 就对应着另一个元素 `fail[fa[d]] = i,d的值为c` 的 `fail[d]` 的值，`fail[d] = i.son[c]`。即使它是虚拟的但他也是由`fail[fail[i]]` 的儿子 `c` 得到的。一直向上，和上一节的循环一样。如果遇到了真实的停，直到根节点。只不过是把这个过程**从上往下**进行了而已。

用**BFS**（广度优先搜索）从上往下更新，可以保证更新一个节点时比它辈分还大的节点已经更新完了。

引用一句名言

> Talk is cheap. Show me the code. 
>
> ​                                               --- Linus

```c++
queue<int> que;
void bfs(){
    for(int i=0;i<=25;i++){
        if(tr[0][i])que.push(tr[0][i]);
    }
    while(!que.empty()){
        int u=que.front();
        que.pop();
        for(int i=0;i<=25;i++){
            if(!tr[u][i])   tr[u][i]=tr[fail[u]][i];//创建虚拟儿子
            else{//更新fail指针
                que.push(tr[u][i]);
                fail[tr[u][i]]=tr[fail[u]][i];
            }
        }
    }
}
```



# 查询

运用虚拟儿子解决了匹配失败的问题。那么匹配成功走真的儿子，匹配失败走假的儿子，就可以忽略这一差别，直接向下走。我们上一章讲了在字符串末尾打上标记，查询的时候就可以直接向下走，然后记录遇到了几个标记，并且把他清零（如果只需要匹配一次的话）。

```c++
void find(string s){
    int u=0,c;
    for(int i=0;i<s.length();i++){
        c=s[i]-'a';
        u=tr[u][c];//向下走一步
        ans+=bo[u];
        bo[u]=0;//确保同一字符串只被统计一次
    }
    return;
}
```



# 代码实践

洛谷：[P3808](https://www.luogu.com.cn/problem/P3808)

```c++
#include<iostream>
#include<string>
#include<queue>

using namespace std;

#define N 1000009

int n;
int ans,cnt;
int fail[N];
int tr[N][30];
int bo[N];
 
void make(string s){
    int u=0;
    for(int i=0;i<s.length();++i){
        int c=s[i]-'a';
        if(!tr[u][c]){
            tr[u][c]=++cnt;
        }
        u=tr[u][c];
    }
    bo[u]++;
    return;
}

queue<int> que;
void bfs(){
    for(int i=0;i<=25;i++){
        if(tr[0][i])que.push(tr[0][i]);
    }
    while(!que.empty()){
        int u=que.front();
        que.pop();
        for(int i=0;i<=25;i++){
            if(!tr[u][i])   tr[u][i]=tr[fail[u]][i];//创建虚拟儿子
            else{//更新fail指针
                que.push(tr[u][i]);
                fail[tr[u][i]]=tr[fail[u]][i];
            }
        }
    }
}

void find(string s){
    int u=0,c;
    for(int i=0;i<s.length();i++){
        c=s[i]-'a';
        u=tr[u][c];
        ans+=bo[u];
        bo[u]=0;//确保同一字符串只被统计一次
    }
    return;
}

int main(){
    int n;
    string s;
    ans=0;cnt=0;

    cin>>n;

    //输入模式串
    while(n--){
        cin>>s;
        //建立trie树
        make(s);
    }

    //建立next指针
    bfs();
    
    //输入文本串
    cin>>s;
    //ac自动机
    find(s);

    cout<<ans<<endl;
    return 0;
}
```

