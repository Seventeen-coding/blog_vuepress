---
title: leetcode数据结构
date: 2020-04-20
tags:
 - leetcode
categories:
 - 技术分享
---

# leetcode 数据结构

> 资料来源：https://leetcode-cn.com/explore/learn

## 数据结构

存取数据的特殊结构

## 数组

### 存取方式

取：下标偏移获取 存： 替换数据

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/31/screen-shot-2018-03-20-at-191856.png)

### 具备属性：

存储空间：连续的

头位置：存储空间首地址

下标：index

存方法：set

取方法：get

### 进阶

多维数组

## 链表

### 存取方式

取，头N次循环访问下一节点N节点，存，头N次循环断开节点，插入节点，拼接节点。

单链

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/12/screen-shot-2018-04-12-at-152754.png)

双链

![img](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/04/17/screen-shot-2018-04-17-at-161130.png)

### 具备属性

- 存储空间  不连续 
- 头位置 ：存储空间首地址
- 存方法 ：insert
- 取方法 ：delete



## 队列

### 存取方式

取头存尾，先入先出

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/08/14/screen-shot-2018-05-03-at-151021.png)



### 具备属性

- 存储空间 ：连续空间：数组， 不连续：链表
- 头位置 ：front head
- 尾位置 ：back tail
- 存方法 ：enqueue
- 取方法 ：dequeue

进阶

循环队列

- 继承队列属性
- 存储空间：循环链表





## 栈

### 存取方式

取头存头，后入先出

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/06/03/screen-shot-2018-06-02-at-203523.png)



### 具备属性

- 存储空间 ：连续空间：数组， 不连续：链表
- 尾位置 ：back tail
- 存 方法 ：push  位置：tail + 1
- 取 方法 ：pop   位置： tail - 1



## 哈希表

### 存取方式

哈希表的关键思想是使用哈希函数`将键映射到存储桶`

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/09/06/screen-shot-2018-02-19-at-183537.png)

### 具备属性

- 存储空间 ：连续空间：数组， 不连续：链表
- 下标 ：keys
- 转换函数：哈希算法
- 存 方法 ：set[value]
- 取 方法 ：get[key]

