# 1. Linux

## 1.1 linux分类

Linux根据原生程度，分为两种：

1. 内核版本 ：Linux不是一个操作系统，严格来讲，Linux只是一个操作系统中的内核。内核是什么？内核建立了计算机软件与硬件之间通讯的平台，内核提供系统服务，比如文件管理、虚拟内存、设备I/O等；
2. 发行版本：一些组织或公司在内核版基础上进行二次开发而重新发行的版本。比如 ubuntu 和 CentOS 

## 1.2 Linux文件系统

### 1.2.1 简介

在Linux操作系统中，所有被操作系统管理的资源，例如网络接口卡、磁盘驱动器、打印机、输入输出设备、普通文件或是目录都被看作是一个文件。

在LINUX系统中有一个重要的概念：**一切都是文件**。

### 1.2.2 文件类型和目录结构

Linux支持5种文件类型：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220301102823673.png" alt="image-20220301102823673" style="zoom: 80%;" />



<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220301102932722.png" alt="image-20220301102932722" style="zoom:80%;" />

**常见目录说明：**

- **/bin：** 存放二进制可执行文件(ls,cat,mkdir等)，常用命令一般都在这里；
- **/etc：**  存放系统管理和配置文件；
- **/home：**  存放所有用户文件的根目录，是用户主目录的基点，比如用户user的主目录就是/home/user，可以用~user表示；
- **/usr ：** 用于存放系统应用程序；
- **/opt：** 额外安装的可选应用程序包所放置的位置。一般情况下，我们可以把tomcat等都安装到这里；
- **/proc：**  虚拟文件系统目录，是系统内存的映射。可直接访问这个目录来获取系统信息；
- **/root：**	超级用户（系统管理员）的主目录（特权阶级^o^）；
- **/sbin:**	存放二进制可执行文件，只有root才能访问。这里存放的是系统管理员使用的系统级别的管理命令和程序。如ifconfig等；
- **/dev：**	用于存放设备文件；
- **/mnt：**	系统管理员安装临时文件系统的安装点，系统提供这个目录是让用户临时挂载其他的文件系统；
- **/boot：**	存放用于系统引导时使用的各种文件；
- **/lib ：**      存放着和系统运行相关的库文件 ；
- **/tmp：**	用于存放各种临时文件，是公用的临时文件存储点；
- **/var：**	用于存放运行时需要改变数据的文件，也是某些大文件的溢出区，比方说各种服务的日志文件（系统启动日志等。）等；
- **/lost+found：**	这个目录平时是空的，系统非正常关机而留下“无家可归”的文件（windows下叫什么.chk）就在这里。

## 1.3 Linux基本命令

### 1.3.1 目录切换命令

- **`cd usr`：** 切换到该目录下usr目录
- **`cd ..（或cd../）`：** 切换到上一层目录
- **`cd /`：** 切换到系统根目录
- **`cd ~`：** 切换到用户主目录
- **`cd -`：** 切换到上一个所在目录

### 1.3.2 目录的操作命令

**`mkdir 目录名称`：** 增加目录

**`ls或者ll`**（ll是ls -l的缩写，ll命令以看到该目录下的所有目录和文件的详细信息）：**查看目录信息**

**`find 目录 参数`：** 寻找目录（查）

示例：

- 列出当前目录及子目录下所有文件和文件夹: `find .`
- 在`/home`目录下查找以.txt结尾的文件名:`find /home -name "*.txt"`
- 同上，但忽略大小写: `find /home -iname "*.txt"`
- 当前目录及子目录下查找所有以.txt和.pdf结尾的文件:`find . \( -name "*.txt" -o -name "*.pdf" \)`或`find . -name "*.txt" -o -name "*.pdf"`

**`mv 目录名称 新目录名称`：** 修改目录的名称（改）

注意：mv的语法不仅可以对目录进行重命名而且也可以对各种文件，压缩包等进行	重命名的操作。mv命令用来对文件或目录重新命名，或者将文件从一个目录移到另一个目录中。

**`mv 目录名称 目录的新位置`：**  移动目录的位置---剪切（改）

注意：mv语法不仅可以对目录进行剪切操作，对文件和压缩包等都可执行剪切操作。另外mv与cp的结果不同，mv好像文件“搬家”，文件个数并未增加。而cp对文件进行复制，文件个数增加了。

**`cp -r 目录名称 目录拷贝的目标位置`：** 拷贝目录（改），-r 代表递归拷贝

注意：cp命令不仅可以拷贝目录还可以拷贝文件，压缩包等，拷贝文件和压缩包时不用写-r递归

**`rm [-rf] 目录`:** 删除目录（删）

注意：rm不仅可以删除目录，也可以删除其他文件或压缩包

### 1.3.3 文件的操作命令

1. **`touch 文件名称`:** 文件的创建（增）

2. **`cat/more/less/tail 文件名称`** 文件的查看（查）

   - **`cat`：** 只能显示最后一屏内容
   - **`more`：** 可以显示百分比，回车可以向下一行， 空格可以向下一页，q可以退出查看
   - **`less`：** 可以使用键盘上的PgUp和PgDn向上 和向下翻页，q结束查看
   - **`tail-10` ：** 查看文件的后10行，Ctrl+C结束

3. **`vim 文件`：** 修改文件的内容（改）

   在实际开发中，使用vim编辑器来修改配置文件 

