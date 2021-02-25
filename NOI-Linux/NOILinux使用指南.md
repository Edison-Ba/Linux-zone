# [博客园直链地址](https://www.cnblogs.com/EdisonBa/p/NOI-Linux.html)

（因为有些东西 luogu 的博客加载不出来，所以建议去博客园欣赏)

（不要忘记点赞再走）

![](https://i.loli.net/2019/02/23/5c7156985bb5a.png)![](https://i.loli.net/2019/02/23/5c7156985bb5a.png)![](https://i.loli.net/2019/02/23/5c7156985bb5a.png)

---

[TOC]

### 关于安装 NOI Linux

这里请参考 [NOI官方公告](http://www.noi.cn/gynoi/jsgz/2018-08-21/710467.shtml)

下载 NOI Linux 光盘映像文件，之后按照安装说明文档进行安装。

如果将其安装为虚拟机，推荐使用 Vmware。创建虚拟机的过程中您可能会出现一系列问题，您可以根据具体问题自行百度。

### 系统配置

经过漫长的安装过程，终于到了开机界面。

这里的默认密码为 123456 。

![开机](https://cdn.luogu.com.cn/upload/image_hosting/8i8fht6a.png)

#### 网络

开机之后，如果你可以联网的话当然要先联网。

如果你的 NOI Linux 是虚拟机，并且连不上网，这多半是虚拟机的问题，不是系统的问题。请参照[这个](https://blog.csdn.net/qq_36838191/article/details/88634351)尝试修复。

当然，如果对你来说联网有点困难，不联网也是可以进行编程的（真正考试的时候也不会让你联网）。

#### 输入法

NOI Linux 是自带中文输入法的。这非常的友好。

当你想要使用中文时，在输入框下，只需要按 Ctrl + Shift 就可以切换为中文了。

![中文](https://cdn.luogu.com.cn/upload/image_hosting/uhlyc0zh.png)

### 编辑器

#### 1. gedit

NOI Linux 有许多编辑器（不是编译器），经过一番初体验，我觉得 gedit 还是比较阳间的。

##### 打开

下面这张图里放的主要是系统自带的编译器及编辑器还有评测系统。但是这里面的编辑器和编译器用起来实在是令人窒息，全都没有括号补全功能，而且有的编辑界面令人不忍直视。

![编辑器](https://cdn.luogu.com.cn/upload/image_hosting/zjym9pck.png)

接下来我要讲的 gedit 不在上图中，打开方式如下：
1. 右键桌面，新建空白文档，**命名为 work.cpp** （必须）。
   
   ![txt](https://cdn.luogu.com.cn/upload/image_hosting/fgskemj6.png)

2. 右键新建的文档，在弹出的框框中如果第一个就是 gedit，那么直接点击。如果不是 gedit，则查找其他应用程序，选择 gedit。
   ![more](https://cdn.luogu.com.cn/upload/image_hosting/19j9ds04.png)
   ![gedit](https://cdn.luogu.com.cn/upload/image_hosting/qqcmw4fz.png)

##### 配置

打开 gedit 之后，直接用可能会有点不舒服，你可以按照 /编辑/首选项 来把编辑器改成你想要的风格。
另外，里面有个自动保存的功能，建议小于十分钟保存一次。

![首选项](https://cdn.luogu.com.cn/upload/image_hosting/r6v97h7r.png)

注意：这个编辑器也没有括号自动补全的功能，这也就需要选手熟悉没有括号补全的编辑器。

##### 外观展示

![code1](https://cdn.luogu.com.cn/upload/image_hosting/ax1hcy3f.png)

#### 2. vim

这个东西非常强大，可以实现括号补全，但是需要自己配置，配置起来比较麻烦。

##### 打开

在 NOI Linux 下，可以使用终端打开 vim 。

1. Ctrl + Alt + T 打开终端
2. 输入 `vim` 之后回车
   ![vim](https://cdn.luogu.com.cn/upload/image_hosting/tsy0ika5.png)
   ![vim2](https://cdn.luogu.com.cn/upload/image_hosting/h3k9mt34.png)

##### 配置
之后，如果你想要继续学下去的话，可以参照大佬们的博客。

[https://www.cnblogs.com/heyboom/p/10522059.html]()

在这里，关于 vim 的详细用法，我就不再讲解。

### 使用 makefile 编译运行 

#### 1. 编写 makefile
NOI Linux 下的编译方法有很多。这里，我只讲一个较为简洁而方便的方法，那就是 **makefile** 。

我们以桌面为工作区。
工作区就是你写代码，编译的地方。

先在桌面上建一个文件，命名为 `makefile` ，没有扩展名，将这个文件和 `work.cpp` 放在同一目录下。

![makefile1](https://cdn.luogu.com.cn/upload/image_hosting/khfvy5mw.png)

使用 gedit 打开 `makefile`，在里面输入以下内容：
```js
名字随便起:work
	g++ work.cpp -o work
	./work
```
注意： 
1. 第一行的 `名字随便起` 是可以替换的。例如，你可以将它换成 `qwq` 等，甚至它的名字就可以是 `名字随便起`。
2. 第二行和第三行之前是一个 Tab 键，并不是几个空格。你可以将上面 Tab 字符复制过去。


保存并关闭文件。

#### 2. 修改终端默认路径
之后，我们需要将打开终端时的默认路径修改为我们的工作区，也就是桌面。操作方法如下：

1. 打开 `/Home/` 
   ![home](https://cdn.luogu.com.cn/upload/image_hosting/gwrytciv.png)
2. 显示隐藏文件
   ![34](https://cdn.luogu.com.cn/upload/image_hosting/ev8w1lnr.png)
3. 找到文件 `.bashrc` 并用 gedit 打开。
4. 在末尾添加
   ```
   cd /home/noilinux/Desktop/
   ```
   

   效果如下图：
   ![cd](https://cdn.luogu.com.cn/upload/image_hosting/rs7hyj4o.png)

   保存并关闭文件。

之后，我们可以愉快地编译了！

注意：如果你不想把工作区放在桌面，而是某一个文件夹，这个路径你可以改为你的文件夹地址。

#### 3. 编译
首先确保 `work.cpp` 没有编译错误，再按 Ctrl + alt + T 打开终端，输入 `make` 之后回车 。

效果如下：
![makeok](https://cdn.luogu.com.cn/upload/image_hosting/e3fxu1hs.png)


注意：你的程序输出最后需要额外回车一行，否则你输出之后会造成一些无法描述的情况，看起来特别☹️️。

**结尾不回车**的样子：
![non](https://cdn.luogu.com.cn/upload/image_hosting/btiggki2.png)

如果说我们编译错误了，终端会返回编译错误的信息。

**编译错误**的样子：
![error](https://cdn.luogu.com.cn/upload/image_hosting/i5qq5yj6.png)

当然，编译的时候我们只能用  `work.cpp` 这一个文档。正常考试的时候有多道题。我们可以打完一个题之后，将程序另存为另一个文件。然后把 `work.cpp` 清空，继续编写下一个题。

总之，我们只能编译 `work.cpp` 这个程序。而编译过程只需要打开终端，输入一个 `make` 指令，就可以完成编译了。编译的过程顺便也运行了以下程序。是不是很方便快捷呢？！

#### 4. 编译开关

如果说，我们需要显示所有警告信息；或者题目非常优秀，给我们开 C++11 , 而且给我们开 O2 优化。那么，这些编译开关我们怎么自己开呢？

命令行编译的基本格式：
```
g++ work.cpp -o work 编译开关
```
那么，我们只需要在 `makefile` 里面添加编译开关即可。

比如说要添加
```
-Wall -Wextra -std=c++11 -Ofast
```

正确的操作是这样的：
![kaiguan](https://cdn.luogu.com.cn/upload/image_hosting/j76dyx8r.png)

保存文件并退出。

接下来，我们试一下编译开关有没有开上。

我们知道，当我们在程序里定义了一个变量，如果接下来变量没有使用，那么它会发出警告。我们可以利用这个特性来判断我们有没有开上编译开关。

![suchas](https://cdn.luogu.com.cn/upload/image_hosting/hjk3ycpd.png)

打开终端，输入 `make` 后回车。

如果你的终端像下图这样，那么你就成功了。

![wall](https://cdn.luogu.com.cn/upload/image_hosting/omg9d3u7.png)

至此，我们就完成了编辑、编译和运行的工作。这样几乎已经满足了考试的需要。

### 总结

关于 NOI Linux 的使用，我就先探索到这里。

这篇博客仅仅是一个快速入门指南，主要是为了帮助没有用过该系统的选手快速适应环境。
如果你需要 GDB 调试，你可以使用 GUIDE ，当然教程需要上网搜索大佬的博客……

如果你有更好的建议，或者我的表述出现了某些问题，欢迎在下方评论留言。

2021 是充满机遇的一年。期待在这一年里大家学业有成，事业顺利，**RP++**！




<p align="right">EdisonBa</p>

<p align="right">2021.2.1 初次编辑</p>
