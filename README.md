# opensuse-15.1-mips64el-diff

**openSUSE 15.1 for mips64el, DIFF ONLY.**

**openSUSE 15.1 for mips64el，只存储修改的差异。**

源代码容量很大，github 这里没法放，需要从 build.opensuse.org 下载。仅仅是 15.1 的正式发布时的版本，除了 binutils 外，没有升级软件包。

二进制包下载地址：

https://pan.baidu.com/s/1iYp_TTvt9MeqhBM2OEZBgw

位于此共享中的 15.1-release 内。除了分卷压缩的 RPM 包外，还有一个做好的 rootfs 压缩包（不含内核），以及龙梦源代码编译的 5.4.38 内核压缩包。

## 说明：
1. obs 不支持用 mips64el （小端）作为架构的设置调用编译，只能用 mips64 （大端）。而且 obs 在处理 spec 时，也会认为系统是 mips64 而进行处理。所以里面的 32 和 64 的架构区别，必须用宏 %mips64 和 %mips32 进行区别保证 obs 的运行正常，而不能直接写 mips64el 和 mips32el 。但是实际上在 rpmbuild 编译软件包时，他是可以识别的，所以详细到编译过程处理，反而可以用。
2. _constraints 和 _service\* 文件的修改请乎略。前者因为我用 chroot 模式运行的 obs ，这个模式不支持这个文件而报错需要删除。后者是我上传给我的本地 obs 时，导致 obs 执行 source service 而卡住。删掉后发现编译又不能离开这些文件，所以又不得不恢复回来。（也就是说，我当初该删的没删，不该删的都删了……）
3. 内核的修改仅仅是为了保证编译，并不能启动系统。对应的也导致了几个依赖内核而编译的包出现了问题。
4. mono 不支持 mips64 ，ldc 不支持 mips 全部。这些语言对应的软件包只能关掉支持，另外 sbcl 不支持 mips64el ，但是 lisp 其实可以用其他编译器。
5. binutils 为了打开 ld.gold 和增加 hash style 的 gnu 支持，升级到了 2.33.1 。差异内容也是针对这个版本做的。但是反而又出现了 ld.gold 无法正确运行的问题。所以在软件包编译时，gold 链接器不能用，都必须关闭。
6. 因为设定 -march=mips64r2 -mabi=64 ，没做支持 o32 和 n32 的内容。所以在龙芯系列等等 CPU 上并不能实现完全性能，而且因为龙芯的融合浮点乘加指令和 mips64 r2 的实现不一样，在 gcc 上也增加了 --with-madd4=no 来保证兼容性。所以原则上说，所有的 mips64r2 的 CPU 都应该可以用。
7. 基于兼容 mips64r2 的原因， ffmpeg 关掉了龙芯 mmi 等等的 mips 扩展指令支持。多媒体播放性能可能有影响。
8. java 用的都是原版源代码编译的，所以运行模式是基于 zero 版本的 interpreted 模式。
9. gcc 的 ada 、 java 的 bootstrap 、 ghc 的 bootstrap 都需要外部提供编译环境实现需要的功能。我是用 debian 的软件包解压缩到临时的 opensuse 环境中编译出来的。另外，java 的 bootstrap 需要的版本，在 build.opensuse.org 上已经被删掉了。我使用的这个版本的源代码是从已经删除的 java 1.7 版本包的数据中下载回来的。
