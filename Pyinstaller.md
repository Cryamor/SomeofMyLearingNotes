### Pyinstaller打包Python多文件流程

#### 准备步骤

安装pyinstaller:

```
pip install pyinstaller
```

看这个文件夹，需要打包的内容是asr.py（main文件）和asrInterface.py（GUI界面文件）两个文件，程序内有用到icon和audio两个文件夹内的东西，别的不需要（懒得新开一个文件夹写了）

<img src="./assets/image-20230427163958256.png" alt="image-20230427163958256" style="zoom: 67%;" />

#### 生成spec文件

spec是安装配置文件，作用是对可执行文件进行配置。

```
pyinstaller -F asr.py
```

有需要就添加的参数：

- -F 表示覆盖
- -w 表示不要控制台窗口
- -c xxx.ico 表示用xxx.ico作为icon图标

成功后会有提示：

<img src="./assets/image-20230427164741338.png" alt="image-20230427164741338" style="zoom:67%;" />

这时可以看到多出了一个dist文件夹和一个spec文件

<img src="./assets/image-20230427164832408.png" alt="image-20230427164832408" style="zoom:67%;" />

dist文件夹里会有一个exe文件，但现在还用不了

#### 编辑spec文件

打开spec文件，把所有要用的python程序写进对应的位置

<img src="./assets/image-20230427165059567.png" alt="image-20230427165059567" style="zoom:48%;" /><img src="./assets/image-20230427165154281.png" alt="image-20230427165154281" style="zoom: 48%;" />



#### 对spec文件打包

```
pyinstaller asr.spec
```

这里不要加 -F，否则会报错

#### 取出exe文件

dist文件夹中的文件现在就可以正常运行了，但图片和音频信息没法正常显示，这是因为exe外面多套了一层dist，把它取出来就可以正常显示了，如果要发给别人或者放在别的地方看，记得把那两个文件夹一起带走。