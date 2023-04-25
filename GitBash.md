## Git Bash配置与第一次上传

安装好了之后，初始是长这个样子的【用户名@电脑型号】

<img src="C:\Users\ZMC\AppData\Roaming\Typora\typora-user-images\image-20230425164131990.png" alt="image-20230425164131990" style="zoom:125%;" />

### 设置用户

首先给电脑设置一个用户，告诉远程仓库是谁上传的

```
git config --global user.name "Your name"
git config --global user.email "email@example.com"
```

**ctrl+v不能粘贴？试试进设置（Options->Keys）里把这个![image-20230425183600549](C:\Users\ZMC\AppData\Roaming\Typora\typora-user-images\image-20230425183600549.png)勾上然后ctrl+shift+v**

### 本地文件操作

#### 进入文件夹

比如我要打开之前的某次作业文件夹 D:\homeWork\sem2\BigHW4

1. 进入文件夹，空白处右键选择 “Git Bash Here”

<img src="C:\Users\ZMC\AppData\Roaming\Typora\typora-user-images\image-20230425165955701.png" alt="image-20230425165955701" style="zoom:50%;" />

2. 在Git Bash中输入路径

   ```
    cd d:/homeWork/sem2/BigHW4
   ```

   **注意：**这里要使用斜线"**/**"而非反斜线"**\\**"

#### 查看

查看当前目录（但其实上一行的黄字就能看到）

```
pwd
```

查看当前文件夹有哪些文件

```
ls
```

<img src="C:\Users\ZMC\AppData\Roaming\Typora\typora-user-images\image-20230425170726126.png" alt="image-20230425170726126" style="zoom:75%;" />

#### 退出

回退至上一文件夹

```
cd .. 
```

#### 新建

（只能）新建文件夹

```
mkdir "foldername"
```

（只能）新建文件

```
touch "filename"
```

删除文件

```
rm "filename"
```

删除文件夹（要先回到上一级）

```
rm -r "foldername"
```

### 仓库设置

大的要来咯

#### 初始化本地仓库

进入要建立仓库的文件夹，有没有东西无所谓，初始化一下：

```
git init
```

文件夹中会出现名为.git的隐藏文件夹![image-20230425171811634](C:\Users\ZMC\AppData\Roaming\Typora\typora-user-images\image-20230425171811634.png)

Github中选择new repository建个仓库

<img src="C:\Users\ZMC\AppData\Roaming\Typora\typora-user-images\image-20230425171955585.png" alt="image-20230425171955585" style="zoom:80%;" />

#### 建立连接

##### SSH Keys配置

先检查一下电脑上有没有SSH Keys:

```
~/.ssh ls
```

没有的话就要新建一个

```
ssh-keygen -t rsa -C "email@example.com"
```

会显示这个：

```
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/ZMC/.ssh/id_rsa):
```

直接回车就行，默认生成id_rsa和id_rsa.pub两个密钥文件

然后需要设置密码，可以直接回车就是不设置，不麻烦

创建成功了他会展示给你看密钥长啥样

<img src="C:\Users\ZMC\AppData\Roaming\Typora\typora-user-images\image-10086.png" style="zoom:67%;" />

添加SSH Key到Github

把.pub文件内容复制到剪贴板上

```
pbcopy < ~/.ssh/id_rsa.pub
```

pbcopy不好使的话直接去打开文件复制

在Github的设置中找到 SSH and GPG keys

选择New SSH key把复制的密钥丢进去

建立完成后可以测试一下

```
ssh -T git@github.com
```

按要求输入yes

<img src="C:\Users\ZMC\AppData\Roaming\Typora\typora-user-images\image-20230425175043226.png" alt="image-20230425175043226" style="zoom:67%;" />

##### SSH 连接

这一步完全按照Github上的提示做就好

在刚建的repository里复制链接

注意：只有你是仓库主人时才能用SSH连接，项目成员只能用HTTPS连接，但操作其实都一样

<img src="C:\Users\ZMC\AppData\Roaming\Typora\typora-user-images\image-20230425175441639.png" alt="image-20230425175441639" style="zoom:60%;" />

在Git Bash里这样

```
git remote add "rname" "link"
```

rname是远程仓库的名字，默认是origin，link就是刚才复制的那段链接

建了也没个提示，试试：

```
git remote -v
```

可以看到叫origin的仓库，一个push和一个fetch

如果不想再连接这个仓库了：

```
git remote remove "rname"
```

### 文件操作

#### 文件上传

将要上传文件添加到暂存区：

```
git add "filename"
```

提交文件夹中所有的文件（包括新文件和被修改的文件，但不包括被删除的文件）：

```
git add .
```

将被修改和被删除的文件提交（不提交新的文件）：

```
git add -u
```

提交所有变化（可以看作是以上两个功能合成）：

```
git add -A
```

将暂存区文件保存到仓库的历史记录中，不加-m的话会进入vim（默认是vim，也可以改成Vscode之类的）

```
git commit -m "notation"
```

把文件推到远程仓库：

```
git push -u "rname" "branch"
```

比如现在只有主分支，那么写master就好了，或者按Github推荐的写个main，合作项目的时候往自己的分支上推

第一次推送master时加上-u参数才会把本地和远程的master分支关联起来，以后就只需要：

```
git push "rname" "branch"
```

就好了，Github上也能看到文件放上去了

<img src="C:\Users\ZMC\AppData\Roaming\Typora\typora-user-images\image-20230425181603159.png" alt="image-20230425181603159" style="zoom:80%;" />

#### 文件修改、下拉

先查看一下提交记录：

```
git log
```

想改commit：

```
git commit --amend -m "new notation"
```

想改文件名，按照上面步骤重新走一遍就好了，报错的话就是因为你本地仓库变了，加个-f，强制推送：

```
git push "rname" "branch" -f
```

但小组合作的时候可不敢这样干，因为变化的大概率是远程仓库，强制推送了是会被组员杀掉的，正确方法是先把文件pull下来，保证自己的文件和远程仓库一致：

```
git pull "rname"
```

or

```
git fetch "rname"
git merge "rname"
```

#### 文件克隆

<img src="C:\Users\ZMC\AppData\Roaming\Typora\typora-user-images\image-20230425182929514.png" alt="image-20230425182929514" style="zoom:67%;" />

在你想要克隆到的位置打开Git Bash

```
git clone "link"
```