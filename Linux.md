> Linux系统简介 (biancheng.net)](http://c.biancheng.net/linux_tutorial/10/)
>
> 系统：CentOS 7

# 【开关机】

## 开机

开机会启动许多程序。它们在Windows叫做"服务"（service），在Linux就叫做"守护进程"（daemon）。

开机成功后，它会显示一个文本登录界面，这个界面就是登录界面。

登录界面中会提示用户输入用户名，而用户输入的用户将作为参数传给login程序来验证用户的身份，密码是不显示的，输完回车即可。

**一般来说，用户的登录方式有三种：**

- 命令行登录
- ssh登录
- 图形界面登录

最高权限账户为 root，可以操作一切

## 关机

在linux领域内大多用在服务器上，很少遇到关机的操作。毕竟服务器上跑一个服务是永无止境的，除非特殊情况下，不得已才会关机。

关机指令为：shutdown

不管是重启系统还是关闭系统，首先要运行 sync 命令，把内存中的数据写到磁盘中。

```shell
sync # 将数据由内存同步到硬盘中。

shutdown # 关机指令，你可以man shutdown 来看一下帮助文档。例如你可以运行如下命令关机：

shutdown –h 10 # 这个命令告诉大家，计算机将在10分钟后关机

shutdown –h now # 立马关机

shutdown –h 20:25 # 系统会在今天20:25关机

shutdown –h +10 # 十分钟后关机

shutdown –r now # 系统立马重启

shutdown –r +10 # 系统十分钟后重启

reboot # 就是重启，等同于 shutdown –r now

halt # 关闭系统，等同于shutdown –h now 和 poweroff
```

**ps:在linux系统中，没有错误即成功**

# 【目录结构】

> Linux目录结构是一个树形结构
>
> Windows系统可以拥有多个盘符，如 C盘、D盘、E盘
>
> Linux没有盘符的概念，只有一个根目录`/`
>
> **在Linux中：**
>
> - 一切皆文件
> - 根目录`/`，所有的文件都挂载在这个节点下
>
> **Linux路径描述方式：**
>
> - Linux中采用`/`表示
> - Windows采用`\`表示

登录系统后，在当前命令窗口下输入命令：

```shell
ls /
```

得到：

![image-20210722194658734](https://gitee.com/dong-dameng/images/raw/master/img/image-20210722194658734.png)

树状目录结构：Linux的一切资源都挂载在这个`/`根节点下

![640](https://gitee.com/dong-dameng/images/raw/master/img/640.png)

**以下是对这些目录的解释：**

|     目录      |                             解释                             |
| :-----------: | :----------------------------------------------------------: |
|    `/bin`     |      bin是Binary的缩写, 这个目录存放着最经常使用的命令       |
|    `/boot`    | 这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。 |
|    `/dev`     | ev是Device(设备)的缩写, 存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。 |
|    `/etc`     | 这个目录用来存放所有的系统管理所需要的配置文件和子目录。比如Redis的配置文件等 |
|    `/home`    | 用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的 |
|    `/lib`     | 这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。 |
| `/lost+found` | 这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件（存放突然关机的文件） |
|               |                                                              |
|               |                                                              |
|               |                                                              |
|               |                                                              |
|               |                                                              |
|               |                                                              |
|               |                                                              |

- **/media**：linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。

- **/mnt**：系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。

  我跟会把一些本地的文件挂载在这个目录下

- **/opt：这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。**

- **/proc**：这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息（不用管）。

- **/root：该目录为系统管理员，也称作超级权限者的用户主目录。**

- **/sbin**：Super User的意思，这里存放的是系统管理员使用的系统管理程序。

- **/srv**：该目录存放一些服务启动之后需要提取的数据。

- **/sys**：这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sysfs 。

- **/tmp：这个目录是用来存放一些临时文件的。用完即丢的文件可以放在这个目录下**

- **/usr：这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录。**

- **/usr/bin：** 系统用户使用的应用程序。

- **/usr/sbin：** 超级用户使用的比较高级的管理程序和系统守护程序。

- **/usr/src：** 内核源代码默认的放置目录。

- **/var：这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。**

- **/run**：是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。

- **/www：存放服务器网站相关的资源，环境**

# 【中文支持】

1. **查看当前系统语言：**

   ```shell
   echo $LANG
   ```

2. **查看安装的语言包：**

   查看是否有中文语言包可以在终端输入**locale**命令，如有**zh_cn**表示已经安装了中文语言。

   ```shell
   [root@chongming ~]# locale
   
   LANG=en_US.UTF-8
   LC_CTYPE="en_US.UTF-8"
   LC_NUMERIC="en_US.UTF-8"
   LC_TIME="en_US.UTF-8"
   LC_COLLATE="en_US.UTF-8"
   LC_MONETARY="en_US.UTF-8"
   LC_MESSAGES="en_US.UTF-8"
   LC_PAPER="en_US.UTF-8"
   LC_NAME="en_US.UTF-8"
   LC_ADDRESS="en_US.UTF-8"
   LC_TELEPHONE="en_US.UTF-8"
   LC_MEASUREMENT="en_US.UTF-8"
   LC_IDENTIFICATION="en_US.UTF-8"
   LC_ALL=
   ```

3. **安装中文语言包**

   ```shell
   yum install kde-l10n-Chinese
   yum reinstall glibc-common
   ```

4. **修改配置文件**

   1. **临时生效**

      在当前会话状态下，可以输入`export LANG=en.US`来改变语言，修改后即时生效，但如果断开会话，下次登录时又会恢复到之前的语言集。

   2. **永久生效**

      要达到会话断开会也改变语言集，就要修改全局变量了，修改**etc**目录下的**locale.conf**,修改为`LANG="zh_CN.UTF-8"`

      

# 【基本命令】

> **命令行：**即Linux终端（Terminal），是一种命令提示符页面。以纯“字符”形式操作系统，可以使用各种字符化命令对系统发出操作指令。
>
> **命令：**即Linux程序。一个命令就是一个Linux程序。命令没有图形化界面，可以再命令行（终端）中获得字符化反馈。
>
> 可以使用 *man [命令]* 来查看各个命令的使用文档，如 ：`man cp`。

## 通用格式

**命令通用格式：**

```shell
command [-options] [parameter]
```

- `command`：命令本身
- `-options`：[可选，非必填]，命令的一些选项，可以通过选项控制命令的行为细节
- `parameter`：[可选，非必填]，命令的参数，多数用于命令的指向目标等

## 目录/文件管理

> - **绝对路径：**以根目录为起点
>
> 路径的写法，由根目录 / 写起，例如：/usr/share/doc 这个目录。
>
> - **相对路径：**以当前目录为起点
>
> 路径的写法，不是由 / 写起，例如由 /usr/share/doc 要到 /usr/share/man 底下时，可以写成：cd ../man 这就是相对路径的写法
>
> **Xshell7中，白色代表文件，蓝色代表目录，青色代表软链接**
>
> `clear`：清屏

**常用命令：**

- `ls`: 列出目录

  ```shell
  ls [-a -l -h] [Linux目录]
  ```

  - `-a`：all，查看全部的文件，包括隐藏文件（隐藏文件以`.`开头）

  - `-l`：以列表形式列出所有的文件，包含文件的属性和权限，不包括隐藏文件
  - `-h`：以易于阅读的方式列出带单位的存储大小，需要和`-l`一起使用

  - 参数可以组合使用，比如`ls -a -l`或`ls -la`

  - 不使用选项和参数，表示以平铺形式列出当前工作目录下的内容

    

- `cd`：切换目录（绝对路径都是以/开头）

    Change Directory

    ```shell
    cd [Linux路径]
    # 不写参数，表示回到用户的家目录
    ```

    - `/`：根目录
    - `./`：当前目录
    - `../`：上一级目录
    - `~`：当前用户的家目录
        - 对于root用户，`cd ~`相当于`cd /root`
        - 对于普通用户，`cd ~`相当于`cd /home/当前用户名`



- `pwd`：显示当前所在的目录

    Print Work Directory

    

- `mkdir`：创建一个新的目录

    ```shell
    mkdir [-p] Linux路径
    ```

    Make Directory

    - `-p`：递归创建多级目录（即父目录不存在会自动创建父目录），如`mkdir -p /test/test1`



- `rmdir`：删除一个空的目录

    ```shell
    rmdir [-p] Linux路径
    ```

    Remove Directory

    - `-p`：递归删除多级目录，如`rmdir -p /test/test1`
    - 仅能删除空的目录，如果下面存在文件，需要先删除文件



- `touch`：创建文件

    ```shell
    touch Linux路径
    ```

    touch命令参数表示要创建的文件路径



- `cp`: 复制文件或目录:

    Copy

    ```shell
    cp [-r] 参数1(old路径) 参数2(new路径)
    ```

    - `-r`：表示可以复制文件夹
    - 如果文件重复，选择覆盖( y )或放弃( n )



- `rm`: 移除文件或目录

    ```shell
    rm [-fri] 目标文件/目录
    ```

    - `-f`：不会出现警告，强制删除
    - `-r` ：递归删除目录
    - `-i`：互动，删除询问是否删除
    - `rm -rf /`：**核弹！！！删除所有文件，删库跑路备用**

    - **通配符：**模糊匹配

      - `test*`：匹配任何以test开头的内容
      - `*test`：匹配任何以test结尾的内容
      - `*test*`：匹配任何包含test的内容

      

- `mv`: 移动文件与目录，或修改文件与目录的名称

  Move

  - `-f`：强制移动

  - `-u`：只替换已经更新过的文件

  - **如果新目录不存在，则执行重命名操作**

    两个文件执行重命名操作，比如`mv test0 test/test1`

    test目录存在，但test1目录不存在，则移动test0到test，并重命名为test1


## 文件查看

Linux系统中使用以下命令来查看文件的内容：

- `cat` 由第一行开始显示文件内容
- `tac` 从最后一行开始显示，可以看出 tac 是 cat 的倒着写！
- `nl`  显示的时候，顺道输出行号！
- `more` 一页一页的显示文件内容
- `less` 与 `more` 类似，但是比 more 更好的是，他可以往前翻页！
- `head` 只看头几行
- `tail` 只看尾巴几行

可以使用 *man [命令]*来查看各个命令的使用文档，如 ：man cp。

### cat

```shell
cat [-AbEnTv]
```

选项与参数：

- `-A` ：相当於 -vET 的整合选项，可列出一些特殊字符而不是空白而已；
- `-b` ：列出行号，仅针对非空白行做行号显示，空白行不标行号！
- `-E` ：将结尾的断行字节 $ 显示出来；
- `-n` ：列印出行号，连同空白行也会有行号，与 -b 的选项不同；
- `-T` ：将 [tab] 按键以 ^I 显示出来；
- `-v` ：列出一些看不出来的特殊字符

测试：

```shell
# 查看网络配置: 文件地址 /etc/sysconfig/network-scripts/
[root@iZuf6hnmsxi97or8gnhw1gZ ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0
BOOTPROTO=dhcp
ONBOOT=yes
```

### tac

tac与cat命令刚好相反，文件内容从最后一行开始显示，tac 也恰好是cat的倒着写

### nl

语法：

```shell
nl [-bnw] 文件
```

选项与参数：

- `-b` ：指定行号指定的方式，主要有两种：
  - `-b a` ：表示不论是否为空行，也同样列出行号(类似 cat -n)；
  - `-b t` ：如果有空行，空的那一行不要列出行号(默认值)；

- `-n` ：列出行号表示的方法，主要有三种：
  - `-n ln` ：行号在荧幕的最左方显示；
  - `-n rn` ：行号在自己栏位的最右方显示，且不加 0；
  - `-n rz` ：行号在自己栏位的最右方显示，且加 0 ；

- -w ：行号栏位的占用的位数。

### `more`

在 more 这个程序的运行过程中，你有几个按键可以按的：

- 空白键 (space)：代表向下翻一页；
- Enter ：代表向下翻『一行』；
- /字串 ：代表在这个显示的内容当中，向下搜寻『字串』这个关键字；
- :f ：立刻显示出档名以及目前显示的行数；
- q ：代表立刻离开 more ，不再显示该文件内容。
- b 或 [ctrl]-b ：代表往回翻页，不过这动作只对文件有用，对管线无用。

```shell
[root@iZuf6hnmsxi97or8gnhw1gZ ~]# more /etc/csh.login
....(中间省略)....
--More--(28%) # 光标也会在这里等待命令
```

### `less`

less运行时可以输入的命令有：

- 空白键 ：向下翻动一页；
- [pagedown]：向下翻动一页；
- [pageup] ：向上翻动一页；
- /字串 ：向下搜寻『字串』的功能；
- ?字串 ：向上搜寻『字串』的功能；
- n ：重复前一个搜寻 (与 / 或 ? 有关！)
- N ：反向的重复前一个搜寻 (与 / 或 ? 有关！)
- q ：离开 less 这个程序；

```shell
[root@iZuf6hnmsxi97or8gnhw1gZ ~]# more /etc/csh.login
....(中间省略)....
:   # 这里可以等待你输入命令！
```

### `head`

语法：

```shell
head [-n number] 文件
```

选项与参数：**-n** 后面接数字，代表显示几行的意思！

默认的情况中，显示前面 10 行！若要显示前 20 行，就得要这样：

```shell
[root@iZuf6hnmsxi97or8gnhw1gZ ~]# head -n 20 /etc/csh.login
```

### `tail`

语法：

```shell
tail [-n number] 文件
```

选项与参数：

- -n ：后面接数字，代表显示几行的意思

默认的情况中，显示最后 10 行！若要显示最后 20 行，就得要这样：

```shell
[root@iZuf6hnmsxi97or8gnhw1gZ ~]# tail -n 20 /etc/csh.login
```

## 查找路径

### 查找命令

> Linux命令本体就是一个个二进制可执行程序，与windows系统中的.exe文件是一个意思
>
> 可以通过which命令查看这些命令的程序文件存放在哪里

```shell
which 要查找的命令
```

### 查找文件

- **按照文件名查找**
  
  ```shell
  find 起始路径 -name "被查找的文件名"
  ```

  ![image-20230516145012627](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230516145012627.png)

  - 支持通配符`*`进行模糊匹配
  
- **按照文件大小查找**
  
  ```shell
  # -n[kMG]也可以
  find 起始路径 -size +n[kMG]
  ```
  
  - `+` `-`表示大于和小于
  - `n`表示大小数字
  - kMG表示大小单位，即kb、mb、gb（注意k是小写）

## 查找内容

### 查找-grep

> grep命令，从文件中通过关键字过滤文件行
>
> 查找该关键字在文件的第几行

```shell
grep [-n] 关键字 文件路径
```

- 选项`-n`，可选，表示在结果中显示匹配的行的行号
- 参数，关键字，必填，表示过滤的关键字，带有空格或其他特殊符号，建议使用`""`将关键字包围起来
- 参数，文件路径，必填，表示要过滤内容的文件路径，**可作为管道符的内容输入端口**

![image-20230516150423192](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230516150423192.png)

### 统计-wc

> 可以通过wc命令统计文件的行数、单词数量等
>
> 文件路径同样可以作为管道符的内容输入端口

```shell
wc [-c -m -l -w] 文件路径
```

- -c，统计bytes数量
- -m，统计字符数量
- -l，统计行数
- -w，统计单词数量

## 管道符

**管道符：`|`**

**含义：将管道符左边命令的结果，作为右边命令的输入**

只要能产生内容输出的命令，其执行结果都可以作为命令结果

**举例：**

```shell
grep [-n] 关键字 文件路径
# 文件路径位置，可以作为内容输入端口
# 所以可以执行以下命令
cat test | grep -n "could"
```

![image-20230516151217116](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230516151217116.png)

## 输出-echo

> 可以使用echo命令在命令行中输出指定内容
>
> 好比c的print函数

```shell
echo 输出的内容
```

输出内容比较复杂的话建议用双引号包裹

## 反引号

被**\`\`**包围的字符会作为命令执行

![image-20230516152313906](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230516152313906.png)

## 重定向符

- `>`：将左侧命令的结果，覆盖写入到符号右侧指定的文件中

  ![image-20230516152536656](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230516152536656.png)

- `>>`：将左侧命令的结果，追加写入到符号右侧指定的文件中

![image-20230516152555227](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230516152555227.png)

## 跟踪-tail

> 使用tail命令，可以查看文件尾部内容，跟踪文件的更改
>
> 可以用来跟踪事实日志

```shell
tail [-f -num] Linux路径
```

- 参数，Linux命令，表示被跟踪的文件路径
- 选项，-f，表示持续跟踪
- 选项，-num，表示查看尾部多少行，这里的num指的是具体的数字
- `ctrl+c`快捷键停止追踪

# 【vim编辑器】

## 简介

vim是从 vi 发展出来的一个文本编辑器。代码补完、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。

简单的来说， vi 是老式的字处理器，不过功能已经很齐全了，但是还是有可以进步的地方。

vim 则可以说是程序开发者的一项很好用的工具。

所有的 Unix Like 系统都会内建 vi 文书编辑器，其他的文书编辑器则不一定会存在。

vim 的官方网站 (http://www.vim.org) 自己也说 vim 是一个程序开发工具而不是文字处理软件。

- 

## 使用模式

```shell
vim filename # 打开/新建一个文件
```

如果文件不存在，则会创建一个新文件用来编辑

**vim三种工作模式：**

![image-20230516153748785](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230516153748785.png)

- **命令模式：**

  命令模式下，所敲的按键都被理解为命令，以命令驱动执行不同的功能

- **输入模式：**

  编辑模式，此模式下可以对文件内容进行自由编辑

- **底线命令模式：**

  以`:`开始，通常用于文件的保存、退出

## 命令模式

用户刚刚启动 vi/vim，便进入了命令模式。

此状态下敲击键盘动作会被vim识别为命令，而非输入字符。比如我们此时按下i，并不会输入一个字符，i被当作了一个命令。

以下是常用的几个命令：

- `i` 切换到输入模式，以输入字符。
- `x` 删除当前光标所在处的字符。
- `:` 切换到底线命令模式，以在最底一行输入命令。
- `ESC`退出编辑模式，进入命令模式

若想要编辑文本：启动vim，进入了命令模式，按下i，切换到输入模式。

命令模式只有一些最基本的命令，因此仍要依靠底线命令模式输入更多命令。

## 输入模式

在命令模式下按下i就进入了输入模式。

在输入模式中，可以使用以下按键：

- **字符按键以及Shift组合**，输入字符
- **ENTER**，回车键，换行
- **BACK SPACE**，退格键，删除光标前一个字符
- **DEL**，删除键，删除光标后一个字符
- **方向键**，在文本中移动光标
- **HOME**/**END**，移动光标到行首/行尾
- **Page Up**/**Page Down**，上/下翻页
- **Insert**，切换光标为输入/替换模式，光标将变成竖线/下划线
- **ESC**，退出输入模式，切换到命令模式

## 底线命令模式

在命令模式下按下:（英文冒号）就进入了底线命令模式。

底线命令模式可以输入单个或多个字符的命令，可用的命令非常多。

在底线命令模式中，基本的命令有（已经省略了冒号）：

- `q` 退出程序
- `w` 保存文件
- `wq`保存并退出

按`ESC`键可随时退出底线命令模式。

# 【用户/权限】

## root管理员

> root用户拥有最大的系统操作权限，而普通用户在许多地方的权限时受限的
>
> 普通用户的权限，一般在其home目录内不受限，一旦出了home目录，大多数地方，普通用户今有只读和执行权限，无修改权限

## 切换用户-su

```shell
su [-] [用户名]
```

- `-`符号可选，表示是否在切换用户后加载环境变量，建议带上
- 参数：用户名，表示要切换的用户，用户名省略表示切换到root
- 切换用户后，可以通过exit命令退回上一个用户，也可以使用快捷键ctrl+d

- 普通用户切换到其他用户需要输入密码

  root用户切换到其他用户不需要输入密码

## 临时权限-sudo

```shell
sudo 命令
```

- sudo可以为普通命令授权，临时以root身份执行该命令
- 并不是所有用户都有使用sudo的权限，需要为普通用户给分配sudo认证

**配置sudo认证：**

- 切换到root用户，执行visudo命令

- vi编辑器会打开：/etc/sudoers

- 在文件最后添加

  ```shell
  用户名 ALL=(ALL) NOPASSWD:ALL
  # 取消认证只需要删除此行
  ```

- 通过:wq保存

- 切换回普通用户

- 执行sudo命令

## 用户/用户组

> Linux系统中可以：
>
> - 配置多个用户
> - 配置多个用户组
> - 用户可以加入多个用户组
>
> **Linux中关于权限的管控级别有2个级别：**
>
> - 针对用户的权限控制
> - 针对用户组的权限控制
>
> 针对某文件，可以控制用户的权限，也可以控制用户组的权限

- `getent passwd`命令查看当前系统有哪些用户

  结果信息：

  用户名：密码(x)：用户ID：组ID：描述信息（无用）：HOME目录：执行终端（默认bash）

- `getent group`命令查看当前系统中有哪些用户组

  结果信息：

  组名称：组认证(显示为x)：组id：用户

### 用户

- **创建用户（root权限）：**

  ```shell
  # 在usr下生成一个对应用户的文件夹
  useradd 用户名
  # 进入账号目录
  cd /usr/用户名/
  # 修改用户密码
  passwd 用户名
  # 输入两次密码设定密码
  ```

  `useradd [-g -d] 用户名`

  - -g：加入指定用户组，默认加入[用户名]的用户组
  - -d：指定home目录，默认放在/home/用户名

- **删除用户（root权限）：**

  ```shell
  userdel -r 用户名
  ```

  - -r：删除用户的home目录，不使用会保留home目录

- **查看用户所属组**

  ```shell
  id[用户名]
  ```

  - 参数不写表示查看当前用户的所属组

- **修改用户所属组**

  ```shell
  usermod -ag 用户组 用户名
  ```

  将指定用户加入到指定用户组

### 用户组

**创建用户组：**

```shell
groupadd 用户组名
```

**删除用户组：**

```
groupdel 用户组名
```

## 权限控制

### 查看权限

Linux系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。为了保护系统的安全性，Linux系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。

**在Linux中可以使用`ll`或者`ls –l`命令来显示一个文件的属性以及文件所属的用户和组（权限管控信息）**

![image-20210722203223026](https://gitee.com/dong-dameng/images/raw/master/img/image-20210722203223026.png)

- **第一列：**表示文件、文件夹的权限控制信息（文件属性）
- **第三列：**表示文件、文件夹所属用户
- **第四列：**表示文件、文件夹所属用户组

### 权限信息

![1699970-20190616125146973-273227113](https://gitee.com/dong-dameng/images/raw/master/img/1699970-20190616125146973-273227113.png)

- **第一个字符：**

  在Linux中第一个字符代表这个文件是目录、文件或链接文件等

  - **当为[ d ]则是目录**

  - **当为[ - ]则是文件；**

  - **若是[ l ]则表示为链接文档 ( link file )；**

  - [ **b** ]则表示为装置文件里面的可供储存的接口设备 ( 可随机存取装置 )；

  - [ **c** ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标 ( 一次性读取装置 )。

- **属主：**所属的用户，文档所有者，这是一个账户，这是一个人

- **属组：**所属的用户组，文档所有者所在的组，这是一个组

- **后三个字符：**

  接下来的字符中，以三个为一组，且均为『rwx』 的三个参数的组合，分别表示**属主**、**属组**和**其他用户**的权限

  这三个权限的位置不会改变，如果没有权限，就会出现减号[ - ]而已。

  - `r`：可读(read)

    - 文件可读

    - 文件夹可查看内容，如ls命令

  - `w`：可写(write)
    - 文件可修改
    - 文件夹可在文件夹内创建、修改、重命名等操作

  - `x`：可执行(execute)
    - 针对文件，可作为程序执行
    - 针对文件夹，表示可更改工作目录到此文件夹

### 修改权限

**1、chgrp：更改文件属组**

Change Group

```shell
chgrp [-R] 属组名 文件名
```

-R：递归更改文件属组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属组都会更改。

**2、chown：更改文件属主，也可以同时更改文件属组**

Change Owner

```shell
chown [–R] 属主名 文件名
chown [-R] 属主名：属组名 文件名
```

**3、chmod：更改文件9个属性**

```shell
chmod [-R] xyz 文件或目录
```

Linux文件属性有两种设置方法，一种是数字（常用），一种是符号。

Linux文件的基本权限就有九个，分别是owner/group/others三种身份各有自己的read/write/execute权限。

文件的权限字符为：『-rwxrwxrwx』， 这九个权限是三个三个一组的！其中，我们可以使用数字来代表各个权限，各权限的分数对照表如下：

```shell
r:4     w:2       x:1

可读可写不可执行 rw- 6
```

**其实就是用二进制1/0表示是否有该权限，并用三位二进制数c的十进制表示权限**

每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为：[-rwxrwx---] 分数则是：

- owner = `rwx` = 4+2+1 = 7
- group = `rwx` = 4+2+1 = 7
- others= `---` = 0+0+0 = 0

```shell
chmod 770 filename
```

# 【实用技巧】

## 命令行

|  快捷键   |                  操作                   |                  说明                  |
| :-------: | :-------------------------------------: | :------------------------------------: |
|  ctrl+c   | 强制停止程序/退出当前命令输入(换行重输) |                                        |
|  ctrl+d   |                退出/登出                | 可以退出某些程序的专属页面，比如python |
|  history  |              搜索历史命令               |                                        |
| !命令前缀 |      自动执行上一次匹配前缀的命令       |                                        |
|  ctrl+r   |          输入内容匹配历史命令           |                                        |
|  ctrl+l   |                  清屏                   |            与clear效果相同             |

## 光标定位、翻页

| 快捷键 |           操作            |   说明   |
| :----: | :-----------------------: | :------: |
|   0    |     将光标定位在行首      | 命令模式 |
|   $    |        定位到行尾         | 命令模式 |
| ctrl+→ |      向右跳一个单词       |          |
| ctrl+← |      向左跳一个单词       |          |
| ctrl+a |       跳到命令开头        |          |
| ctrl+e |       跳到命令结尾        |          |
|  n+gg  | 跳转到某一行，不需要按住  | 命令模式 |
|  Pgup  | 向上翻页（ctrl+f，front） |          |
|  Pgdn  | 向下翻页（ctrl+b，back）  |          |

## 撤销、删除

|   快捷键    |      操作      |     说明     |
| :---------: | :------------: | :----------: |
|      u      |      撤销      |              |
|   ctrl+r    | 恢复（反撤销） |              |
|     dd      |    删除一行    |              |
| :首行,尾行d |    删除多行    | 底线命令模式 |

## 复制、剪切、粘贴、选中

| 快捷键 |            操作            | 说明 |
| :----: | :------------------------: | ---- |
|   yy   |       复制光标所在行       |      |
|  nyy   |   复制所在行开始向下n行    |      |
|   dd   |         剪切所在行         |      |
|  ndd   |   剪切所在行开始向下n行    |      |
|   v    |  开始选中，可以配合yy复制  |      |
|  ggVG  | (g+g+按住CapsLock+v+g)全选 |      |
|   p    |            粘贴            |      |

## 其他

| 快捷键 |              操作              | 说明 |
| :----: | :----------------------------: | :--: |
|  /abc  | 查找"abc"并高亮，按n查找下一个 |      |
| :nohl  |          取消查找高亮          |      |

# 【软件安装】

> Linux按照软件方式：
>
> - 下载安装包自行安装（rpm命令安装）
> - 使用Linux命令行的应用商店，采用yum命令安装

## rpm

> RPM（Red-hat Package Manager）是底层管理工具，适用于所有环境，在安装软件时只会安装指定的软件，而不会安装依赖性文件，若所安装软件无依赖性文件或依赖性文件被解决，则可以安装，否则会报错。
>
> **查看已安装的软件：（以mysql为例）**
>
> ```shell
> rpm -qa | grep mysql
> ```
>
> - `-q` 查询软件包是否安装
> - `-a` 查询系统中所有安装的软件包
>
> **查看软件包的详细信息：**
>
> ```shell
> rpm -qi 包名
> ```
>
> ****

|   目标   |                  命令                  |
| :------: | :------------------------------------: |
| 安装软件 |           `rpm -ivh` 包全名            |
| 升级软件 | `rpm -Uvh` 包全名   ` rpm -Fvh` 包全名 |
| 卸载软件 |             `rpm -e` 包名              |

- `-i` install
- `-v` verbose 显示详细信息
- `-h` hash打印 #，显示安装进度
- `-U` 如果该软件没安装过则直接安装；若安装过则升级至最新版本
- `-F` 如果该软件没有安装，则不会安装，必须安装有较低版本才能升级
- `-e` erase 卸载

## yum

> YUM（Yellow dog Updater, Modified）基于 rpm，增加了自动解决依赖关系的方案，是上层管理工具，会自动解决依赖性。yum 在服务器端存有所有的 RPM 包，并将各个包之间的依赖关系记录在文件中，当使用 yum 安装 RPM 包时，yum 会先从服务器端下载包的依赖性文件，通过分析此文件从服务器端一次性下载所有相关的 RPM 包并进行安装
>
> **yum命令需要root权限与联网（不使用本地yum源的话）**
>
> Ubuntu系统采用apt命令，安装包为dep包
>
> **查看已安装的软件：（以mysql为例）**
>
> ```shell
> yum list installed mysql
> ```

```shell
yum [-y] [install | remove | search] 软件名称
```

- 选项：-y，自动确认，无需手动安装或卸载过程
- install：安装
- remove：卸载
- search：在应用商店中搜索

**举例：安装wget：**

```shell
# 安装
yum install wget
# 卸载
yum remove wget
# 搜索
yum search wget
```

**其他命令**

|                      目标                      |                             命令                             |
| :--------------------------------------------: | :----------------------------------------------------------: |
|                列出所有软件仓库                |                       yum repolist all                       |
| 列出仓库中的包（所有、可更新、仓库外、已安装） | yum list 软件包名称<br />all<br />updates<br />extras<br />installed |
|    查看软件包信息（可更新、仓库外、已安装）    | yum info 软件包名称<br />updates<br />extras<br />installed  |
|                   重装软件包                   |                   yum reinstall 软件包名称                   |
|                   升级软件包                   |                    yum update 软件包名称                     |
|                清除所有仓库缓存                |                        yum clean all                         |
|                检查可更新软件包                |                       yum check-update                       |

## 部署MySQL

> 由于MySQL并不在CentOS的官方仓库中，所以要配置yum仓库
>
> 需要root权限

### 安装

1. **配置仓库：**

   - **5.7版本库：**

     ```shell
     # 5.7版本仓库
     # 更新密钥
     rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
     
     # 安装Mysql yum库
     rpm -Uvh http://repo.mysql.com//mysql57-community-release-el7-7.noarch.rpm
     ```

   - **卸载库**

     ```shell
     rpm -qa | grep mysql
     
     mysql57-community-release-el7-7.noarch
     
     rpm -e mysql57-community-release
     ```

   - **8.0版本库**

     ```shell
     # 更新密钥
     rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
     
     # 安装Mysql8.x版本 yum库
     rpm -Uvh https://dev.mysql.com/get/mysql80-community-release-el7-2.noarch.rpm
     ```

2. **使用yum安装MySQL：**

   ```shell
   yum -y install mysql-community-server
   ```

3. **安装完成后，启动MySQL并配置开机自启动：**

   ```shell
   systemctl start mysqld
   systemctl enable mysqld
   ```

   > MySQL安装完成后，会自动配置为名称叫做：`mysqld`的服务，可以被systemctl所管理

4. **检查MySQL运行状态：**

   ```shell
   systemctl status mysqld
   ```

### 配置

1. **获取MySQL的初始密码：**

   ```shell
   # 通过grep命令，在/var/log/mysqld.log文件中，过滤temporary password关键字，得到初始密码
   grep 'temporary password' /var/log/mysqld.log
   ```

2. **登录MySQL数据库系统：**

   ```shell
   # 执行
   mysql -uroot -p
   # 解释
   # -u，登陆的用户，MySQL数据库的管理员用户同Linux一样，是root
   # -p，表示使用密码登陆
   
   # 执行完毕后输入刚刚得到的初始密码，即可进入MySQL数据库
   ```

3. **修改root密码：**

   - 5.7版本：

     ```mysql
     # 在MySQL控制台内执行
     ALTER USER 'root'@'localhost' IDENTIFIED BY '密码';	-- 密码需要符合：大于8位，有大写字母，有特殊符号，不能是连续的简单语句如123，abc
     ```

   - 8.0版本：

     ```mysql
     ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '密码';	-- 密码需要符合：大于8位，有大写字母，有特殊符号，不能是连续的简单语句如123，abc
     ```

4. **[扩展]配置root的简单密码**

   > 此配置仅仅是用于测试环境或学习环境的MySQL，如果是正式使用，请勿设置简单密码

   ```mysql
   set global validate_password.policy=0;		# 密码安全级别低
   set global validate_password.length=4;		# 密码长度最低4位即可
   ```

5. **[扩展]允许root远程登录，并设置远程登录密码**

   > 默认情况下，root用户是不运行远程登录的，只允许在MySQL所在的Linux服务器登陆MySQL系统
   >
   > 允许root远程登录会带来安全风险

   - 5.7版本：

     ```mysql
     # 授权root远程登录
     grant all privileges on *.* to root@"IP地址" identified by '密码' with grant option;  
     # IP地址即允许登陆的IP地址，也可以填写%，表示允许任何地址
     # 密码表示给远程登录独立设置密码，和本地登陆的密码可以不同
     
     # 刷新权限，生效
     flush privileges;
     ```

   - 8.0版本：

     ```mysql
     # 第一次设置root远程登录，并配置远程密码使用如下SQL命令
     # %表示授权任意ip登录
     create user 'root'@'%' IDENTIFIED WITH mysql_native_password BY '密码!';	-- 密码需要符合：大于8位，有大写字母，有特殊符号，不能是连续的简单语句如123，abc
     
     # 后续修改密码使用如下SQL命令
     ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '密码';
     ```

6. **退出MySQL控制台页面：**

   `exit`命令或者`ctrl+d`快捷键

7. **检查端口：**

   MySQL默认绑定了3306端口，可以通过端口占用检查MySQL的网络状态

   ```shell
   netstat -anp | grep 3306
   ```

## 部署Tomcat

> Tomcat建议使用非Root用户安装并启动
>
> 可以创建一个用户：tomcat 用以部署

# 【服务】

## systemctl命令

> Linux系统很多软件（内置第三方）均支持systemctl命令控制：启动、停止、开机自启
>
> 支持systemctl命令管理的软件一般也称之为：服务
>
> 系统内置服务很多：
>
> - NetworkManager，主网络服务
> - network，副网络服务
> - firewalld，防火墙服务
> - sshd，ssh服务（xshell远程登录使用的就是这个）

**语法：**

```shell
systemctl start | stop | status | enable | disable 服务
```

- start：启动服务
- stop：停止服务
- status：查看服务状态
- enable：开机自启
- disable：关闭开机自启

**一些第三方软件安装后也可以通过systemctl进行控制：**

因为这些软件安装完成后会自动注册到systemctl中，没有自动注册的软件也可以进行手动注册

```shell
# ntp，安装后会自动注册服务ntpd
yum install -y ntp
# httpd，安装后会自动注册服务httpd
yum install -y httpd
```

# 【链接】

## 软链接

> 在系统中创建软链接，可以将文件、文件夹链接到其他位置
>
> 类似windows系统中的《快捷方式》
>
> `ln`：创建软链接

**语法：**

```shell
ln -s 参数1 参数2
```

- 选项：-s，创建软链接
- 参数1：被链接的文件或文件夹
- 参数2：要链接去的目的地

![image-20230517112524015](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230517112524015.png)

# 【日期时间】

## date命令

> 通过date命令可以在命令行查看系统的时间

```shell
date [-d] [+格式化字符串]
```

- -d，按照给定的字符串显示日期，一般用于日期计算

  可以配合格式化字符串使用

  ```shell
  [root@localhost test1]# date -d "+1 day" +%Y-%m-%d
  # 2023-05-17，+1 day后变成05-18
  2023-05-18
  
  [root@localhost test1]# date -d "-1 year +1 month +1 day" +%Y-%m-%d
  2022-06-18
  ```

  支持的时间标记：

  - year 年
  - month 月
  - day 天
  - hour 小时
  - minute 分钟
  - second 秒

- 格式化字符串：通过特定的字符串标记，用来控制显示的日期格式：

  ![image-20230517152400822](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230517152400822.png)

**举例：**

```shell
# 含有空格用双引号包裹
[root@localhost test1]# date "+%Y-%m-%d %H:%M:%S"
# 显示效果：
2023-05-17 23:26:01
```

## 修改Linux时区

```shell
# 当前时区为UTC，与国内时区不同
[root@localhost test1]# date
Wed May 17 23:37:23 UTC 2023
```

**使用root权限，执行以下命令，修改时区为东八区：**

删除系统自带的localtime文件，并将上海的localtime链接为localtime即可

```shell
rm -f /etc/localtime
sudo ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

## 自动校准-ntp

**安装ntp：**

```shell
# 自动注册ntpd服务
yum -install ntp
```

**启动服务，并设置成自动校准：**

ntp启动后会定期联网校准时间

```shell
systemctl enable ntpd
systemctl start ntpd
```

**手动校准（root）：**

```shell
# 阿里云的ntp服务器
ntpdate -u ntp.aliyun.com
```

# 【ip/主机名】

## IP地址

**查看ip地址**

```shell
ifconfig
```

![image-20230517155445611](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230517155445611.png)

**特殊ip地址：**

- 127.0.0.1：主机ip地址
- 0.0.0.0：特殊ip地址
  - 可以用于指代本机
  - 可以再端口绑定中用来确定绑定关系
  - 在一些ip地址限制中，表示所有ip的意思，如放行规则设置为0.0.0.0，表示允许任意ip访问

## 主机名

> 每一台电脑除了对外联络地址（ip地址）外，也可以给系统设计一个名字，称为主机名

**查询主机名**

```shell
hostname

[root@localhost test1]# hostname
localhost.localdomain
```

**修改主机名（root）：**

```shell
hostnametctl set-hostname 主机名,修改主机名
```

## 域名解析

域名是ip地址的字符化，通过DNS服务器解析域名拿到对应的ip地址。

域名解析具体流程如下：

![image-20230517160710132](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230517160710132.png)

可以在windows系统中修改本地地址本，将主机名和ip地址的映射关系配置进去，这样本机远程连接服务器，就不需要通过ip地址访问（而是可以通过主机名）

**这个方法只是本地有效**

- host文件地址：C:\Windows\System32\drivers\etc

- 修改文件（管理员权限），尾部追加 ip 主机名映射关系：

  ```txt
  192.168.124.168 chongming
  ```

# 【网络传输】

## 下载和网络请求

### ping命令

**ping命令检查服务器是否可联通**

```shell
ping [-c num] ip或域名
```

- 选项：-c，检查的次数，不设置将无限次数持续检查
- 参数：ip或域名，即被检查的服务器的ip地址或域名

```shell
[root@localhost test1]# ping -c 5 www.baidu.com

PING www.a.shifen.com (180.101.50.188) 56(84) bytes of data.
64 bytes from 180.101.50.188 (180.101.50.188): icmp_seq=1 ttl=128 time=17.3 ms
64 bytes from 180.101.50.188 (180.101.50.188): icmp_seq=2 ttl=128 time=31.3 ms
64 bytes from 180.101.50.188 (180.101.50.188): icmp_seq=3 ttl=128 time=12.7 ms
64 bytes from 180.101.50.188 (180.101.50.188): icmp_seq=4 ttl=128 time=15.6 ms
64 bytes from 180.101.50.188 (180.101.50.188): icmp_seq=5 ttl=128 time=23.2 ms

--- www.a.shifen.com ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4029ms
rtt min/avg/max/mdev = 12.742/20.064/31.397/6.619 ms

```

### wget

wget是非交互式的文件下载器，可以再命令行内下载网络文件

**安装**

```shell
yum -install wget
```

**语法：**

```shell
wget [-b] url
```

- 选项：-b，可选，后台下载，会将日志写入到当前工作目录的wget-log文件
- 参数：url，下载地址
- ctrl+c退出下载，但下载到一半的文件还存在，需要手动rm删除

### curl

curl可以发送http网络请求，可用于：下载文件、获取信息等

**语法：**

```shell
curl [-O] url
```

- 选项：-O，用于下载文件，当url是下载链接时，可以使用此选项保存文件
- 参数：url，要发起请求的网络地址

## 端口

> 端口，是设备与外界通讯交流的出入口。
>
> - 物理端口：又可称之为接口，是可见的端口，如usb端口、hdmi端口等
> - 虚拟端口：是指计算机内部的端口，是不可见的，是用来操作系统和外部进行交互使用的
>
> ![image-20230517171104411](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230517171104411.png)
>
> ![image-20230517171259135](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230517171259135.png)

### 查看指定ip-nmap

**安装：**

```shell
yum install nmap
```

**语法：**

```shell
nmap 被查看的IP地址
```

查看目标地址公开的端口

```shell
[root@localhost test1]# nmap 192.168.124.128

Starting Nmap 6.40 ( http://nmap.org ) at 2023-05-18 01:16 CST
Nmap scan report for 192.168.124.128
Host is up (0.0000050s latency).
Not shown: 996 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
111/tcp open  rpcbind
443/tcp open  https

Nmap done: 1 IP address (1 host up) scanned in 1.69 seconds
```

### 查看端口占用-netstat

可以通过netstat命令，查看指定端口的占用情况

**安装和使用：**

```shell
yum install net-tools
# netstat -anp 查看本机端口占用情况
# 管道符+grep过滤关键字，找到指定端口
netstat -anp | grep 端口号
```

```shell
[root@localhost test1]# netstat -anp | grep 6000
unix  3      [ ]         STREAM     CONNECTED     26000    1728/su
```

如果过滤后找不到说明该端口没有被使用

# 【进程管理】

## 进程

程序运行在操作系统中，是被操作系统所管理的

为管理运行的程序，每一个程序在运行的时候，便被操作系统注册为系统中的一个进程

每一个进程会被分配一个独有的进程id

## 查看进程

**语法：**

```shell
ps [-e -f]
```

- 选项：-e，显示出全部的进程

- 选项：-f，以完全格式化的形式展示信息（展示全部信息）

- 一般固定用法就是：`ps -ef`（配合grep筛选）

  比如：`ps -ef | grep 30001` 查找进程号关键字为30001的进程

![image-20230519143701977](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230519143701977.png)

| 属性  |                  说明                   |
| :---: | :-------------------------------------: |
|  UID  |            进程所属的用户ID             |
|  PID  |              进程的进程号D              |
| PPID  |   进程的父ID（启动此进程的其他进程）    |
|   C   |       此进程的CPU占用率（百分比）       |
| STIME |             进程的启动时间              |
|  TTY  | 启动此进程的终端序号，`?`表示非终端启动 |
| TIME  |            进程占用CPU的时间            |
|  CMD  |    进程对应的名称/启动路径/启动命令     |

## 关闭进程

**语法：**

```shell
kill [-9] 进程ID
```

- 选项：-9，强制关闭，不使用则向进程发送要求关闭的信号，是否关闭要看进程本身的处理机制。

**举例：**

```shell
kill 1712
kill -9 1771
```

![image-20230519145040853](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230519145040853.png)

# 【主机状态】

## 系统资源监控

**语法：**

```shell
top
# 退出 ctrl+c
```

```shell
-p 只显示某个进程的信息
-d 设置刷新时间，默认是5秒
-c 显示产生进程的完整命令，默认是进程名
-n 指定刷新次数，比如top -n 3，刷新3次自动退出
-b 以非交互非全屏模式运行，以批次的方式执行top（不是刷新内存，而是一页一页更新）;一般配合-n指定输出几次统计信息，将输出重定向到指定文件，比如 top -b -n 3 > /tmp/top.tmp
-i 不显示任何闲置（idle）或无用（zombie）的进程
-u 查找特定用户启动的进程
```

类似Win的任务管理器，默认5秒刷新一次

![image-20230519145211334](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230519145211334.png)

![image-20230519145402263](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230519145402263.png)

![image-20230519145852734](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230519145852734.png)

**top交互式选项：**

| 按键 |              功能              |
| :--: | :----------------------------: |
| h键  |          显示帮助画面          |
| c键  |     显示产生进程的完整命令     |
| f键  |       选择需要展示的项目       |
| M键  |   根据驻留内存大小(RES)排序    |
| P键  |   根据CPU使用百分比大小排序    |
| T键  |     根据时间/累计时间排序      |
| E键  |      切换顶部内存显示单位      |
| e键  |      切换进程内存显示单位      |
| l键  | 切换显示平均负载和启动时间信息 |
| i键  |     不显示闲置或无用的进程     |
| t键  |      切换显示CPU状态信息       |
| m键  |        切换显示内存信息        |

## 磁盘信息监控

- **`df`命令，查看空间使用情况**

  ```shell
  df -h
  ```

  h选项，以更人性化的单位显示

  ![image-20230519151338610](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230519151338610.png)

- **`iostat`命令，查看CPU、磁盘相关信息**

  ```shell
  iostat [-x] [num1] [num2]
  ```

  - 选项：-x，显示更多信息
  - num1：数字，刷新间隔
  - num2：数字，刷新次数

  ![image-20230519151746482](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230519151746482.png)

  

## 网络状态监控

sar命令查看网络的相关统计（sar命令非常复杂，仅简单用于统计网路，采用固定写法）

```shell
sar -n DEV num1 num2
-n 查看网络
DEV 查看网络接口
num1 刷新间隔（默认一次就结束）
num2 查看次数
```



# 【环境变量】

> 环境变量是操作系统在运行时记录的一些关键性信息，用以辅助系统运行
>
> 在Linux系统中执行`env`命令可以查看当前系统的环境变量
>
> 环境变量是一种KeyValue型结构
>
> ```shell
> [root@chongming ~]# env
> XDG_SESSION_ID=3
> HOSTNAME=chongming
> TERM=xterm-256color
> SHELL=/bin/bash
> HISTSIZE=1000
> SSH_CLIENT=47.96.60.211 13159 22
> SSH_TTY=/dev/pts/0
> USER=root
> ```

## PATH

无论当前工作目录在哪，都能执行/usr/bin/cd下的命令（命令是可执行程序），就是借助环境变量中PATH项目的值实现。

PATH记录了系统执行任何命令的搜索路径（`:`隔开）：

```shell
[root@chongming ~]# env | grep PATH
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
```

当执行任何命令，都会安装上述顺序搜索可执行程序并执行

- /usr/local/sbin
- /usr/local/bin
- /usr/sbin
- /usr/bin
- /root/bin

## $符号

在Linux系统中，`$`符号用于取“变量”的值

环境变量记录的信息，除了操作系统自己使用外，用户也可以调用

**语法：**

```shell
$环境变量名
```

**举例：**

- `echo $PATH`

- `echo ${PATH}abc`

  当和其他内容混在一起时，使用`{}`包裹环境变量名

  ```shell
  [root@chongming ~]# echo 环境变量：${PATH}
  环境变量：/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
  ```

## 自定义变量

Linus环境变量可以用户自行设置：

- **临时设置：**当前会话生效

  语法：`export 变量名=变量值`

- **永久生效：**

  - 针对当前用户生效，配置在当前用户的`~/.bashrc`文件中
  - 针对所有用户生效，配置在系统的`/etc/profile`文件中

  通过语法：`source 配置文件`，进行立刻生效；或者重启系统/重新建立会话（登录xshell）生效

## 自定义PATH

环境变量PATH这个项目里面记录了系统执行命令的搜索路径

可以自行添加自己的可执行程序到PATH中，以在命令行中任何目录下执行命令

**演示：**

- 在当前HOME目录内创建shell脚本：`myenv`

  ```shell
  echo "hello world"
  ```

- 修改文件为可执行文件：

  ```shell
  chmod 755 myenv
  ```

- 修改PATH的值

  临时修改PATH：`export PATH=$PATH:/usr/chongming`

  `PATH=$PATH`后面接`:目录`这样就可以在原目录的基础上添加自己的目录，而非替换

- 在任何目录下执行`myenv`命令，都可以执行：

  ```shell
  [chongming@chongming usr]$ myenv
  hello world
  ```

  

# 【压缩/解压】

## tar命令

> **Liunx和Mac系统通常有两种压缩格式：**
>
> - `.tar`
>
>   称之为tarball，归档文件，即简单的将文件组装到一个.tar文件夹内，并没有太多文件体积的减少，仅仅是简单的封装
>
> - `.gz`
>
>   常见为.tar.gz、gzip格式压缩文件，即使用gzip压缩算法将文件压缩到一个文件夹内，可以极大的减少压缩后的体积
>
> ```shell
> # 查询帮助文档
> tar --help
> ```

**针对上述两种格式，使用`tar`命令进行压缩和解压缩**

**语法：**

```shell
tar [-c -v -x -f -z -C] 参数1 参数2 ... 参数N
```

- -c，创建压缩文件，用于压缩模式
- -v，显示压缩、解压过程，用于查看进度
- -x，解压模式
- -f，要创建的文件，或要解压的文件，-f选项必须在所有选项中位置处于最后一个
- -z，gzip模式，不适用-z就是普通的tarball格式
- -C，选择解压的目的地，用于解压模式

**常用组合：**

- **压缩：**

  ```shell
  tar -[z]cvf test.tar(压缩包名) 待压缩文件1 待压缩文件2 ...
  ```

  将带压缩文件压缩到test.tar包内，如果使用-z，则采用gzip模式，压缩包以.tar.gz结尾

  - -z，选项如果要使用的话，一般处于选项位第一个
  - -f，选项如果要使用的话，一般处于选项位最后一个

- **解压：**

  - 解压test.tar至当前目录

    ```shell
    tar -xvf test.tar
    ```

  - 解压test.tar至指定目录

    ```shell
    tar -xvf test.tar -C /tmp/test/testTar
    ```

  - 以gzip模式解压test.gz.tar至指定目录

    ```shell
    tar -zxvf test.gz.tar -C /temp/test/testTar
    ```

  - -f，选项必须位于选项组合中末位
  - -z，选项建议放在开头
  - -C，选项必须单独使用，和解压所需的其它参数分开

> Linux下使用tar压缩或解压文件，如果压缩或解压后的文件与同目录下的文件重名，会直接覆盖重名文件。
>
> 但是，如果原文件夹下有解压后的重名文件夹没有的文件，这些文件仍然存在。
>
> 即重名的文件夹不会直接覆盖，而是比较两个文件夹中重名的文件，只替换这些重名文件。

## zip/unzip命令

> 可以使用zip命令，压缩/解压文件zip压缩包
>
> unzip命令解压文件如果有重名文件，不会默认覆盖，需要指定是否覆盖/重命名

**语法：**

- **压缩：**

  ```shell
  zip -r 压缩包名 待压缩文件1 待压缩文件2 ...
  ```

  - -r，被压缩的文件包含文件夹时使用

- **解压：**

  ```shell
  unzip [-o] 待解压包名 [-d 解压到的路径]
  ```

  - -d，指定要解压去的位置，同tar的-C选项
  - -o，默认覆盖重名文件

# 【Shell脚本】

> shell 解释器可当作人与计算机硬件的“翻译官”， 他作为用户与linux系统内部的通信媒介，除了能够支持各种变量与参数外，还提供了像判断，循环等高级编程语言有的控制流程的特性。 想要正确高效的做好系统运维工作，脚本的使用至关重要。
>
> shell脚本的工作方式有两种：交互式和批处理。
>
> - 交互式(interactive) : 用户输入一条就立即执行
> - 批处理(batch): 由用户事先编写好一个完整的shell脚本，shell会一次性执行脚本中诸多的命令。
>
> 脚本中不仅会用到一般的linux命令、管道符、重定向，还需要把内部功能模块化后通过逻辑语句进行处理，最终形成日常使用的脚本。

## 编写简单脚本

使用vim将命令写入到一个文件中，就是一个简单的脚本

shell脚本的名称可以任意，一般以`.sh`后缀标识

```shell
#！/bin/bash
# this is a demo 
java -cp hello.jar Test
```

> 一般脚本的第一行脚声明 `#!`用来告诉系统使用哪种shell解释器来执行该脚本
> 第二行是对该脚本的注释信息，以便让后来使用脚本的人了解该脚本的功能或警告信息
> 之后是脚本的命令

## 脚本参数

为了让shell脚本程序更好地满足用户的实时需求，以便灵活完成工作，必须要让脚本像交互式执行命令一样，能够接收用户的参数。
脚本文件就可以接受参数，每个参数以空格隔开。

```shell
$0 代表当前脚本程序的名称
$N 代表第N个参数，如$1为第一个参数，$2为第二个参数。。。
$* 为所有的参数
$? 为上一次命令执行的返回值
```

**举例：**

```shell
#!/bin/bash 
# this a example
echo the name of this script is $0 
echo the first argument of this script is $1
echo the second argument of this script is $2
echo there are $# arguments totally, they $* 
```

**执行脚本：**

```shell
. test.sh 参数1 参数2
```

