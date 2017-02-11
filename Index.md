# Matplotlib

> Matlab的语法、python语言、LaTeX的画图质量，你值得学习~

## I. Installing

> 基本上第一学期安装过Python的同学都含有Matplotlib和numpy这两个包，所以可以直接略过。

如果没有的，在[这里](http://matplotlib.org/users/installing.html)去学习如何安装，也推荐使用pip安装管理python的其他包

## II. Introduction

> 对于matplotlib来说，它的功能远远不止局限于画函数图像这一点，它可以生成很多种图表结构，从简单的描点作图，直方图到K线等复杂的图表。
>
> 如果需要更多的功能，如学长学姐们的毕设论文，需要精致的数据图像，可以先到知乎的[如何在论文中画出漂亮的插图](https://www.zhihu.com/question/21664179)去看最高票答案，然后决定是否需要使用Matplotlib，决定之后直接到[官网](www.matplotlib.ogr)去阅读docs即可。
>
> 以下则是我对于目前这个阶段，可能用到的一些特性的总结，可以帮助大家快速入手Matplotlib.

## III. Main Body

>在某次研究时间复杂度的时候，我想比较两个函数的阶，但是方程直观上不方便比较，因此我想通过画图的方式，来看出两个函数的变化规律
>
>例：$n^{0.75} \cdot \log_{2}n = O(n)$ ？
>

#### 1. Plot

Plot是默认在当前对象中进行画图，而本文中的figure和subplot其实都只是起到了选取画图对象的功能，因此这个才是重中之重。

**直接以代码示例**

```python
"""
Simple demo with multiple subplots.
"""
import numpy as np
import matplotlib.pyplot as plt


x1 = np.linspace(0,5,1000)
x2 = np.linspace(0,5,1000)

fig = plt.figure(1)

y1 = pow(x1,0.75) * np.log2(x1)
y2 = x2

plt.subplot(111)

plt.plot(x1, y1,color = 'b',label = '$n^{0.75} \cdot \log_{2}n$')

plt.plot(x2, y2,color = 'g',label = '$n$')

plt.xlabel('time (s)')
plt.ylabel('Undamped')
plt.legend(loc = 'upper left',shadow = True)


plt.grid()
plt.show()
```

**图是这样的**

![图 1](http://p1.bqimg.com/567571/e6a0d7f58eed703a.png)

首先介绍一下这个图中这几个按钮的特性，我们利用第三个/Pan按钮和第四个/Zoom按钮，可以将交点坐标放大。

![图 2](http://i1.piimg.com/567571/ca943df10e2ff87e.png)

而最后一个按钮/save，如果是动态绘制的图形可以保存成 gif，很神奇的地方对吧？则可以保存此图形用于其他地方，图像如下。

![图 3](http://i1.piimg.com/567571/31e57b8f4089390c.png)

下面来分析代码，讲解pyplot的一些基础包原理。

1. 其中x1和x2都是数组类型，np.linspace是调用numpy中的一个类，其中的参数表示生成在区间0~5之间总共1000个值大小的数组，前两个值是区间范围，第三个值可以理解为密度。
2. 而y1和y2同样是数组类型，本例中实际相当于将x1于y1通过$x_{1}^{\frac{3}{4}} \cdot log_{2}x$的函数式关联起来，y2同理。
3. 接下来则是plot子类，plot接受x和y两个数组作为描点的依据，然后后面如color，label都是可选参数。
   - color 的参数很灵活，可以是 b ，也可以是 blue，基本上只要写对颜色名字，就能找到相应的颜色，当然也可参照 docs 。
   - label 则是指图示的意思，label的格式支持$\LaTeX$，非常酷对吧，它搭配 legend() 一起使用，下面在 legend() 处会一起提到。
   - what's more，plot的作用远不止此，在docs里面可以看到更多奇妙的用法。
4. xlabel 指横轴图示，ylabel 指纵轴图示。
5. legend() 这个子类会创建一个小框，这个小框里会显示出每种颜色对应的文字(color matches label)，我在其中传入了两个参数，loc和shadow，其中loc指定了位置，可选值为'upper center'，'upper right'等，形式十分自由，shadow = True 则让这个小框有了一些投影，看起来更美观一些，同样这个子类还有很多很可爱的玩法，参照docs，你可以画出更实用，更美观的图形。
6. grid() 子类则是为背景添加了虚线小方格，可以为小方格指定大小，也可以指定虚线的种类，详情参见docs。
7. 代码的最后别忘了调用 show() 这个类，没有它就没有办法绘制图形了。

#### 2. Figure

figure是pyplot下的一个子类，调用figure可以创建一个图层，使其成为当前选中的画图对象。

```python
import matplotlib.pyplot as plt
plt.figure(figsize = (12,10),facecolor = 'blue')

#matplotlib.pyplot.figure(num=None, figsize=None, dpi=None, facecolor=None, edgecolor=None, frameon=True, FigureClass=<class 'matplotlib.figure.Figure'>, **kwargs)
```

其中有如上的可选参数，但是我使用之后发现可能只有 figsize 以及 facecolor 才较为有用

- figsize：使用一个tuple（元组）赋值，单位为inch，第一个值是width，第二个值是height
- facecolor：使用%s 类型赋值，'blue'，'black'……

如上的代码，会创建出一个宽为12，高为10，背景为蓝色的图层。

![图 1](http://i1.piimg.com/567571/596981201a214d0f.png)

如果只需要创建一个图层，这个操作其实默认是不需要的，因为默认会有一个图层，只是需要创建另一个图层的时候才可能需要figure操作。

#### 3.Subplot

subplot是pyplot下的另一个子类，其中参数情况如下，对于pyplot.subplot(2,1,1)，会创建一个2行1列的画图对象，从左至右，从上至下的选取画图对象。

`subplot(nrows, ncols, plot_number)`

对于刚才的代码，稍加改动则可变成如下的图示

```python
plt.subplot(211)

plt.plot(x1, y1,color = 'b',label = '$n^{0.75} \cdot \log_{2}n$')
plt.xlabel('time (s)')
plt.ylabel('Undamped')
plt.legend(loc = 'upper left',shadow = True)

plt.subplot(212)#(2，1，2)可以简写成212
plt.plot(x2, y2,color = 'g',label = '$n$')
plt.xlabel('time (s)')
plt.ylabel('Undamped')

plt.legend(loc = 'upper left',shadow = True)
```

![图 4](http://p1.bqimg.com/567571/ed9663ca70d0e5dd.png)

其中 subplot(211) 代表选定了上面那块区域为当前作图区域，画图完成之后需要选中下面那块，因此需要调用 subplot(212)，且需要重新设定 x, y 轴的文字。



## IV. End

简单的针对 plot() 和 subplot() 这两个子类进行了讲解，目前对这两个类的使用可以满足我当前的需求，至少通过画图，我可以得知$n^{\frac{3}{4}} \cdot log_{2}n = O(n)$，其实matplotlib的作用远不止此，我个人感觉它很适用于理工科，用于统计相关的数据并描点作图，大二上的有一次作业需要我们分析算法的时间复杂度并且画出图像，其实当时如果掌握了matplotlib的话，在cpp文件中，将规模n和对应所用的时间t，写出.txt文件，然后再调用python读入.txt，将一组 n 传入上述的 x 数组，将时间 t 传入上述的y数组（ plot 的本质是根据数组值描点作图！），生成图像，比我用Excel画图简洁了太多。

下面贴一些很酷的图

![](https://pic1.zhimg.com/d1019455f649f11c1319e07f6d0289cc_r.jpg)



![](https://pic3.zhimg.com/6bdaaee539739245344d607731ef671a_r.jpg)

![](https://pic4.zhimg.com/f3ad883d7456f4178e4d2a9df53edadf_r.jpg)

甚至还可以[交互式作图](http://nbviewer.jupyter.org/github/plotly/python-user-guide/blob/master/s0_getting-started/s0_getting-started.ipynb)，简直太强大了~

