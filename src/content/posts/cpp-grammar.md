---
title: 'c++语法知识点'
published: 2023-01-10
updated: 2025-09-02
description: 'c++ grammar'
image: 'https://image.haoye.plus/title/snow_canada.jpg'
tags: [c++]
category: 'Programming Language'
draft: false 
lang: 'zh_CN'
---


c++程序设计
==========

by：小鸭呀

来源：信息学奥赛一本通+菜鸟教程+oiwiki+我的脑袋

本文章遵循cc协议，转载请署名

对于本文章的建议或纠错可以发表评论哦（=^^）

[TOC]

（如果支持的话应该上面会显示个目录）

## 让程序不闪退的神奇代码

`system("pause")`版本4.99以前

getchar() 约等于  system("pause")

## 注释

```c++
//这是一行注释
/*这是有头有尾的注释*/
```



## 标识符

含义：标识实体（变量名，函数名，对象名）

由大小写字母，下划线和数字0~9组成

必须以大小写字母和下划线开头

## 运算符

| 啥子运算符     |             干啥的             | 长啥样                                                       |
| -------------- | :----------------------------: | ------------------------------------------------------------ |
| 算数运算符     |            数值运算            | +,-,*,/（如果俩操作数是整数，则为整除）,% (俩操作数必须是整数) ,++,--(七种) |
| 关系运算符     |            比较运算            | >,<,==,<=,>=,!=(六种)     返回布尔值                         |
| 逻辑运算符     |            逻辑运算            | &&,\|\|,!                                                    |
| 位操作运算符   |           二进制运算           | &,\|,~（非),^(异或),<<,>>(六种)                              |
| 赋值运算符     |            赋值运算            | 简单赋值(=),复合算数赋值(+=,-=,*=,/=,%=),复合位运算赋值(&=,\|=,^=,<<=,>>=)三类共十一种 |
| 条件运算符     |            条件求值            | 条件?结果a:结果b         条件成立，用结果a，否则用结果b      |
| 逗号运算符     | 把若干个表达式组合成一个表达式 | ,                                                            |
| 指针运算符     |         取内容，取地址         | 取内容(*),取地址(&)                                          |
| 求字节数运算符 |       数据类型所占字节数       | sizeof(数据类型)                                             |
| 特殊运算符     |            玛卡巴卡            | (   )   [  ]   .   ->                                        |





### 优先级

自己打括号

## 常用库函数

依赖cmath库：

| 格式                       | 功能        |
| :------------------------- | :---------- |
| abs(x)/fabs(x)             | 绝对值      |
| exp(x)                     | e的x次方    |
| floor(x)                   | 向下取整    |
| ceil(x)                    | 向上取整    |
| log(x)                     | x的自然对数 |
| pow(x,y)                   | x的y次方    |
| sqrt(x)                    | 根号        |
| round(x)                   | 四舍五入    |
| sin,cos,tan,acos,asin,atan | 三角函数    |

依赖cstdlib库

| 格式   | 功能                          |
| :----- | :---------------------------- |
| rand() | 0到RAND-MAX之间随机取一个整数 |

依赖ctime库

| 格式    | 功能                       |
| ------- | -------------------------- |
| clock() | 返回程序从开始运行了多少秒 |

依赖algorithm库

| 格式            | 功能                            |
| --------------- | ------------------------------- |
| sort(a,a+n,cmp) | 对长度为n的a数组排序，用cmp方法 |



## 常量

###  定义常量

#define 变量名 数据

const 类型 变量名 = 数据;

常量初始化后不可以改变

### 整型常量

#### 十进制形式

如：99，-1

#### 八进制形式

0开头

如：012

#### 十六进制形式

0x开头

如：0x12A

## 变量

int i = 5,j,k(5);    //i，j，k为整型变量，i，k是5，j未知

a = b = c = 2       等同于    c = 2;b = c;a = b;

## 标准数据类型

### 整型与实型

| 数据类型       | 定义标识符           |
| -------------- | -------------------- |
| 短整型         | short [int]          |
| 整型           | [long] int           |
| 长整型         | long [int]           |
| 超长整型       | long long [int]      |
| 无符号整型     | unsigned [int]       |
| 无符号短整型   | unsigned short [int] |
| 无符号长整型   | unsigned long [int]  |
| 无符号超长整型 | unsigned long long   |
| 单精度实型     | float                |
| 双精度实型     | double               |
| 长双精度实型   | long double          |
| 布尔           | bool                 |
| 枚举类型       | enum                 |

