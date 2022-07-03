# 【Git】

## Git简介

> **Git是一个免费的、开源的分布式版本控制系统** ，可以快速高效地处理从小型到大型的各种项目
>
> - Git Bash : Unix和Linux风格的命令行，使用最多，推荐最多
> - Git CMD：Windows风格的命令行
> - Git GUI：图形化命令行，不建议使用
>
> **版本控制：**
>
> | 版本 | 文件名      | 用户 | 说明                   | 日期       |
> | :--- | :---------- | :--- | :--------------------- | :--------- |
> | 1    | service.doc | 张三 | 删除了软件服务条款5    | 7/12 10:38 |
> | 2    | service.doc | 张三 | 增加了License人数限制  | 7/12 18:09 |
> | 3    | service.doc | 李四 | 财务部门调整了合同金额 | 7/13 9:51  |
> | 4    | service.doc | 张三 | 延长了免费升级周期     | 7/14 15:17 |
>

### 版本控制

> 版本控制（Revision control）是一种在开发过程中用于管理我们对文件、目录或工程等内容的修改历史，方便查看更改历史记录，备份以便恢复以前的版本的软件工程技术。
>
> - 实现跨区域多人协同开发
> - 追踪和记载一个或者多个文件的历史记录
> - 组织和保护你的源代码和文档
> - 统计工作量
> - 并行开发、提高开发效率
> - 追踪记录整个软件的开发过程
> - 减轻开发人员的负担，节省时间，同时降低人为错误
>

简单来说就是用于管理多人协同开发项目的技术

没有进行版本控制或版本控制本身缺乏正确的流程管理，在软件开发过程中会引入许多问题，比如软件代码的一致性、软件内容的冗余、软件过程的事物性、软件开发过程中的并发性、软件源代码的安全性，以及软件的整合问题。

#### 常见的版本控制器

主流的版本控制器有如下这些：

- **Git**
- **SVN**（Subversion）
- **CVS**（Concurrent Versions System）
- **VSS**（Micorosoft Visual SourceSafe）
- **TFS**（Team Foundation Server）
- **Visual Studio Online**

版本控制产品非常的多，现在影响力最大且使用最广泛的是Git与SVN。

#### 版本控制分类

**1.本地版本控制**

记录文件每次的更新，对每个版本做一个快照或是记录补丁文件，适合个人用。

**2.集中版本控制**

所有的版本数据都保存在一个中央服务器上，协同开发者从服务器上同步更新或上传自己的修改。

所有的版本数据都存在服务器上，用户的本地只有自己以前所同步的版本，如果不连网的话，用户就看不到历史版本，也无法切换版本验证问题，或在不同分支工作。而且，所有数据都保存在单一的服务器上，有很大的风险：如果这个服务器会损坏，这样就会丢失所有的数据。

**3.分布式版本控制**

所有版本信息仓库全部同步到本地的每个用户，这样就可以在本地查看所有版本历史，可以离线在本地提交，只需在联网时推送到相应的服务器或其他用户那里。由于每个用户那里保存的都是所有的版本数据，只要有一个用户的设备没有问题就可以恢复所有的数据，但这增加了本地存储空间的占用。

不会因为服务器损坏或网络问题，造成不能工作的情况！

#### Git与SVN的主要区别

**SVN是集中式版本控制系统，版本库是集中放在中央服务器的。** 

工作的时候，用的都是自己的电脑，所以首先要从中央服务器得到最新的版本，然后工作。完成工作后，需要把自己做完的活推送到中央服务器。集

中式版本控制系统是必须联网才能工作，对网络带宽要求较高。

**Git是分布式版本控制系统，没有中央服务器。** 

每个人的电脑就是一个完整的版本库，工作的时候不需要联网了，因为版本都在自己电脑上。

协同的方法是这样的：比如说自己在电脑上改了文件A，其他人也在电脑上改了文件A，这时，你们两之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。Git可以直接看到更新了哪些代码和文件。

### 工作机制

**Git有三个分区：本地库、暂存区和工作区**

![image-20220702063423364](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20220702063423364.png)

![image-20220702064104923](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20220702064104923.png)

- **工作区（Working Directory）**

  是我们直接编辑的地方，例如IDEA正在编写的代码、记事本打开的文本等，肉眼可见，直接操作。

- **暂存区（Stage 或 Index）**

  数据暂时存放的区域，可在工作区和版本库之间进行数据的交流，工作区的代码需要先添加到暂存区。

- **本体库（commit History）**

  存放已经提交的数据，push的时候，就是把这个区的数据 push到远程仓库了。

