# ffmpeg

## 写在前面

​	其实真不是我不想写，主要是一两天抠出来一个项目不太现实，所以第一周的计划就有些杂乱，虽然今天也准备docker部署一个好玩儿的项目就跑路，结果你猜怎么着，服务器他跟我对着干:cry:,部署成功之后就是用IP访问不了，虽说用`vsCode`自带的浏览器也可以访问，但是作为一个刚刚幼儿园毕业的小菜鸡来说，技术可以没有，但是心高气傲是一定要有的，所以我就开始:surfing_man:寻找原因，把可以试的都试了一遍，结果就是响应不了，可是推文总得写吧。于是，我就去回归我`CS61C`得怀抱了。这不，今天的主角就出场了。

## ffmpeg

![image-20221015171526057](https://s2.loli.net/2022/10/15/Tu19RDOlo7mrVHE.png)

​	看看这朴实的外表，肯定隐藏着一些好玩儿的内容。还是得提一嘴，为啥会用到它，因为呢，官方原话长下面这样

![image-20221015171708242](https://s2.loli.net/2022/10/15/KnUIDgMsxpot4aN.png)

就是这个实验比较好玩儿的地方是他可以做出一个以方程变化到图片变化的效果，然后呢，如果把每一帧都拼接起来就成为了一个不错的视频，所以，我就来下载他了。

​	所以还是正事儿要紧，先跑完实验，再来拓展一下它的功能。

​	在开始用之前，首先是安装，做为一向友好的`Linux`大哥，`yum`带给我的从来都只有惊喜，但是这次为啥`yum install ffmpeg`安装好之后不能用啊。`bing`了一下之后是因为版本太老了，所以就只好自己编译安装了。先`clone`一下，然后……，因为他有点大，所以先等会儿，去做个核酸先。

> git 仓库地址 https://git.ffmpeg.org/ffmpeg.git 

进入文件夹内部`cd ffmpeng`，然后执行`./configure`，如果这个时候有一行红字出现在你的显示器上

> FFmpeg yasm/nasm not found or too old. Use --disable-yasm for a crippledbuild

是的，你又要安装别的环境了，yasm是汇编编译器，ffmpeg为了提高效率使用了汇编指令，如MMX和SSE等。所以系统中未安装yasm时，就会报上面错误。但同样也可以不用安装，直接`--disable-yasm`。

安装yasm命令是`yum install yasm`

然后就可以编译安装了，首先编译`make`，期间可以去吃个饭再回来，因为我就是这么干的，然后再进行安装`make install`，安装完成后呢要把`ffmpeg`复制到bin目录，来方便直接使用`ffmpeg`命令,使用`cp ffmpeg /usr/bin/ffmpeg`，最后查看一下有没有安装成功`ffmpeg -version`。

![image-20221015173535312](https://s2.loli.net/2022/10/15/A38g2TPkCsZ4W9j.png)

我先试了一下`TestB2`然后得到了五张ppm，但是我依旧心满意足，觉得还不错，有兴趣的朋友可以去看一下`official website`

![image-20221015174845543](https://s2.loli.net/2022/10/15/kxYnTQ3v5VoZXwG.png)

> CS61C pro1  https://inst.eecs.berkeley.edu/~cs61c/fa19/projects/proj1/

然后我试着转换了一下，把.mp4从服务器上下载下来后拖入一个某视频剪辑软件，然后我知道我成功了。他是真的可以输出一个视频的。

<img src="https://s2.loli.net/2022/10/15/sQV1W8EUhvHf6mL.png" alt="image-20221015174732891" style="zoom: 50%;" />

无奈，这些太短，都不够我看的，根据官方大大给的提示，让我再去跑个步，让电脑把剩下的大测试跑完。

![image-20221015175216499](https://s2.loli.net/2022/10/15/quc1TWhJNdZU5Io.png)

最后奉上大作。:dog::dog::dog:（狗头保命）

![image-20221015205149487](https://s2.loli.net/2022/10/15/Unk14T2zuWC5NsA.png)

![image-20221015205220493](https://s2.loli.net/2022/10/15/tdchnLMJxzbfk4Y.png)

终于啊，再也不敢了，好在最后终于出来了。

![img](../../公众号/220px-Mandelbrot_sequence_new.gif)

以同样的结尾结束今天的分享，希望各位都可以收获今天心满意足的自己。:gift_heart: