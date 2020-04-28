## 1.配置user信息

用户名，邮箱

```
$ git config --global user.name '用户名'
$ git config --global user.email '邮箱'
```

## git的作用域

```
设置！！！！！！！！！！！！！！！！！！！！！！
$git config --local  只对于某个仓库有效 ，优先级最高
$git config --global 当前用户的所有仓库有效
$git config --system 对系统所登录的用户有效

查看！！！！！！！！！！！！！！！！！！！！！！
git config --list --global 查看设置的全局信息
//那啥 日志太多的话 按q退出
```

## git 提交

```
git init //创建仓库
git status //查看git状态


git add '文件名'  //将某个文件存到 暂存区

git add -u：将文件的修改、文件的删除，添加到暂存区。
git add .：将文件的修改，文件的新建，添加到暂存区。
git add -A：将文件的修改，文件的删除，文件的新建，添加到暂存区。
```

## git 变更文件名	

```
git mv 旧文件 新文件
git mv readme readme.md
```

## git log 打印日志

```
gitk //图形界面 ~~~~~~~~~~~~~~~~~~~~~~

git log //打印提交日志
git log --oneline //简洁的打印日志
git log -n4  // 打印最近的 4次提交

git log --all //打印所有分支的提交日志
git log --all --graph //图形化
```

## git 分支

```
git branch -v 查看所有分支

git checkout -b 创建的分支名称 (某个分支 或者 某次commit)  //创建并切换到分支

git branch -D 分支名 //清除分支 
```

## git 备份

```
git clone --bare  file://XXXXXXX 名称     //克隆到本地
```

