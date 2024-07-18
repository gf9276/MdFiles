<!-- TOC -->

- [1. Leedcode.md](#1-leedcodemd)
- [2. 记录点细节](#2-记录点细节)
  - [2.1. todo](#21-todo)
  - [2.2. int 与 Integer 转换](#22-int-与-integer-转换)
    - [2.2.1. int\[\] 转 List](#221-int-转-list)
    - [2.2.2. int\[\] 转 Integer\[\]](#222-int-转-integer)
    - [2.2.3. List 转 int\[\]](#223-list-转-int)
    - [2.2.4. Integer\[\] 转 int\[\]](#224-integer-转-int)
  - [2.3. 找子集（暂时记录一下，我揣摩揣摩）](#23-找子集暂时记录一下我揣摩揣摩)
  - [2.4. 难度分](#24-难度分)

<!-- /TOC -->

# 1. Leedcode.md

刷题，记录

[按照这个顺序来](https://www.zhihu.com/question/266888066)

![](https://cdn.jsdelivr.net/gh/gf9276/image/Leetcode/20230515164857.png)

[初级算法](https://leetcode.cn/leetbook/detail/top-interview-questions-easy/)

# 2. 记录点细节

## 2.1. todo

1. 找图（最大正方形）
2. 排序

## 2.2. int 与 Integer 转换

### 2.2.1. int[] 转 List<Integer>

```
Arrays.stream(nums).boxed().toList(); // nums 是 int[]
```

Arrays.stream(arr) 可以替换成 IntStream.of(arr)。

1. 使用 Arrays.stream 将 int[]转换成 IntStream。
2. 使用 IntStream 中的 boxed()装箱。将 IntStream 转换成 Stream<Integer>。
3. 使用 Stream 的 collect()，将 Stream<T>转换成 List<T>，因此正是 List<Integer>

### 2.2.2. int[] 转 Integer[]

```
Arrays.stream(data).boxed().toArray(Integer[]::new); // nums 是 int[]
```

1. 前两步同上，此时是 Stream<Integer>。
2. 然后使用 Stream 的 toArray，传入 IntFunction<A[]> generator。这里`Integer[]::new`是 Lambda 写法之一，意思是创建一个数组的构造器方法。就是将数组构造器作为 toArray 的输入了，换句话说，就是将数组的创造函数作为 toArray 的输入了。
3. 这样就可以返回 Integer 数组。
4. 不然默认是 Object[]。

### 2.2.3. List<Integer> 转 int[]

```
int[] arr1 = list1.stream().mapToInt(Integer::valueOf).toArray();
```

1. 想要转换成 int[]类型，就得先转成 IntStream。
2. 这里就通过 mapToInt()把 Stream<Integer>调用 Integer::valueOf 来转 3. 成 IntStream。
3. 而 IntStream 中默认 toArray()转成 int[]。
4.

### 2.2.4. Integer[] 转 int[]

```
int[] arr2 = Arrays.stream(integers1).mapToIn(Integer::valueOf).toArray();
```

思路同上。先将 Integer[]转成 Stream<Integer>，再转成 IntStream。

## 2.3. 找子集（暂时记录一下，我揣摩揣摩）

p = (p - 1) & j

## 2.4. 难度分

[这里有难度分表](https://huxulm.github.io/lc-rating/zen)

[也可以装插件](https://github.com/zhang-wangz/LeetCodeRating)