注:科学计数法是指数形式的表示方法，`十进制小数E整数指数`，例如
$$
1.2\times10^5
$$
就是1.2E+5

### 枚举类型

```c++
enum 枚举名{
	标识符,标识符,标识符(=整型常数)
} 枚举变量;
//举个栗子
enum food{drink,eat=5}c;
c=drink;
```



### 重定义数据类型

```c++
typedef int metre;
```



## 转义字符

| 转义字符 | 含义         |
| -------- | ------------ |
| \b       | 退格         |
| \r       | 不换行的回车 |
| \0       | 空           |
| \n       | 回车         |

char类型里+32是大写转小写，-32是小写转大写

强制类型转换：类型名（被转换的可怜变量）

## 输入输出

事先说明：

```c++
#include<iostream>
#include<cstdio>       //这个章节的内容必须有这些语句
using namespace std;
```

**cout用小于，cin用大于**

### getchar

输入单个字符，无参

char ch;ch=getchar();

### putchar

输出单个字符，有参

putchar(66)      根据ASCII码66是字符B，输出B

### cout

#### setw

要导入iomanip库

```c++
#include<iomanip>
cout<<setw(6)<<a;
```

setw控制输出的宽度

### cstdio 库

#### 输入

`scanf("%类型",&变量名)`

格式符：

| 格式符 | 说明                           |
| ------ | ------------------------------ |
| d，i   | 十进制整数                     |
| u      | 无符号十进制整数               |
| o      | 八进制整数                     |
| x      | 十六进制整数                   |
| c      | 单个字符                       |
| s      | 字符串（非空格开始，空格结束） |
| f，e   | 实数                           |

附加格式说明符：

| 附加格式         | 说明             |
| ---------------- | ---------------- |
| l（L字母）       | 长整型或double型 |
| h                | 短型数           |
| 域宽（一个整数） | 输入多少东西     |
| *                | 不赋值给变量     |

scanf("%4d,%4d",&a,&b);

cout<<a<<','<<b<<endl;

如果输入的是12345678，则输入为1234,5678

##### scanf有返回值！！！！！

返回了成功读取的变量的个数哦！！！

```c++
while(scanf("%d",&x) != EOF){
​	语句
}
```

如果成功读取，就执行语句

#### 输出

`printf(格式控制符,输出列表)`

##### 格式符

| 格式符 | 说明                                  |
| ------ | ------------------------------------- |
| d或i   | 十进制整数                            |
| u      | 无符号十进制整数                      |
| x或X   | 十六进制无符号整数（没有前导符0x）    |
| o      | 八进制无符号整数（没有前导符0）       |
| c      | 字符                                  |
| s      | 字符串                                |
| f      | 单、双精度小数，一般6位               |
| e或E   | 单、双精度以指数形式输出小数，一般6位 |

##### d格式符

| 参数      | 说明              |
| --------- | ----------------- |
| d         | 数字本身长度      |
| md        | 输出m位           |
| -md       | 同上，但左对齐    |
| ld        | 长整型            |
| mld       | 输出m位，左补空格 |
| 0md或0mld | 位数不足m位补0    |



##### f格式符

| 参数  | 说明                         |
| ----- | ---------------------------- |
| f     | 实数格式，小数6位            |
| m.nf  | 总位数m（含小数点），n位小数 |
| -m.nf | 同上，左对齐                 |



##### s格式符

| 参数  | 说明                           |
| ----- | ------------------------------ |
| s     | 字符串                         |
| ms    | 指定宽度m                      |
| -ms   | 同上，左对齐                   |
| m.ns  | 输出m个字符位置，字符数最多n个 |
| -m.ns | 同上，左对齐                   |

## 分支

### if

```
//基本
if(条件){
	主体;
}

//if...else
if(条件){
	主体1;
} else {
	主体2;
}

//else if
if (条件1) {
	主体1;
} else if (条件2) {
	主体2;
} else if (条件3) {
	主体3;
} else {
	主体4;
}
```



### switch

case后面要加break哦

```c++
//实例
switch (a){
    case 1:{
        xxx;
        break;
    }
    case 2:{
        xxx;
        break;
    }
    default :{
        xxx;
    }
}
```



## 循环

### for

```c++
for(初始化;条件;更新){
	循环体;
}
```

### while

```
while(条件){
	循环体;
}
```

### do...while

```c++
do {
	循环体;
} while (条件);
```

### break 和 continue

