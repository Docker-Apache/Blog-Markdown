---
title: MapReduce
date: 2021-05-10 22:47:39
tags:
  - mapper
  - reducer
category:
  - Hadoop
---

## map-reduce

数据以一条记录为单位，经过 map 方法映射成 key-val，相同的 key 为一组，一组调用一次 reduce 方法，

## Map

以一条记录为单位做映射，实现 key-val (键值对)

- 映射、变换、过滤
- 1 进 N 出

## Reduce

- 分解、缩小、归纳
- 一组进 N 出

## group(key)

键值相同作为一组（最小单元）

**block > split**: 1:1, N:1, 1:N
split > map --- 1:1
map > reduce --- N:1, N:N,: 1:1, 1:N
group(key) > partition 1:1, N:1, N:N, 1:N

fdaf
