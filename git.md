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
git init //创建
git status //查看git状态

git add '文件名'  //将某个文件存到 暂存区
git add . //全部存到 暂存区 

```

