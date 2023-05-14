### VS 访问 Oracle 数据库

### ODAC安装

**ODAC (Oracle Data Access Components)**顾名思义是用来访问`Oracle`数据库的程序，可以实现在没有安装`Oracle`数据库软件的电脑上完成对其的访问。

> ODAC includes Oracle Data Provider for .NET, Oracle Developer Tools for Visual Studio (ODT), Oracle Providers for ASP.NET, .NET stored procedure support, as well as additional Oracle data access software for Windows.

—`Oracle`官网

在[ODAC下载地址](https://www.oracle.com/database/technologies/net-downloads.html)找到你想要的版本

以下载`ODAC-21.7-Xcopy-64-bit`为例：

##### 安装ODAC包

解压安装包后，在`cmd`中输入

```
install.bat all D:\OracleClient ODAC
```

- 第一个参数`all`代表安装文件夹下的所有组件

- 第二个参数`D:\OracleClient`是安装路径，根据实际情况修改

- 第三个参数`ODAC`，这个叫`ORACLE HOME NAME`，这个参数也可以自己随便指定一个字符串，是用来写入注册表的。比如，上面这条语句执行后，会在注册表的以下位置写入`HKLM\Software\Oracle\KEY_ODAC`，这`KEY_`后面的`ODAC`就是你在参数中传入的那个`ODAC`
- 第四个参数一般不使用，是在安装组件的时候会自动把它依赖的组件都安装上，不想装的话可以传入`false`

可能出现**错误：拒绝访问**，这里我用管理员身份运行`cmd`解决了，安装完成不会有提示。

##### 设置环境变量

- 添加一个环境变量`ORACLE_HOME`，值为之前的安装路径，如我这里就是`D:\OracleClient`
- 在`Path`中添加两个路径，用分号隔开：

```
%ORACLE_HOME%;%ORACLE_HOME%\bin
```

##### 配置`tnsnames.ora`文件

如果要用组件访问Oracle数据库，那么就要根据需要配置`tnsnames.ora`文件，并存放于`%ORACLE_HOME%\network\admin`目录下。

从网上偷的的`tnsnames.ora`文件格式如下：

##### 卸载ODAC

`cmd`中运行

```
uninstall.bat all ODAC
```

或

```
uninstall.bat all D:\OracleClient
```

再手动删除环境变量