**git的工作流程：**

1. 在工作区中增删、修改文件；
2. 将需要进行版本管理的文件放入暂存区域；
3. 将暂存区域的文件提交到git仓库。

因此，git管理的文件有三种状态：已修改（modified）,已暂存（staged）,已提交(committed)

**Git中无法单独删除一个版本的代码，只能基于上一个版本提交一个删除所需代码的新版本**

### 代码托管中心

> 代码托管中心是基于网络服务器的代码远程仓库，我们一般简单称为远程库

 **代码托管中心分类：**

- **局域网**

  GitLab

- **互联网**

  GitHub（外网）

  Gitee 码云（国内网站）

## 安装

> 官网地址： https://git-scm.com/

## 常用命令

### 命令列表

|                  命令                   |      作用      |
| :-------------------------------------: | :------------: |
| `git config --global user.name 用户名 ` |  设置用户签名  |
|  `git config --global user.email 邮箱`  |  设置用户签名  |
|               `git init `               |  初始化本地库  |
|              `git status`               | 查看本地库状态 |
|            `git add 文件名`             |  添加到暂存区  |
|    `git commit -m "日志信息" 文件名`    |  提交到本地库  |
|              `git reflog`               |  查看历史记录  |

### 设置用户签名

**语法：**

```bash
git config --global user.name 用户名

git config --global user.email 邮箱
```

**说明：**

签名的作用是区分不同操作者身份。用户的签名信息在每一个版本的提交信息中能够看到，以此确认本次提交是谁做的。

Git 首次安装必须设置一下用户签名，否则无法提交代码。

**注意：** 设置用户签名和将来登录 GitHub（或其他代码托管中心）的账号没有任何关系。

```txt
[user]
	name = Chongming
	email = shuejian2190427@163.com
[core]
	autocrlf = true
```

### 初始化本地库

> 初始化本地库，即将当前文件夹交给git管理

**语法：** 

```bash
# 在需要初始化的文件夹下执行
git init	
```

初始化后，会在当前目录下生成一个隐藏目录：`.git`

### 查看本地库状态

**语法：** 

```bash
# 在需要查看的本地库下执行
git status
```

- **首次查看（工作区无文件）：**

```ba
$ git status
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

### 添加到暂存区

> 将工作区的文件添加到暂存区

**语法：**

```bash
git add 文件名
```

- **工作区有文件未添加到暂存区时：**

```bash
$ git status
On branch master
No commits yet
# 提示有未被追踪的文件
Untracked files:
 (use "git add <file>..." to include in what will be committed)
 hello.txt
nothing added to commit but untracked files present (use "git add" 
to track)
```

- **暂存区有新文件未被提交：**

```bash
$ git status
On branch master
No commits yet
# 提示有更改未被提交
Changes to be committed:
# 提示删除暂存区文件的命令如下：
 (use "git rm --cached <file>..." to unstage)
 new file: hello.txt
```

- **工作区有文件被修改：**

```bash
On branch master
# 提示修改未被添加到暂存区
Changes not staged for commit:
 (use "git add <file>..." to update what will be committed)
 # git checkout -- 文件名 可以放弃工作区的修改，让工作区的文件回到最近一次add或commit的状态
 (use "git checkout -- <file>..." to discard changes in working 
directory)
 modified: hello.txt
no changes added to commit (use "git add" and/or "git commit -a")
```

### 提交本地库

**语法：**

```bash
git commit -m "日志信息" 文件名
```

- **成功提交后**

```bash
[master (root-commit) 33bb3d1] practice o be committed:
 1 file changed, 1 insertion(+)
 create mode 100644 heelo.txt
```

### 查看历史版本

**语法：**

```bash
git reflog 查看版本信息
git log 查看版本详细信息
```

**效果：**

```bash
$ git reflog
# 开头的代码即版本号，(HEAD -> master)指向当前版本
8ca5c3b (HEAD -> master) HEAD@{0}: commit: second
33bb3d1 HEAD@{1}: commit (initial): practice
```

### 版本穿梭

**语法：**

```bash
git reset --hard 版本号
```

**效果：**

```bash
--首先查看当前的历史记录，可以看到当前是在 8ca5c3b 这个版本
$ git reflog
8ca5c3b (HEAD -> master) HEAD@{0}: commit: second
33bb3d1 HEAD@{1}: commit (initial): practice

--切换到 33bb3d1 版本，也就是第一次提交的版本
$ git reset --hard 33bb3d1
HEAD is now at 33bb3d1 practice o be committed:


