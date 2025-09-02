---
title: '拓展kmp (exkmp/ Z function)'
published: 2023-11-11
updated: 2025-09-02
description: 'Z function'
image: 'https://image.haoye.plus/title/exkmp.jpg'
tags: [Algorithm, String]
category: 'Algorithm'
draft: false 
---

# 引入

KMP算法可以求解出一个模式串在文本串中出现的位置。扩展KMP算法（Z函数）在这个思路上进行了延伸。对于文本串S的每一个后缀（如果文本串S长度为n的话，就指的是 `S[i:n]` ，其中 `(0<=i<=n-1)` ），找到这个后缀与模式串T的最长公共前缀的长度，存储在 `extend[i]` 中。设模式串T长度为m，不难发现，如果我们只看 `extend[i] = m` 的情况，相当于在文本串S找出模式串T出现的位置，就和原本的KMP算法一样了。

扩展KMP算法在国外一般叫做Z函数(Z function)。

# 计算extend数组

对于模式串T，我们需要预处理一个数组 `next` 。其第i项表示从i开始T的后缀与T的最长公共前缀的长度。`next` 数组的求解在下节可以看到，我们先来看看如何用 `next` 数组计算出 `extend` 数组。

和之前说的马拉车算法类似，拓展KMP算法也是利用之前的数据对暴力算法进行优化。维护一个当前所到达过的最右边的值 `right` 以及所对应的起点 `left` （即 `left + extend[left] - 1` 的最大值为 `right` ）。此时在第 `i` 项，默认其前面的 `extend[i-1], extend[i-2], ... extend[0]` 都已经更新完毕。

如果 `i > right` ，说明此时第 `i` 项还没有更新。此时就用最简单的暴力算法向前推进，并且更新 `left` 和 `right` 的值。

```C++
left = i;
while((i+extend[i])<s.length() && (extend[i])<t.length() && t[extend[i]] == s[i+extend[i]]){//思考题：为什么这里没有+1
	extend[i]++;
}
right = left + extend[i] - 1
```

如果 `i <= right` ，就可以利用 `left` 和 `right` 的值来更新 `extend[i]` 了。

## 求解extend数组的最小值

![extend高级图片2](https://image.haoye.plus/Screenshot%20from%202023-11-11%2019-59-13.png)

从上图结合之前学的内容可知， `S[left:right+1]` 和 `T[0:right-left+1]` 的值是相同的。也就是 `S[i:right+1]` 与 `T[i-left:right-left+1]` 的值是相同的（图中红色区域和绿色区域是相同的）。`next[i-left] = 2` 可以求得 `T[0:2] = T[i - left:i - left + 2 + 1]` 也就是黄色区域等于绿色区域的前两个。又因为红色区域等于绿色区域，所以红色区域前两个等于黄色区域。也就是文本串S以第i项开头的后缀与模式串T的最长公共前缀为2，也就是黄色区域的长度 `next[i - left]`。进而得到 `extend[i] = next[i - left] `

![extend高级图片1](https://image.haoye.plus/Screenshot%20from%202023-11-11%2020-03-04.png)

我们再来看一个例子。

根据上文所述的方法画出红色，绿色和黄色区域。我们会发现套用上一个结论行不通。因为黄色区域的长度已经超过了红色区域的长度。而我们只能保证红色区域与绿色区域相同，并不能保证红色区域的下一个值和绿色区域的下一个值相同，也就是 `S[5]` 和 `T[4]` 不能确定是否相同。此时就应该用红色区域的长度作为 `extend[i]` 的值。`extend[i] = right - i + 1`。

综上，可以得出一个通用式子。

```c++
extend[i] = min(right - i + 1, next[i - left]);
```



## 拓展

如果 `i + extend[i] - 1 = right` ，说明本次更新已经到了 `s[left:right+1]` 所管辖的尽头，需要对其进行更新。此时就和暴力的时候一样就好了，拓展 `extend[i]` 的值并且更新 `left` 和 `right`。

```C++
if(i + extend[i] - 1 > right){
    left = i;
    while((i+extend[i])<s.length() && (extend[i])<t.length() && t[extend[i]] == s[i+extend[i]]){//思考题：为什么这里没有+1
        extend[i]++;
    }
    right = i + extend[i] - 1
}
```

# next 数组

其实和上一节很相似，只不过是文本串和模式串都是T而已。这里就直接上代码

```c++
void calNextt(){
    nextt[0] = t.length(); // 自己和自己的最长公共前缀就是自己本身
    int left,right;
    left = right = 0;
    for(int i=1;i<t.length();i++){
        if(i<=right)
            nextt[i] = min(right-i+1,nextt[i-left]);
        while((i+nextt[i])<t.length() && t[nextt[i]] == t[i+nextt[i]]){
            nextt[i]++;
        }
        if(i+nextt[i]-1>right){
            left = i;
            right = i+nextt[i]-1;
        }
    }
}
```

# 代码实践

代码实践：[洛谷P5410](https://www.luogu.com.cn/problem/P5410)

**不要忘了用long long!!!!（改了很久的愤怒）**

```c++
//LICENSE: GPL v3.0
//https://blog.haoye.plus

#include<iostream>
#include<string>
#define int long long
using namespace std;
string s,t;
int nextt[20000009];//next 为保留字，不能用
int extend[20000009];

void calNextt();
void calExtend();

signed main(){
    cin>>s>>t;

    calNextt();
    //计算答案
    int ans = nextt[0]+1;
    for(int i=1;i<t.length();i++)
        ans = ans xor ((i+1)*(nextt[i]+1));
    cout<<ans<<endl;

    calExtend();
    //计算答案
    ans = extend[0]+1;
    for(int i=1;i<s.length();i++){
        ans = ans xor ((i+1)*(extend[i]+1));
    }
    cout<<ans<<endl;

    return 0;
}

void calNextt(){
    nextt[0] = t.length(); // 自己和自己的最长公共前缀就是自己本身
    int left,right;
    left = right = 0;
    for(int i=1;i<t.length();i++){
        if(i<=right)
            nextt[i] = min(right-i+1,nextt[i-left]);
        while((i+nextt[i])<t.length() && t[nextt[i]] == t[i+nextt[i]]){
            nextt[i]++;
        }
        if(i+nextt[i]-1>right){
            left = i;
            right = i+nextt[i]-1;
        }
    }
}

void calExtend(){
    extend[0]=0;
    while((extend[0])<s.length() && (extend[0])<t.length() && t[extend[0]] == s[extend[0]]){
        extend[0]++;
    }

    int left,right;
    left = 0;
    right = extend[0]-1;
    for(int i=1;i<s.length();i++){
        if(i<=right)
            extend[i] = min(right-i+1,nextt[i-left]);
        while((i+extend[i])<s.length() && (extend[i])<t.length() && t[extend[i]] == s[i+extend[i]]){
            extend[i]++;
        }
        if(i+extend[i]-1>right){
            left = i;
            right = i+extend[i]-1;
        }
    }
}
```

