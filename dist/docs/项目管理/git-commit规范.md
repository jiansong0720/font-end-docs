# Git commit

> 目录

[TOC]

## 1. 格式: <type>(<scope>): <subject>

> 例

```
fix(首页): 修复首页登录bug
feat(个人中心): 增加退出动画
```



### 1.1 格式说明

#### type

-  `commit` 的类别，允许使用以下7个标志
   -  feat：新功能（feature）
   -  fix：修补bug
   -  docs：文档（documentation）
   -  style： 格式（不影响代码运行的变动）
   -  refactor：重构（既不是新增功能，也不是修改bug的代码变动）
   -  test：增加测试
   -  chore：构建过程或辅助工具的变动

#### scope

- 说明 `commit` 影响的范围，比如数据层、控制层、视图层等等

#### subject

-  `commit` 目的的简短描述，不超过50个字符, 结尾不加标点

   ​


## 2.  异常处理

	### 2.1 在当前分支撤销之前commit

> 如果当前 commit 用于撤销以前的 commit，则必须以`revert:`开头，后面跟着被撤销 Commit 的Header。
>

```
例: 
revert: feat(pencil): add 'graphiteWidth' option

This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
```

> Body部分的格式是固定的，必须写成`This reverts commit <hash>.`，其中的`hash`是被撤销 commit 的SHA 标识符。

### 2.2 分支合并后撤销commit 

> 如将 A 功能从 branchA 合入到主分支 master 后过了很多个提交后,发现 A 功能某个地方有 bug,那最好的方法就是跳回到写 A 功能的时候进行修改。

```
如我现在的 git log 如下:

4fd65115db FUNCTION Mipi Camera Camera IC: OV13850 Interface: RX197a8ad0f7f FUNCTION 移植 8寸 Mipi LCD Driver IC: RM720149633cf0919 FUNCTION 移植 8寸 TP Driver IC:GT911
我现在发现当时移植 TP 的时候有 bug,我需要回到 9633cf0919 对进行 TP 进行移植的时候来修复这个 Bug。

1、将当前分支无关的工作状态进行暂存

git stash
2、将 HEAD 移动到需要修改的 commit 上

git rebase 9633cf0919^ --interactive
3、找到需要修改的 commit ,将首行的 pick 改成 edit 
4、开始着手解决你的 bug 
5、 git add 将改动文件添加到暂存 
6、 git commit –amend 追加改动到提交 
7、git rebase –continue 移动 HEAD 回最新的 commit 
8、恢复之前的工作状态

git stash pop
```








