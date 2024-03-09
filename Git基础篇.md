# Git基础篇

GitHub作为全球最大~~同性交友网站~~软件源代码托管服务平台. 它是建立在Git这个分布式版本控制系统上的. 分布式: 每个客户端都是一个服务器，拥有一个完整地版本库. 版本控制：将代码轻松回退至以前的版本. [Git下载]("https://git-scm.com/downloads")  

## 命名全局用户名和邮箱地址

安装后第一次运行Git Bash先把你的全局用户名和邮箱告诉Git, 这样它才知道是谁提交的代码. 这是全局命名意味着这台机器所有仓库将使用这个身份.

```bash
git config --global user.name 'username'
git config --global user.email 'email'
```

查看用户名和邮箱.

```bash
git config user.name
git config user.email
```

## 初始化仓库

命令进入你的仓库(你要上传的文件夹)中对这个库使用初始化命令.

```bash
git init
```

## 将修改放入暂存区

可提交整个文件夹或单个文件.

```bash
git add .
git add file_name
```

## 提交修改(生成新版本)

将暂存区的所有修改提交生成新版本.

```bash
git commit -m '新建README文件'
```

## 回退版本

```bash
git reset --hard HEAD^
```

>HEAD^ 表示上一个版本, HEAD^^ 表示上两个版本, 以此类推. HEAD~10表示上10个版本. 也可以直接输版本号(只用输版本号前几位即可).

## 查看日志

查看当前状态

```bash
git status
```

查看每一次版本记录.

```bash
git log
```

查看每一次命令记录.

```bash
git reflog
```

## 添加远程仓库

```bash
git remote add origin 仓库地址
```

## 推送本地仓库到远程仓库

```bash
git push -u origin master:master #推送本地master分支到远程master分支且将二者关联
git push --all origin #推送所有分支
```

>前一个master表示本地分支名, 后一个master表示远程分支名.

## 复制远程仓库到本地

```bash
git clone 仓库地址
```

## 拉取远程仓库

常用pull命令获取更新.

```bash
git pull origin master:master #将远程更新拉取到本地并且与当前分支合并
```

## 查看分支

```bash
git branch -a #查看所有分支
git log --graph #查看分支合并图
```

## 创建分支

```bash
git branch branch_name
```

## 切换分支

```bash
git switch branch_name #切换分支
git switch -c branch_name #新建分支并切换到该分支上
```

## 删除分支

```bash
git branch -d branch_name #删除分支
git branch -D branch_name #强行删除分支
```

## 预览分支的差异

```bash
git diff source_branch_name target_branch_name
```

## 合并分支

```bash
git merge --no-ff -m "合并分支" branch_name #禁用fastforward, 将brach_name合并到当前分支
```

## 新建远程分支

```bash
git push origin dev:dev
```

## 删除远程分支

```bash
git push origin :master #推送空分支即删除对应远程分支
git push origin --delete master #删除远程分支
```

## 查看标签

```bash
git tag
```

## 创建标签

```bash
git tag tag_name #标签放在当前版本下
git tag tag_name commit_id #标签放在对应版本号的版本下
git tag -a tag_name -m '标签初始化' #新建标签时附加备注
```

## 删除标签

```bash
git tag -d #删除标签
```

## 推送标签

```bash
git push origin tag_name #推送标签到远程
git push origin --tags #推送所有标签到远程
```
