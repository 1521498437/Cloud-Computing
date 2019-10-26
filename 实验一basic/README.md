# 云计算实验基础搭建

## 一、购买腾讯云Cent OS服务器

腾讯云服务器有专门的学生套餐，链接：https://cloud.tencent.com/act/campus

地区选择[上海二区]，操作系统选择[CentOS 7.6 64位]，购买时长为6个月

![](./Image1/1.png)

进入腾讯云控制台，选择购买的云服务器，点击登录

![](./Image1/2.png)

输入设置的root密码

![](./Image1/3.png)

Web Shell登录已购买的云服务器实例成功

![](./Image1/4.png)

## 二、创建一个GitHub账号并同步本地项目仓库

### 1、创建账户

进入GitHub官网：https://github.com/，点击右上角的[Sign up]

![](./Image1/5.png)

创建GitHub账户

![](./Image1/6.png)

### 2、安装Git

Git Bash是Windows操作系统下进行Git操作的Shell，简而言之就是同步本地项目到GitHub网站上所使用的命令工具。

下载地址：https://git-scm.com/download/win 或 https://git-for-windows.github.io/    在安装时，全部选择默认即可。

安装成功并打开git-bash.exe ：

![](./Image1/7.png)

### 3、创建SSH Key

使用Git Bash进行命令行操作，首先要拥有一份ssh key进行身份验证。详细信息参见：[Connecting to GitHub with SSH](https://help.github.com/en/articles/connecting-to-github-with-ssh)。

#### ①验证是否存在ssh keys

```
ls -al ~/.ssh
```

默认情况下生成的ssh key放置于“C:\Users\UserName”下的.ssh文件夹中，该命令列出该文件夹下包含的文件。常见的ssh密钥文件可能是以下文件之一：

- id_dsa.pub
- id_ecdsa.pub
- id_ed25519.pub
- id_rsa.pub

#### ②创建新的ssh key

如果不存在ssh密钥，则新建一个：

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

your_email@example.com替换成我的Github邮箱地址：1521498437@qq.com    随后会让我键入想要保存的ssh key的文件名

```
Enter a file in which to save the key
```

这里一般建议不输入任何文件名，直接回车，这样就使用系统默认的设置。那么在“C:\Users\UserName”文件夹下就会创建.ssh文件夹，文件夹中生成“id_rsa”和“id_rsa.pub”两个文件，分别对应私钥和公钥。

随后复制“id_rsa.pub”的内容到GitHub网站的Settings–>SSH and GPG keys中：

![](./Image1/8.png)

设置title（任意），并将“id_rsa.pub”的内容复制到“Key”之中。

#### ③测试SSH Key是否配置成功

```
ssh -T git@github.com
```

第一次测试，在continue的时候，选择“yes”，即可显示成功认证。

***特别说明：如果在上述创建SSH Key文件的时候键入新的文件名，则需要将新文件挂靠到ssh-agent上，否则会出现“git@github.com: Permission denied (publickey)”的错误。***

详细的操作参见：[Generating a new SSH key and adding it to the ssh-agent](https://help.github.com/en/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

### 4、配置GitHub的用户名和邮箱

使用Git Bash配置本地使用Git的全局设置。

[配置用户名](https://help.github.com/en/articles/setting-your-username-in-git)

```
git config --global user.name “your name”
```

"your name"替换成我的GitHub用户名：1521498437

[配置邮箱](https://help.github.com/en/articles/setting-your-commit-email-address)

```
git config --global user.email “email@example.com”
```

这里"email@example.com"替换成我的Github邮箱地址：1521498437@qq.com

### 5、访问GitHub网站并新建代码仓库

GitHub可以很方便地创建新的代码仓库“New Repository”：

![](./Image1/9.png)

填写仓库的名称【Repository name】为“CloudComputing“，添加描述【Description】，还可以添加README（此处先不添加，后续采用命令行操作），以及添加忽略文件和开源协议。

### 6、创建本地代码仓库

首先在本地规划好一处文件夹用于同步GitHub的项目，我的本地代码仓库文件夹为：C:\Users\Administrator\CLOUD\GithubProgram

然后打开Git Bash，使用“cd”命令，定位到此次我想要同步的这个文件夹：（此时要注意路径名中的斜杆方向）

```
cd C:/Users/Administrator/CLOUD/GithubProgram
```

[接下来将在此文件夹添加刚才创建的GitHub的项目。](https://help.github.com/en/articles/adding-an-existing-project-to-github-using-the-command-line)

#### ①初始化本地文件夹作为一个Git仓库：

```
git init
```

#### ②拷贝GitHub网站中的项目网址：

![](./Image1/10.png)

#### ③添加远程代码仓库的URL：

```
git remote add origin remote_repository_URL
```

`remote_repository_URL`替换为刚才拷贝的项目的URL:

https://github.com/1521498437/CloudComputing.git。

说明，origin指代远程代码仓库（GitHub中），master表示本地的主分支。验证一下添加是否成功：

```
git remote -v
```

#### ④首先从远程代码仓库拉取数据：

```
git pull origin master
```

#### ⑤新建README文档，README文档是每个GitHub项目必备，说明项目内容。上文没有创建，在此处完成。

```
touch README.md
```

#### ⑥添加文件夹中的所有文件：

```
git add.
```

#### ⑦提交文件：

```
git commit -m "First commit"
```

注意commit只在本地提交，并未同步到远程服务器；"First commit"可随意更改为其它说明。

#### ⑧推送本地更新至远程服务器：

```
git push -u origin master
```

注意：这里第4步，首先从GitHub拉取数据，不能忽略这个步骤。这样容易导致出现本地版本和远程版本冲突的困境。详见错误[Updates were rejected because the tip of your current branch is behind](https://stackoverflow.com/questions/22532943/how-to-resolve-git-error-updates-were-rejected-because-the-tip-of-your-current)。

### 7、下载Typora

README.md文档必须由Typora软件来撰写、编辑；

Typora下载链接：https://pc.qq.com/detail/1/detail_24041.html

**注意：**当每次用Typora更新完文档以后，需同步到GitHub中，就要再次执行“cd”和最后三条命令。

## 三、安装VMware Workstation虚拟机并下载创建Cent OS

### 1、安装VMware虚拟机

VMware下载及安装链接：https://blog.csdn.net/empty_ly/article/details/79992611

安装成功并打开

![](./Image1/11.png)



### 2、创建Cent OS操作系统

首先下载CentOS 7镜像文件，下载地址：http://mirrors.aliyun.com/centos/7/isos/x86_64/

点击进入链接后，选择如图所示的CentOS-7-x86_64-DVD-1908.iso来下载

![](./Image1/centos.png)

点击刚刚打开的VMware中的 [创建新的虚拟机]

![](./Image1/12.png)

选择[典型]

![](./Image1/13.png)

如图选择[Linux(L)] ， [CentOS 7 64位]

![](./Image1/14.png)

如图设置【虚拟机名称】、【位置】，建议不要用中文名称，安装路径不要有中文且最好不要安装在C盘系统盘。

![](./Image1/15.png)

设置磁盘容量，可按默认大小设置，并选择 [将虚拟磁盘存储为单个文件(O)]

![](./Image1/16.png)

点击完成。

![](./Image1/17.png)

点开 [编辑虚拟机设置]

![](./Image1/18.png)

在【处理器】栏目中勾选 [虚拟化Intel VT-x/EPT 或 AMD-V/RVI(V)]

![](./Image1/20.png)

在【CD/DVD(IDE)】栏目中点击 [使用ISO映像文件(M)]，并选择先前下载好的CentOS 7镜像文件

![](./Image1/19.png)

开启虚拟机，出现如图界面，敲击回车键进行安装

![](./Image1/21.png)

选择语言

![](./Image1/22.png)

选择【安装位置】

![](./Image1/23.png)

进行【ROOT密码】设置

![](./Image1/24.png)

等待安装完成，重启进入系统

![](./Image1/25.png)











