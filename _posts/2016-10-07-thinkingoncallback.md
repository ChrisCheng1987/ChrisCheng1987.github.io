---
layout: post
title:  "回调函数思考"
date:   2015-09-26 18:51:59
categories: jekyll update
---


回调函数指通过函数指针或者函数引用将一个函数A作为参数传给函数B，以在特定时间（如函数B执行得到一些结果）执行函数A。
详细例子可参考知乎回答
[回调函数（callback）是什么？](https://www.zhihu.com/question/19801131) @futeng (Java代码)。Java中不能将函数作为参数传递，所以回调一般是通过interface实现的

## 回调的作用主要有两个

### 库函数或其他函数中执行外部函数
因为库函数都是事先写好的，因此要在特定时间或者得到特定结果时执行客户端程序，不能简单的include头文件然后直接调用A函数。就算在一般的B程序中，以参数形式传入函数也使得程序便利性大大增加。例子：

library.h (库函数，已封装)
{% highlight c %}
#include <cstdio>
void fill_screen(int (*some_func)()) 
//这里也可以使用泛型定义
//见https://www.zhihu.com/question/19801131@lee philip的回答
{printf("%d\n",some_func());}
{% endhighlight %}

使用库callback.cpp
{% highlight c %}
#include "library.h"

int draw1(){return 1;}
int draw2(){return 2;}
int draw3(){return 3;}

int main(){
    fill_screen(&draw3);//这里可以根据不同情况选择回调不同的函数，而不用修改库函数(函数B)
    fill_screen(&draw1);
    fill_screen(&draw2);
    return 0;
}
{% endhighlight %}

输出:
{% highlight c %}
3
1
2
{% endhighlight %}

### 异步调用
另一种使得回调函数非常重要的原因是它广泛应用于异步调用中。事件驱动程序编程中，通常需要监听事件的结果，如鼠标操作等，同时主程序继续执行。这时，可以通过多线程执行事件和回调函数响应时间来实现。注：这里其实还是和第一点有关，由于事件监听通常都是库函数，因此使用回调基本上是处理响应的唯一选择(无法修改库函数来响应事件)。
{% highlight c %}
void character_Move(int direction){...}//direction: e.g., 1 means move forward
//游戏主进程
void main()
{
    ...//主进程
    listenKeyboard(&character_Move);
    ...//主进程
}
void listenKeyboard(void (*respons_func)(int))
{
    new thread(){
        void run(){
            while(1){
                int key = get_direction();//取得键盘输入并返回int
                respons_func(key);
            }
        }
    }
}
{% endhighlight %}