---
title: 创建hexo遇到的一些问题
date: 2023-01-04 14:31:45
tags:
---

## 安装问题
在`yarn add global hexo-cli`后，无法执行hexo命令（command not found）
在添加全局变量未果后，发现需要使用npx指令
* `npx hexo init blog`
* `cd blog`
* `yarn add`
* `npx hexo server`
会在4000端口生成网站

## 运行问题
但成功在4000端口运行后，发现无法载入，可能是端口已经被占用
*解决方法：修改端口
`npx hexo s -p 5000`
PS:如果找不到hexo命令，都需要提前加个npx，因为在命令行下调用，可以让项目内部安装的模块用起来更方便，npx运行的时候，会到node_modules/.bin路径和环境变量$PATH里面，检查命令是否存在，所以系统命令也可以调用。