break跳出循环，continue结束本次循环，前往下次循环

## 数组

初始化：

```c++
int a[5] = {1,2,3,4,5}   //初始化
int a[5] = {}      //初始化为0
int a[5] = {1,2}   //初始化为1,2,0,0,0
//读取
a[i];
    
```

### 复制

```c++
#include<cstring>
memcpy(b,a,sizeof(a))   //a复制到b
memcpy(b,a,sizeof(/*a的数据类型*/)*k)   //a复制k个元素到数组b
```

### 清零

```c++
#include<cstring>
int a[5];
memset(a,0/*除了str类型的数组其它只能为0和-1*/,sizeof(a))    //清零
```

## 二维数组

数组的数组

### 格式

数据类  数组名  [常量表达式1] [常量表达式2]

创造后为：常量表达式1行，常量表达式2列

## 程序运行时间

```c++
#include<ctime>
#include<iostream>
using namespace std;
int main(){
    ...代码
    cout<<clock()/CLOCKS_PER_SEC<<endl;   //输出用时
}
```

## 字符类型、字符数组和字符串

| 序号 | 函数 & 目的                                                  |
| :--- | :----------------------------------------------------------- |
| 1    | **strcpy(s1, s2);** 复制字符串 s2 到字符串 s1。              |
| 2    | **strcat(s1, s2);** 连接字符串 s2 到字符串 s1 的末尾。连接字符串也可以用 **+** 号，例如: `string str1 = "runoob"; string str2 = "google"; string str = str1 + str2;` |
| 3    | **strlen(s1);** 返回字符串 s1 的长度。                       |
| 4    | **strcmp(s1, s2);** 如果 s1 和 s2 是相同的，则返回 0；如果 s1<s2 则返回值小于 0；如果 s1>s2 则返回值大于 0。 |
| 5    | **strchr(s1, ch);** 返回一个指针，指向字符串 s1 中字符 ch 的第一次出现的位置。 |
| 6    | **strstr(s1, s2);** 返回一个指针，指向字符串 s1 中字符串 s2 的第一次出现的位置。 |

### 字符类型

一个字符

### 字符数组

装字符的数组

#### 输入

gets(字符串名称);   

getchar(); //返回字符，回车后才输入

只能输入一个字符串！！！

#### 输出

puts(字符串名称);

putchar(一个字符);

输入后会自动换行

### 字符串

结尾为”\0“，不计入字符串的实际长度

#### 字符串的输入输出

cin：输入、忽略开头的制表符、换行符、空格。遇到空格或换行停止（不会读取空字符）

getline：输入、直接读取一行内所有内容，包括空格。格式为`getline(cin,字符串变量)`

#### 字符串的常用操作

| 字符串s的操作       | 意义                                                        |
| ------------------- | ----------------------------------------------------------- |
| s.empty()           | s空？返回true否则false                                      |
| s.size()            | s中字符个数                                                 |
| s[i]                | s中第i个，从0开始                                           |
| s1+s2               | 连接s1和s2                                                  |
| s1=s2               | s1替换为s2的副本                                            |
| s1==s2              | 一样true，否则false                                         |
| !=,<,<=,>,>=        | 原意，不变                                                  |
| s.insert(pos,s2)    | s里面pos位置前插入s2                                        |
| s.substr(pos,n)     | 返回从pos起的n个字符，返回string                            |
| s.erase(pos,n)      | 删除从pos起的n个字符                                        |
| s.replace(pos,n,s2) | 替换从pos起的n个字符为s2                                    |
| s.find(s2,pos)      | 寻从pos下标起查找s2第一次出现的位置，找不到返回string::npos |
| s.c_str             | 返回与s内容相同的C语言风格的字符串临时指针                  |
| sscanf(s,"%d",%N)   | 约等于n=int(s)                                              |
| sprintf(s,"%d",N)   | 约等于s=str(n)                                              |
| s.substr(2,4)       | python中s[2,2+4-1]                                          |

#### 字典序

#### 定义

从前往后，挨个比较。第i不同，用i比较。如果相等，谁长谁大。一样的话，相等。

#### 方法

s1.compare(s2)

如果相等返回0，s1小于s2返回小于0，s1大于s2返回大于0

### 注意

字符串和字符数组的区别在于字符串的结尾是\'\\0\'

## 函数

### 定义

```c++
void swap(int &a,int &b);//声明
//主体
void swap(int &a,int &b){
    int tmp = a;a = b;b = tmp;
}
```

