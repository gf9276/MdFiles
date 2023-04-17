<!-- TOC -->

- [1. matplot.md](#1-matplotmd)
  - [1.1. 关于axes](#11-关于axes)

<!-- /TOC -->


# 1. matplot.md

关于 python 绘图的知识

[参考链接](https://zhuanlan.zhihu.com/p/93423829)

## 1.1. 关于axes

首先 matplotlib 架构上分为三层
底层：backend layer
中层：artist layer
最高层：scripting layer

在任意一层操作都能够实现画图的目的，而且画出来还都一样。但越底层的操作越细节话，越高层越易于人机交互。

.plt 对应的就是最高层 scripting layer。这就是为什么它简单上手，但要调细节就不灵了。

ax.plot 是在 artist layer 上操作。基本上可以实现任何细节的调试。

backend layer, 至今，我还没有见有人在这个layer上操作过。

另外，对于axes (翻译就是 坐标系)，我看到有个评论写了很多，说的没有错，但也没说到点子上。
如果大家学过大学物理的话，会知道每一个物体都有一个独立的坐标系。而matplotlib 就是引用这一理念。所以在artist layer上画多条曲线时，你会用 ax1.plot, ax2.plot ... 而最后显示在一张图上是因为所有独立坐标系对齐的结果。