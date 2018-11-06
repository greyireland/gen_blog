---
title: git cheatcheet
tags:
  - git
categories:
  - tool
date: 2018-09-19 16:12:32
---

# 简图
![](https://ws1.sinaimg.cn/large/e5320b2aly1fwy5q46d3lj215k10nabc.jpg)


# 常用命令
~~~
git add . //ga
git status //gs
git diff //gd
git commit -m "desc" //gcm
git push //gp
git checkout -b feature_1 //gcb
git pull origin master //gpom
~~~

## 偶尔用到
~~~
git push --set-upstream origin dev_2 
git stash
git log
git cherry-pick xxx
git init
git clone
~~~

### 很少用到
~~~
git config --global user.name "muName"
git config --global user.email "myEmail"
~~~

### GET NEW THING

#### 比对 diff

```
git diff 比对当前内容和暂存区内容。
git diff HEAD 比对当前内容和最近一次提交。
git diff HEAD^ 比对当前内容和倒数第二次提交。
git diff HEAD^ HEAD 比对最近两次提交。
```