4. **`rm -rf 文件`：** 删除文件（删）

### 1.3.4 压缩文件的命令

**打包并压缩文件：**

**Linux中的打包文件一般是以 .tar结尾的**，**压缩的命令一般是以 .gz结尾的。**

一般情况下打包和压缩是一起进行，打包并压缩后的文件的后缀名一般 .tar.gz ,  命令： 

`tar -zcvf 打包压缩后的文件名 要打包压缩的文件`  

其中 ： z 调用gzip压缩命令进行压缩 c 打包文件  v 显示运行过程  f 指定文件名

举例 :  如 test目录下的三个文件分别是: aaa.txt bbb.txt  ccc.txt , 如果我们要打包test目录并指定压缩后的压缩包名称为 test.tar.gz  ，使用命令 ：`tar -zcvf test.tar.gz aaa.txt bbb.txt  ccc.txt`

解压压缩包：

命令：`tar [-xvf] 压缩文件`   其中 x代表解压

举例： 将test.tar.gz 压缩包 解压到当前目录下 ： `tar -xvf test.tar.gz`

### 1.3.5 Linux的权限命令

操作系统中每个文件都拥有特定的权限，所属用户和所属组。权限是操作系统用来限制资源访问的机制，在Linux中权限一般分为读(readable)、写(writable)和执行(excutable)，分为三组。分别对应文件的属主(owner)，属组(group)和其他用户(other).

通过 **`ls -l`** 命令我们可以查看某个目录下的文件或目录的权限

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/7/5/1646955be781daaa~tplv-t2oaga2asx-watermark.awebp)

第一列的内容的信息解释如下：

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/7/5/16469565b6951791~tplv-t2oaga2asx-watermark.awebp)

下面分别介绍 文件类型 Linux中权限以及文件 所有者 所在组 其他组 

**文件的类型：**

- d： 代表目录
- -： 代表文件
- l： 代表链接（可以认为是window中的快捷方式）

**文件和目录权限的区别：**

| 权限名称 | 文件                      | 目录                     |
| -------- | ------------------------- | ------------------------ |
| r        | 可以使用cat查看文件的内容 | 可以查看目录下列表       |
| w        | 可以修改文件的内容        | 可以创建和删除目录下文件 |
| x        | 可以将其运行为二进制文件  | 可以使用cd进入目录       |

**在linux中的每个用户必须属于一个组，不能独立于组外。在linux中每个文件有所有者、所在组、其它组的概念。**

* 所有者（owner）

  一般为文件的创建者，谁创建了该文件，就天然的成为该文件的所在组，用 `ls-ahl`  可以看到文件的所有者，可以使用 `chown 用户名 文件名`来修改文件的所有者 。

* 文件所在组(group)

  当某个用户创建了一个文件后，这个文件得所在组就是该用户所在的组  用 ls-ahl命令可以看到文件的所有组，也可以使用 `chgrp 组名 文件名`来修改文件所在的组。

* 其他组 (other)

  除开文件的所有者和所在组的用户外，系统的其它用户都是文件的其它组

修改文件/目录的权限的命令 ： `chmod` 

示例： 修改 /test下的 aaa.txt 的权限为 属主有全部权限，属主所在的组有读写权限，其他用户只有读的权限。

```js
chomd u=rwx,g=rw,o=r aaa.txt 
```

### 1.3.6 Linux用户管理

Linux系统是一个多用户多任务的分时操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。

用户的账号一方面可以帮助系统管理员对使用系统的用户进行跟踪，并控制他们对系统资源的访问；另一方面也可以帮助用户组织文件，并为用户提供安全性保护。

Linux用户管理相关命令：

* `useradd 选项 用户名` ：添加用户账号 
* `usermod 选项 用户名` ： 删除用户账号
* `usermod 选项 用户名` ： 修改账号
* `passwd 用户名` ： 更改或创建用户的密码
* `passwd -S 用户名` ： 显示用户账号密码信息
* `passwd -d  用户名`：清除用户密码

使用useradd指令建立的账号，实际上保存在 **/etc/passwd**文件中  ，一般用户只能变更自己的密码，只有管理者可以指定用户名称。

### 1.3.7 Linux系统用户组的管理

每个用户都有一个用户组，系统可以对一个用户组中的所有用户进行集中管理。不同Linux 系统对用户组的规定有所不同，如Linux下的用户属于与它同名的用户组，这个用户组在创建用户时同时创建。

用户组的管理涉及用户组的添加、删除和修改。组的增加、删除和修改实际上就是对**/etc/group**文件的更新。

**Linux系统用户组的管理相关命令:**

* `groupadd 选项 用户组` ： 增加一个新的用户组
* `groupdel 用户组`：要删除一个已有的用户组
* `groupmod 选项 用户组` ： 修改用户组的属性 

## 1.4 其他常用命令

* `pwd` : 显示当前所在位置
* `grep 要搜索的字符串 要搜索的文件 --color`  ：搜索命令，--color代表高亮命令

* **`ps -ef`/`ps aux`：** 这两个命令都是查看当前系统正在运行进程
* kill -9 进程的pid ：杀死进程（-9表示强制终止） 

网络通信命令：

* 查看当前系统的网卡信息： ifconfig
* 查看与某台机器的连接情况：ping
* 查看当前系统的端口使用：netstat -an 



