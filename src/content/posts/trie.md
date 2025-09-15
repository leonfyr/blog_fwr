---
title: '字典树(Trie)'
published: 2023-11-18
updated: 2025-09-02
description: 'Trie'
image: 'https://image.haoye.plus/title/trie.jpg'
tags: [Algorithm, String]
category: '[Series] 字符串算法'
draft: false 
lang: 'zh_CN'
---

AC自动机的前置知识（=^^）/

# 问题

给定很多个文本串 `t[i]` 和很多个模式串 `s[i]` 。怎样算出每个文本串是多少个模式串的前缀？

# 字典树~削弱版~

我们一个一个匹配显然太慢了。有没有什么方法可以简化这个流程呢？

![美丽的Trie图](https://image.haoye.plus/Screenshot%20from%202023-11-18%2019-47-15.png)

这就是，，，，**字典树**！！

可以从上面的图片看到，对于多个字符串，如果把他们的公共前缀合在一起，最终得到的结果类似于一个树。这样具有公共前缀的不同字符串只用枚举一次就可以了！！！（这就是为什么它也叫**前缀树**）

**不过**，这张图片还有点不完美。我们并不知道有几个模式串有这个前缀。可以在字典树的每个节点加一个数据 `num[i]`（在图片右下角），记录拥有这个公共前缀的字符串的个数。

**再**给每个字符串的末尾打上标记后（虽然对于上面的问题来说这个步骤不重要，但是看到下一章的AC自动机（~~自动AC机~~）来说，这个就重要了），我们就得到了一个比较完美的字典树。

![高级智慧炫酷充满力量的最终版本字典树](https://image.haoye.plus/Screenshot%20from%202023-11-18%2019-46-55.png)

可我们还没有考虑一种情况。

# 字典树

有没有发现，上一节这些字符串们的第一个字符都是 `O`

对于第一个字符不同的字符串该怎么弄呢？

我们可以定义一个虚拟的**根节点**。这个节点并没有实际的值，只不过是为了链接第一个字符不同的字符串们的。

![根节点特写](https://image.haoye.plus/Screenshot%20from%202023-11-18%2019-43-22.png)

为了演示方便，把根节点染成了黄色（其实代码实际用起来并不用做特别区分）。

把字符串A改为 `IF` 后，我们就得到了最终版本的字典树了。

![高级智慧炫酷充满力量的最终版本字典树+++](https://image.haoye.plus/Screenshot%20from%202023-11-18%2019-50-37.png)

# 查询

查询很简单。只要在文本串从头到尾遍历，然后在字典树中从上到下遍历就好了。

如果遍历着遍历着下面没节点了，说明文本串不是任何模式串的前缀。

如果遍历着遍历着文本串没了，那么此时所在的节点所记录的 `num[i]` 就是最终的答案。

（可以想想为什么会这样。~~（绝对不是懒得写了）~~）

# 代码实践

洛谷：[P8306](https://www.luogu.com.cn/problem/P8306)

```c++
#include<bits/stdc++.h> //万能头！！！

using namespace std;

int cnt;
int trie[3000009][70];
int num[3000009];

int c2i(char x){
    if('a'<=x and x<='z')return x - 'a' + 1;
    if('A'<=x and x<='Z')return x - 'A' + 27;
    if('0'<=x and x<='9')return x - '0' + 53;
}

inline void clearTrie(){
    for(int i=0;i<=cnt;i++){
        for(int j=0;j<70;j++){
            trie[i][j]=0;
        }
        num[i]=0;
    }
    cnt = 0;
}

inline void pushTrie(string x){
    int u=0;
    for(int i=0;i<x.length();i++){
        int c = c2i(x[i]);
        if(!trie[u][c]){
            trie[u][c]=++cnt;
        }
        u=trie[u][c];
        num[u]++;
    }
}

int queryTrie(string x){
    int i,u;
    i=u=0;
    while(i<x.length() && trie[u][c2i(x[i])]){
        u=trie[u][c2i(x[i])];
        i++;
    }
    if(i == x.length()){
        return num[u];
    }
    return 0;
}

int main(){
    int t;
    cin>>t;
    while(t--){
        clearTrie();
        int n,q;
        cin>>n>>q;
        while(n--){
            string x;
            cin>>x;
            pushTrie(x);
        }
        while(q--){
            string x;
            cin>>x;
            cout<<queryTrie(x)<<endl;
        }
    }
}
```

