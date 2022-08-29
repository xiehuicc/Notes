**创建版本库**

版本库又名仓库，英文名**repository**，可以简单理解成一个目录

第一步 创建一个目录

```bash
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
```

第二步，通过`git init`命令把这个目录变成Git可以管理的仓库

```bash
$ git init
Initialized empty Git repository in /Users/michael/learngit/.git/
```



添加文件到Git仓库，分两步：

1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
2. 使用命令`git commit -m <message>`，完成。

### 时光机穿梭

#### 版本控制

`git log`命令查看提交的历史记录，以便确定要回退到哪个版本。

`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

回退到上一版本

```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```

上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`

也可以指定commit 选择回退的版本

```bash
 git reset --hard 1094a
 // commit id不必写全，可以git log 看一下提交记录
```

当你回退版本的时候，Git仅仅是把HEAD从指向`append GPL`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   └──> ○ append GPL
        │
        ○ add distributed
        │
        ○ wrote a readme file
```



#### 工作区和暂存区

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

`git status`查看一下提交的状态：



#### 管理修改

> Git跟踪并管理的是修改，而非文件。
>
> 比如你新增了一行，这就是一个修改，，更改了某些字符，也是一个修改...

`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别：

`git checkout -- file`可以丢弃工作区的修改：

```
$ git checkout -- readme.txt
```

`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：(用`HEAD`时，表示最新的版本。)

```bash
$ git reset HEAD readme.txt
Unstaged changes after reset:
M	readme.txt
```

### 远程仓库

#### 添加远程仓库

```
 git remote add origin 仓库地址
```

下一步，就可以把本地库的所有内容推送到远程库上：

```bash
$ git push -u origin master
```



### 分支管理

#### 创建与合并分支

切换分支 

`git checkout`命令加上`-b`参数表示创建并切换，

```
$ git checkout -b dev
```

`git checkout` 查看分支



把`dev`分支的工作成果合并到`master`分支上：

```bash
$ git merge dev
// 此时是在master上
```

删除分支

```bash
$ git branch -d <name>
```

#### `.gitignore`

- 匹配模式前`/`代表项目根目录
- 匹配模式最后加`/`代表目录
- 匹配模式前加`!`代表取反
- `*`代表任意个字符
- `?`匹配任意一个字符
- `**`匹配多级目录

```.gitigore
# 把node_modules下面的（任意级目录）所有index.js文件全部忽略
node_modules/**/index.js 
# 忽略任意swp文件
*.swp
#


```

#### `.npmignore`

> 忽略`npm`包中的东西



### 常用指令

- git log ：查看提交记录

  - git log --stat  查看提交记录，包括了提交的 哪些文件记录

    