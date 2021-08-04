## Git


- [分支](#分支)  
- [添加远程仓库](#添加远程仓库)  
- [常用命令](#常用命令)  


#### 分支
Git 鼓励大量使用分支
- 查看分支  
```shell script
git branch
```

- 创建分支
```shell script
git branch [name]
```

- 切换分支  
```shell script
git checkout [name]
```
或者
```shell script
git switch [name]
```

- 创建+切换分支
```shell script
git checkout -b [name]
```
或者
```shell script
git switch -c [name]
```

- 合并某分支到当前分支
```shell script
git merge [name]
```

- 删除分支
```shell script
git branch -d [name]
```

- 远程跟踪分支
```shell script
git push origin serverfix:awesomebranch  # 将本地的 serverfix 分支推送到远程仓库上的 awesomebranch 分支
git push origin serverfix  # 推送本地的 serverfix 分支来更新远程仓库上的 serverfix 分支
```


#### 添加远程仓库
Git 添加远程仓库的几种方式
- Create a new repository  
```shell script
git clone git@gitee.com:xuelingfei/Study_Notes.git
cd Study_Notes
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

- Existing folder
```shell script
cd existing_folder
git init
git remote add origin git@gitee.com:xuelingfei/Study_Notes.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

- Existing Git repository
```shell script
cd existing_repo
git remote add origin git@gitee.com:xuelingfei/Study_Notes.git
git push -u origin --all
git push -u origin --tags
```


#### 常用命令
- 查看git版本  
```shell script
git --version
```

- 创建SSH Key
```shell script
ssh-keygen -t rsa -C "your_email"
```

- 查看已有的配置信息
```shell script
git config --list
```

- 全局初始化用户名和邮箱
```shell script
git config --global user.name "your_name"
git config --global user.email "your_email"
```

- 修改全局初始化的用户名和邮箱
```shell script
git config --global --replace-all user.name "your_name"
git config --global user.email "your_email"
```

- CRLF 和 LF 转换设置
```shell script
git config --global core.autocrlf true  # 默认值，把 CRLF 转换成 LF
git config --global core.autocrlf input  # 上库转换，从库中迁出代码不转换
git config --global core.autocrlf false  # 不转换，一般用于 Windows
```

- 把当前目录变成 git 可以管理的仓库
```shell script
git init
```

- 把文件修改添加到暂存区
```shell script
git add file
```

- 把暂存区的所有内容提交到当前分支
```shell script
git commit -m "message"
```

- 丢弃工作区的修改  
撤销文件的修改。使用暂存区的替换掉工作区的文件。（让 file 回到最近一次 `git commit` 或 `git add` 时的状态。）
```shell script
git checkout -- file  # "." 代表撤销所有
```

- 查看仓库的当前状态
```shell script
git status
```

- 查看提交历史
```shell script
git log
```

- 单行显示简略提交历史
```shell script
git log --pretty=oneline
```

- 查看命令历史
```shell script
git reflog
```

- 取消暂存的文件
撤销刚才的 add 操作。如果不指定文件名，则撤销 add 的所有文件。
```shell script
git reset HEAD file
```

- 回退到上一版本
```shell script
git reset --hard HEAD^
```

- 回到 commit_id（5~8位）指定的版本
```shell script
git reset --hard commit_id
```

- 本地回退版本后更新远程仓库
```shell script
git push --force
```

- 查看 file 文件工作区和版本库里面最新版本的区别
```shell script
git diff HEAD -- file
```

- 更新本地分支与远程同步
```shell script
git pull -p
```
等同于下面的命令
```shell script
git fetch --prune origin
git fetch -p
```