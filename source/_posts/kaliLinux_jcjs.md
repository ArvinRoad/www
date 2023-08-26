---
title: kali-Linux 基础
date: 2021/9/15 23:50:25
tags:
  - Kali Linux
  - 教学文档
categories:
  - 程序
sticky: 1
cover: https://tse3-mm.cn.bing.net/th/id/OIP-C.RI-jGvsfW8kRiYJdf5eqlwHaD4?pid=ImgDet&rs=1
copyright_author: Arvin
copyright_author_href: https://github.com/ArvinRoad
copyright_info: 此文章版權歸Arvin所有，如有轉載，請註明來自原作者
---

## kali-Linux基础

`Lali Linux 系列第一篇`

**在线命令查找：[Linux命令参考](http://linux.51yip.com)**

**实验没用环境情况下可以在网上搜索在线PHP工具来测试**[在线代码工具](https://tool.lu/coderunner/)

### Kali-Linux代码介绍

```v
su root --- 切换到Root
sudo su --- 注册Root
Passwd root --- 输入新密码并重新输入改密码，注册Root
:w !sudo tee %  --- 管理员身份强制保存只读文件
lastb --- 查看目前与过去登入系统的用户相关信息
users --- 查看在线用户
chmod --- 权限管理
cat /etc/passwd --- 查看用户信息
cat /etc/group --- 查看用户组
id <用户名> --- 查看指定用户信息
id <组名> --- 查看指定用户组信息
useradd -m <用户名>  --- 自动在home文件夹下创建<用户>目录 - 添加用户
useradd -g <组名> <用户名> --- 指定添加在某个组里
passwod <用户名>	  --- 修改用户的新密码
userdel -r <用户名>  --- 删除目录以及用户
groups <用户名>      --- 查看用户组归属
usermod -g <组名> <用户名> --- 添加组 直接添加组用用户从原来组
usermod -a-G <组名> <用户名> --- 追加到新的组
groupdel <组名> 删除组

cat /etc/shells --- 查看它支持哪些Shells
echo $SHELLS --- 查看使用的哪个Shells，$是变量的意思|区分大小写
service ssh status --- 查看SSH服务是否开启（开机必查）
/etc/init.d/ssh status --- 查看SSH服务是否开启，方式二
service ssh start --- 开启Shells服务，（必须开启）
systemctl status ssh --- 查看运行状态

ifconfig --- 查询IP地址，远程连接等需要IP使用（不建议使用固定IP地址）
ifconfig eth0 192.168.1.65 --- 给eth0配置固定的IP地址，当然可以固定网关、子网掩码。具体看命令参考的介绍
service apache2 start --- 开启阿帕奇服务（自动开启WEB服务，在eth0端口查看）

clear --- 刷新屏幕
reset --- 完全刷新终端屏幕
ls --- 查看目录下文件和dir命令一样
ls -a --- 查看隐藏文件 -A取消隐藏文件
ls -l --- 查看文件权限划分（查看文件权限）
ls -lt --- 时间排序查看文件
dir --- 查看目录下文件和ls命令一样
<指令> -help --- 查看指令的帮助命令
pwd --- 查看当前目录
cd --- 切换目录，cd ~ 快速进入root目录|cd / 进入根目录
cp <文件名1>.txt <目录路径> --- 将文件复制粘贴在一个目录里
mv <文件名> <重命名> --- 文件改名
mv <文件名> <目标路径> --- 移动文件
file <文件名> --- 查看文件的类型信息
tar -czvf <文件>.tar <文件> ---- 打包压缩文件
tar zxvf <文件>.tar --- 解压文件
zip --- 压缩zip文件（具体请看命令参考的详细信息）
unzip <文件>.zip --- 解压zip文件（具体请看命令参考的详细信息）
wget --- 下载命令
git clone <地址> --- 下载git项目，clone克隆的意思
wget <图片地址> --- 下载图片
apt-get install <软件名称> --- 下载安装软件、文件
dpkg -i <安装包名称> --- 安装软件（如果报错请下载依赖：debian或ubuntu软件管理包）
sudo apt-get update --- 更新系统
dpkg -r <安装包名称> --- 删除软件包
dpkg -r --purge <安装包名称> --- 连同配置文件一起删除（卸载）

rm <文件> --- 删除文件
rm -rf --- 强制删除文件目录等 f是强制删除 r是递归删除
mkdir <文件夹名称> --- 创建一个文件夹
touch <文件名称>.txt --- 创建文件
vim <文件名>.txt --- 进入文件命令模式，具体参考VM编辑器的使用
cat <文件名>.txt --- 查看文件内容（少的）
more <文件名>.txt --- 查看文件内容（多的）
stat <文件名>.txt --- 查看文件存储、时间信息
wc <文件名>.txt --- 查看文件的行数、字数、字节数
man nmap --- 帮助我们查看Nmap软件的帮助文件
find <地址文件etc/passwd> --- 根据要求查找文件也可以查找文件内容（具体请看命令参考的详细信息）
man nmap >> <文件名>.text --- 将Nmap帮助文件内容输入在文件内
touch -c -t 09112003 <文件名>.text --- 将文件时间改为，9月11日20点03分 -c 不创建任何文件
touch -d "2 days ago" <文件名>.text --- 将文件时间修改为2天以前根据系统时间改（具体请看命令参考的详细信息）

apt update -y && apt install xfonts-intl-chinese ttf-wqy-microhei --- 安装字体
echo LANG="zh_CN.UTF-8" > /etc/default/locale --- 官方虚拟机包运行可设置系统语言为中文
dpkg-reconfigure locales --- 更换语言
dpkg-reconfigure locales --- 安装字体
apt-get update && apt-get dist-upgrade -y --- 刷新系统更新源

rm /* 删除所有文件、文件目录
```
---
#### Linux常用命令介绍
- **帮助命令**：在 Linux 环境中，如果遇到困难，可以使用帮助命令来取得帮助
- **常用系统工作命令**：Linux 中有一些是常用的系统工作命令
- **系统状态检测命令**：在 Linux 有一些可以查看 Linux 配置系统的基本命令
- **工作目录切换命令**：在 Linux中，工作目录指的是用户当前在系统中所处的位置
- **文本文件编辑命令**
- **文件目录管理命令**
- **打包压缩与搜索命令**
- **文件管理权限命令**（Linux分为用户和组的感念）



**在线命令查找：[Linux命令参考](http://linux.51yip.com)**

```v
echon命令
echon命令用于在终端输出字符串或变量提取后的值，格式为"echo[字符串 | $变量] "

date命令
date命令用于显示以及设置系统的时间或日期，格式为"date [选项] [+指定的格式]"

poweroff命令
poweroff命令用于关闭系统，其格式为poweroff

top命令
显示当前系统正在执行的进程的相关信息，包括进程ID、内存占用率等,格式为"top [参数]"

ifconfig命令
ifconfig命令用于获取网卡配置与网络状态等信息，格式为"ifconfig [网络设备] [参数]"

uname命令
uname命令用于查看系统内核与系统版本等信息，格式为"uname [-a]"

who命令
who命令用于查看当前登入主机的用户终端信息，格式为"who [参数]"

history命令
history命令用于显示历史执行过的命令，格式为"history [-c]"

pwd命令
pwd命令用于显示用户当前所处的工作目录，格式为"pwd [选项]"

cd命令
cd命令用于切换工作路径，格式为"cd [目录名称]"

ls命令
ls命令用于显示目录中的文件信息，格式位"ls [选项] [文件]"

cat命令
cat命令用于查看纯文本文件（内容较少的），格式为"cat [选项] [文件]"

more命令
more命令用于查看纯文本文件（内容较多的），格式为"more [选项] [文件]"

head命令
head命令用于查看纯文本文档的前N行，格式为"head [选项] [文件]"

wc命令
wc命令用于统计指定文本的行数、字数、字节数，格式为"wc [参数] 文本"

stat命令
stat命令用于查看文件的具体存储信息和时间等信息，格式为"stat 文件名称"

touch命令
touch命令用于创建空白文件或设置文件的时间信息，格式为"touch [选项] [文件]"

mkdir命令
mkdir命令用于创建空白的文件目录，格式为"mkdir [选项] [目录名称]"

cp命令
cp命令用于复制文件或文件目录，格式为"cp [选项] [源文件] [目标文件]"

mv命令
mv命令用于移动文件或将文件重命名，格式为"mv [选项] [源文件] [目标路径|目标文件名]"

file命令
file命令用于查看文件的类型，格式为"file [文件名]"

tar命令
tar命令用于对文件进行打包压缩或解压，格式为"tar [选项] [文件]"

find命令
find命令用于按照指定条件来查找文件，格式为"find [查找路径] [寻找条件] [操作]"

```

---

#### 常用命令简洁解释

```v
cat --- 查看文本文件（内容少的）
more --- 查看文本文件（内容多的）
head --- 查看文本文档的前N行
wc --- 统计指定文本的行数、字数、字节数
stat --- 查看文本的存储、时间等信息
echo --- 输出命令，它可以和map帮助命令一样使用帮助我们创建文件
date --- 查看时间
poweroff --- 关机
top --- 和 Windos 任务管理器功能类似（防卡死、检测程序是否执行）
ifconfig --- 查看网络信息
uname --- 查看用户、系统信息
who --- 查看终端、本地连接用户信息
whoami --- 查看自己的用户信息
history --- 查看用户指令历史（默认保存1000条）可查看其他用户的操作
lastb --- 查看目前与过去登入系统的用户相关信息
pwd --- 查看当前目录
cd --- 切换目录
ls --- 查看目录下文件
touch --- 创建空白文件或改变文件时间信息
mkdir --- 创建空白文件目录
cp --- 复制文件或文件目录
mv --- 移动文件或重命名文件
file --- 查看文件类型
tar --- 对文件进行打包压缩或解压
find --- 按照指定条件查找文件
grep --- h
chmod --- 权限管理
wget --- 下载命令
rm --- 删除
rm /* --- 所有东西
```

---

#### 文件管理权限命令

`使用 chmod 命令进行文件的权限修改`

> chmod [选项] < 模式, 模式... > <文件名>

**文件和目录的权限**

> Linux 中文件和目录的权限有所不同

- 文件权限

```v
r --- 可以读文件  数值为4
w --- 可以写文件  数值为2
x --- 可以执行文件 数值为1
```

- 目录权限

```v
x r --- 可以读取（cp）和查看（ls）目录的内容（即文件和目录），同时还需要可执行权限
x w --- 可以在目录里创建文件（touch）和目录（mkdir）和删除文件（rm）和目录（rmdir），同时还需要可执行权限
x x --- 可以进入目录（cd）和执行文件 实践过程
```



---

#### Kali Linux 系统更新源  

```v
# 阿里源
deb https://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb-src https://mirrors.aliyun.com/kali kali-rolling main non-free contrib
 
# 清华大学
deb https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free

# 中科大
deb http://mirrors.ustc.edu.cn/kali kali main non-free contrib
deb-src http://mirrors.ustc.edu.cn/kali kali main non-free contrib
deb http://mirrors.ustc.edu.cn/kali-security kali/updates main contrib non-free
```

---
### 什么是Shell
在计算机科学中**Shell** 俗称外壳（用来区别于内核），它类似于Windows的DOS系统，能够接收用户的命令并翻译给操作系统执行，**是用户与操作系统（内核）之间的桥梁**。

#### 查看Shell
查看系统支持哪些 **Shell（命令）**：`eat /etc/shells`
查看正在使用的**Shell**：`echo $SHELL`

##### Shell与终端的区别
- **终端**：接收用户的输入，并传递给Shell程序，接收程序输出并展示到屏幕。
- **Shell**：接收并解析用户的指令给操作系统，将结果输出到终端。建议采用Zsh终端

---
### VM编辑器
**VM编辑器是所有Unix及Linux系统下标准的编辑器，就相当于Windows系统中的记事本一样**，它的强大不逊色与任何最新的文本编辑器。它是我们使用Linux系统不能缺少的工具。
- **vim具有程序编辑的能力，可以以字体颜色辨别语法的正确性，方便程序设计**；
- **vim可以当作vi的升级版本，它可以用多种颜色的方式来显示一些特殊的信息**；
- **vim会依据文件扩展名或者是文件内的头部信息，判断该文件的内容而自动的执行该程序的语法判断式，再以颜色来显示程序代码与一般信息**；
- **vim里面加入了很多额外的功能，例如支持正则表达式的搜索、多文件编辑、块复制等等。这对于我们在Liunx上进行一些配置文件的修改工作是很棒的功能**；

#### VM编辑器的使用（代码介绍）
**Vi / Vim 编辑器模式**：

- **命令模式（默认**：刚进入Vim的时候，默认就是命令模式，可以复制行、删除行等。
- **输入模式**：可以输入内容。

**模式转化 **:
使用insert键切换进入输入模式：命令模式 --> 输入模式：

> i ：在当前光标所在字符的前面，转为输入模式
> I ：在当前光标所在行的行首，转化为输入模式
> a ：在当前光标所在字符的后面，转化为输入模式
> A ：在当前光标所在行尾，转化为输入模式
> o ：在当前光标所在行的下方，新建一行，并转化为输入模式
> O ：在当前光标所在行的上方，新建一行，并转化为输入模式
> s ：删除光标所在字符
> r ：替换光标处字符

输入模式 --> 命令模式
> **ESC键**

关闭、保存文件
> **Shift+：号**命令模式下：
> w ：保存
> q ：退出
> wq 和 x 都是保存退出
> q! ：强制退出
> w! ：强制保存，管理员才有权限

查找
> **普通模式：**
> /PATTERN ：从当前位置向后查找
> ?PATTERN ：从当前位置向前查找



































