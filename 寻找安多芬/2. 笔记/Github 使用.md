---
created: 20211125 16:54
tags: [前端]
---

# Github 使用
# 提交代码
## 自己是仓库的成员
1. clone 代码
2. 创建分支提交
3. 合并到 master
4. 通过 Pull Requests 合并代码
## 自己不是仓库的成员
1. Fork 别人的代码到自己的 Github
2. clone Fork 后的仓库代码
3. 添加远程仓库
```bash
# 添加远程仓库
git remote add upstream git@github.com:su37josephxia/wheel-awesome.git
```

```bash
# 抓取源仓库修改 master 分支
git fetch upstream master
# 切换分支
git checkout master
# 合并远程分支
git merge upstream/master
```
4. 提交代码
5. 通过 Pull Requests 合并代码

# SSH 使用方法
测试