传址调用

### 匿名函数

```c++
[闭包](参数){函数体}                 //不指定返回值类型
[闭包](参数) -> 返回值类型 {函数体}   //指定返回值类型
```

#### 闭包

```c++
[]        //未定义变量.试图在Lambda内使用任何外部变量都是错误的.
[x, &y]   //x 按值捕获, y 按引用捕获.
[&]       //用到的任何外部变量都隐式按引用捕获
[=]       //用到的任何外部变量都隐式按值捕获
[&, x]    //x显式地按值捕获. 其它变量按引用捕获
[=, &z]   //z按引用捕获. 其它变量按值捕获
//来源:https://www.cnblogs.com/pzhfei/archive/2013/01/14/lambda_expression.html
```



## 文件操作

### freopen

需要导入cstdio库

```c++
#include<cstdio>
freopen("in.txt","r",stdin);
freopen("out.txt","w",stdout);
fclose(stdin);fclose(stdout);/*
freopen("CON","r",stdin);
freopen("CON","w",stdout);*/
```

### fopen

fopen版本

```c++
//定义文件指针
FILE *fin,*fout;
fin=fopen("in.txt","rb");
fout=fopen("out.txt","wb");
fscanf(fin,"%d",&temp);
fprintf(fout,"%d\n",sum);
fclose(fin);fclose(fout);
//切换回标准输入输出
fin=stdin;
fout=stdout;
fscanf(fin,"%d",&temp);
fprintf(fout,"%d\n",sum);
```

### 流

```c++
#include<cstdio>
#include<fstream>
ifstream fin("haoye.in");//定义输入流fin
ofstream fout("huaiye.out");//定义输出流fout
int main(){
	//fin输入，fout输出，用法和cin，cout一样
	fin.close();
	fout.close();
	return 0;
}
```



## 结构体

### 建立

```c++
struct 名字 {
	类型名 变量名;
	结构体名字指针 变量名;
} 变量名;
```

### 访问

```c++
名字.元素
指针->元素
(*指针).元素
```



## 指针

### 定义和运算

| 说明               | 样例             |
| ------------------ | ---------------- |
| 定义：类型 *变量名 | int a=10;int *p; |
| 取地址运算符：&    | p = &a;          |
| 间接运算符：*      | *p = 20;         |

指针可以和整数加减，每加一，偏移一个类型的长度

### 初始化

| 方法               | 说明                          |
| ------------------ | ----------------------------- |
| int *p = NULL;     | 声明零指针                    |
| int a;int *p=&a;   | p初始化为a的地址              |
| int *p = new(int); | 申请一个空间给p，*p内容不确定 |

### 无类型指针

```c++
void *p;
int a = 10;
p = &a;
cout<<*(int*)p<<endl;
```

### 指针作为函数参数

```c++
void swap(int *x,int *y){
	int t=*x;
	*x=*y;
	*y=t;
}
```

### 函数指针

```c++
int t(int a){
	return a;
}
int main(){
	cout<<t<<endl;
    int (*p)(int a);//定义函数指针变量,不能写成*p(int a)
    p=t;
    cout<<p(5)<<","<<(*p)(10)<<endl;
    return 0;
}
```

还有一种定义的方法

```c++
void t1(){cout<<"1";}
void t2(){cout<<"2";}
void t3(){cout<<"3";}
void t4(){cout<<"4";}
int sum(int a,int b){return a+b;}
typedef int (*LP)(int,int);
typedef void(*aa)();
int main(){
    aa a[]={t1,t2,t3,t4}
    a[1]();   //输出2
    LP p = sum;
    cout<<p(2,5);     //返回7
}
```

### 结构体指针

```c++
struct a{
    float b;
} *p;//这个可以
a *p;//这个也行
//应用结构体指针变量
p->b
(*p).score
```

### 动态

```c++
int *p = new int(1234);
delete p;
int *p = new int[114514];
delete[] p;
```

## 类（oop）

```c++
//基本：
class 类名{
	//访问修饰符：private/public/protected
    //变量
    //方法
    返回类型 operator/*跟上要改的运算符号*/(参数){
        阿巴阿巴;
    };
    virtual int haoye(){//调用函数的时候都用子类的，不用这个函数
        ;
    }
};
//派生类
class 好耶 : public/*访问修饰符*/ 坏耶 ,其他父类...{
    阿巴阿巴;
    int haoye(){
        ;
    }
};
```

<input type="checkbox" class="hooray"> 