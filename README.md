# Kirikiri2Compile
## 准备原料：

1. github上的krkr2源码一份：https://github.com/krkrz/krkr2
2. 工具包：nasm和ActivePerl
3. 第三方库：boost库（注意，版本必须为1.30）
4. Borland C++ Builder 5.0（安装时必须安装源码）
5. VisualStudio的命令行工具（可选）

上面的2，3，4中提到的我这里放个下载地址：

链接：https://pan.baidu.com/s/13z59yWcSzDS0DKvsLjtguA

提取码：sora

## 准备工作（以下操作最好都在管理员权限下进行，避免出什么莫名其妙的错）：

先安装一下ActivePerl，因为这东西在安装的时候还会下载东西，而且是外网，很慢，装不成多试几遍，或者开个梯子，我是开梯子的，这东西没啥好说的，除了安装路径可以按照自己喜欢的改，其他的就一直next下去就可以了。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img1.jpg)

然后，把nasm解压出来，在原目录创建一个名字叫nasmw.exe的快捷方式指向nasm.exe，或者你也可以把nasm.exe复制一份出来，改名叫nasmw.exe：

![不过我喜欢装臭逼打命令：mklink nasmw.exe nasm.exe](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img2.jpg)

在nasm的目录下创建好nasmw.exe以后，把这个目录配到环境变量PATH中，确保在命令行下敲出nasmw回车有东西出来。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img3.jpg)

然后再安装Borland C++ Builder 5.0（以下简称bcb5），用虚拟光驱加载iso文件，这个是必要的，不要把iso文件里的东西解压出来安装，会安装失败。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img4.jpg)

安装序列号在压缩包里有，打上去就是了

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img5.jpg)

然后后面都按照默认的安装next下去就行了，除了安装路径可以改以外，其他都别改，这里还是要把bcb的源码给装上的，因为后面需要改动这部分的源码重新编译。

安装完bcb5以后，在命令行中打出make，确保有东西出现。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img6.jpg)

然后解压boost库，这里只要编译regex库就可以了，去到\libs\regex\build中，启动命令行，打出命令：make -fbcb5.mak

![警告多没关系，能用就是了](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img7.jpg)

警告多没关系，能用就是了

回车以后，稍等一会编译完成，进到\libs\regex\build\bcb5文件夹中，有一个叫boost_regex_bcb5_mss.lib的静态链接库

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img8.jpg)

拷出来，放到krkr2源码的\kirikiri2\trunk\kirikiri2\src\core\environ\win32\bcb2006\lib里，这里面已经有一个boost_regex_bcb2006.lib了，无视，把bcb5的也放进来

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img9.jpg)

然后，打开到你的bcb5的安装目录，去到\Borland\CBuilder5\Lib下，把krkr2工程下的\kirikiri2\trunk\kirikiri2\src\core\environ\win32\bcb2006\lib里，除了boost_regex_bcb2006.lib以外的所有静态链接库都都拷到bcb5下的\Borland\CBuilder5\Lib里面去。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img10.jpg)

再去到bcb5的安装目录下的\Borland\CBuilder5\Include，然后把krkr2工程下的\kirikiri2\trunk\kirikiri2\src\core\environ\win32\bcb2006\include中所有的文件夹都拷到\Borland\CBuilder5\Include里面去。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img11.jpg)

然后，开始重新编译bcb5的RTL，先打开krkr2工程下的\kirikiri2\trunk\kirikiri2\src\core\base\win32\SysInitIntf.cpp文件，看到第150行这里的地方。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img12.jpg)

这里有两段代码需要插入到bcb5的RTL中进行重新编译的，打开bcb5的安装目录下的RTL位置\Borland\CBuilder5\Source\RTL\source\except，这个目录下有个xx.cpp的文件。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img13.jpg)

打开，找个地方插入上面红色框的代码。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img14.jpg)

然后找到getExceptionObject函数，在最后return前加上黄色框的代码。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img15.jpg)

保存退出后，把bcb5的RTL目录下的tool文件夹\Borland\CBuilder5\Source\RTL\tools配到环境变量PATH中，返回上一级，去到bcb5的RTL目录下\Borland\CBuilder5\Source\RTL，这里有个build.bat，以管理员权限的命令行执行，对RTL进行重新编译。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img16.jpg)

稍等几分钟等编译完成，然后把RTL文件夹下的lib目录\Borland\CBuilder5\Source\RTL\lib下的所有文件，拷到bcb5安装目录下的lib目录\Borland\CBuilder5\Lib下，直接覆盖即可。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img17.jpg)

然后修改krkr2工程下的\kirikiri2\trunk\kirikiri2\src\core\environ\win32\tvpwin32.bpr的第78行和第79行的LIBFILES标签成下面这个样子。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img18.jpg)

其实就是把前面几个相对路径的点改成$(BCB)这个值，然后下一步把krkr2工程下的\kirikiri2\trunk\kirikiri2\src\core\environ\win32\_release.bat的33行，换成bcb5的安装路径下的bin目录中的bcb文件。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img19.jpg)

下一步把krkr2工程下\kirikiri2\trunk\kirikiri2\src\core\base\win32\makestub.pl文件中327行中的正则表达式，改为：/\/\*\[C\*\/(.*?)\/\*C]\*\//gs

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img20.jpg)

好了，到这里，krkr2的编译准备工作就完成了，下面可以开始编译了。找到krkr2工程下的\kirikiri2\trunk\kirikiri2\src\core\environ\win32\release.bat，在管理员权限下的命令行下执行它

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img21.jpg)

按照提示操作，敲回车就可以了，它会自己启动bcb5并且开始编译，编译完成后bcb5会自动关闭，再按照提示敲多几次回车就可以了。

![img](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img22.jpg)

到这里，krkr2就编译完成了，具体输出位置在\kirikiri2\trunk\kirikiri2\bin\win32下

![版本号是2.31的，好像比最新的2.32低了一个版本，不过差别不大](https://github.com/Yamilemon/Kirikiri2Compile/blob/main/img23.jpg)

到这里的话，其实还有一步是需要做的，就是适配高dpi，可以看到上面的目录下有个highdpi.bat，不过那玩意需要在VisualStudio的命令行工具下才能执行，普通的命令行执行不了，由于这玩意装着挺大的，就算了，这里也不放入到编译所需工具链中了，有条件的自己装完VS以后执行一下吧。

简单解释一下这东西就是，在一些高分辨率但是低屏幕尺寸的笔记本里，通常都会把屏幕进行缩放，执行了这个highdpi.bat以后会在krkr2的主程序后面塞进一个配置文件，有了这个配置文件以后，krkr的窗口画面才会跟随着屏幕的缩放而进行缩放，没有的话是不会进行这个缩放的，所以不执行这个的话，这会导致在高分辨率下窗口没跟着缩放而导致游戏画面看起来非常小。