--切换完毕之后再查看历史记录，当前成功切换到了 86366fa 版本
$ git reflog
33bb3d1 (HEAD -> master) HEAD@{0}: reset: moving to 33bb3d1
8ca5c3b HEAD@{1}: commit: second
33bb3d1 (HEAD -> master) HEAD@{2}: commit (initial): practice
```

## 忽略文件

有些某些文件不需要纳入版本控制中，比如数据库文件，临时文件，设计文件等

可以在主目录下建立".gitignore"文件，此文件有如下规则：

1. 忽略文件中的空行或以井号（#）开始的行。
2. 可以使用Linux通配符。例如：
   - 星号（*）代表任意多个字符
   - 问号（？）代表一个字符
   - 方括号（[abc]）代表可选字符范围
   - 大括号（{string1,string2,...}）代表可选的字符串等。

3. 如果名称的最前面有一个感叹号（!），表示例外规则，将不被忽略。
4. 如果名称的最前面是一个路径分隔符（/），表示要忽略的文件在此目录下。
5. 如果名称的最后面是一个路径分隔符（/），表示要忽略的是此目录下该名称的子目录，而非文件（默认文件或目录都忽略）。

```bash
# 为注释
*.txt        # 忽略所有.txt结尾的文件
!lib.txt     # lib.txt除外
/temp        # 仅忽略项目根目录下的temp文件
build/       # 忽略build/目录下的所有文件
doc/*.txt    # 会忽略doc目录下所有的.txt结尾的文件
```

## 分支

### 简介

> ![image-20220702073233862](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20220702073233862.png)
>
> 在版本控制过程中，同时推进多个任务，可以为每个任务创建单独分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时候，不会影响主线分支的运行。
>
> 分支底层其实也是指针的引用。
>
> 分支的好处：
>
> - 同时并行推进多个功能的开发，提高开发效率
> - 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支造成任何影响，把失败的分支删除重新开始即可。

### 操作

|         命令          |             作用             |
| :-------------------: | :--------------------------: |
|  `git branch 分支名`  |           创建分支           |
|    `git branch -v`    |           查看分支           |
| `git checkout 分支名` |           切换分支           |
|  `git merge 分支名`   | 把指定的分支合并到当前分支上 |

### 合并分支

**语法：**

```bash
# 在当前分支的基础上合并另一个分支
git merge 要合并的分支名
```

**冲突：**

合并分支时，两个分支在同一个文件的同一个位置有两套完全不同的修改，Git 无法替我们决定使用哪一个，必须人为决定新代码内容。

**产生冲突：**

```bash
$ git merge hot-fix
Auto-merging heelo.txt
CONFLICT (content): Merge conflict in heelo.txt
Automatic merge failed; fix conflicts and then commit the result.

# 假如当前分支有冲突：
lenovo@DESKTOP-GPUT1FD MINGW64 ~/Desktop/git练习 (master|MERGING)
```

**解决冲突：前往有冲突的文件进行修改**

```bash
# git会自动标识出有冲突的代码
<<<<<<< HEAD
当前分支的代码
=======
要合并分支的代码
>>>>>>> 分支名

# 示例：
<<<<<<< HEAD
holle warlb！
是单都怕我
=======
hello world！
砸瓦鲁多
>>>>>>> hot-fix
```

**解决冲突后需要重新添加到暂存区并提交**

## 远程仓库

### 命令列表

|                 命令                 |                           作用                           |
| :----------------------------------: | :------------------------------------------------------: |
|           `git remote -v`            |                 查看当前所有远程地址别名                 |
|    `git remote add 别名 远程地址`    |                     给远程地址起别名                     |
|         `git push 别名 分支`         |              推送本地分支上的内容到远程仓库              |
|         `git clone 远程地址`         |                    克隆远程仓库到本地                    |
| `git pull 远程库地址别名 远程分支名` | 将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并 |

### 添加远程库

**语法：**

```bash
# 添加到远程库，并给远程地址起别名
git remote add 别名 远程地址

# 查看当前所有远程地址别名
git remote -v
```

**git默认别名是origin**

### 移除远程库

**语法：**

```bash
 git remote rm 远程地址（别名）
```

### 推送本地库

**语法：**

```bash
git push 别名 分支
```

### 克隆远程库

**语法：**

```bash
git clone 远程地址
```

### 拉取远程库

**语法：**

```bash
git pull 别名  
```



# 【GitHub】



# 【Gitee】

# 【Gitlab】
