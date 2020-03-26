OrangeOS
===

1. nasm下载地址
   https://www.nasm.us/pub/nasm/releasebuilds/2.14.03rc2/
   > 我下载的是上边这个版本，还有其他很多版本选择。
2. 原书光盘资源下载地址
   https://blog.csdn.net/qq_37422196/article/details/81840274?
   depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.
   pc_relevant.none-task
3. bochs下载地址
   https://sourceforge.net/projects/bochs/files/bochs/2.6.11/
   bochs-2.6.11.tar.gz/download
   > 国内网半天转不出来，然后到别的地方下载的

-------------------------------------------------------------------------------
1. error

```
Bochs is exiting with the following message:
[      ] bochsrc:29: keyboard directive malformed.
```
--修改
将bochsrc中键盘映射方式修改为
```
keyboard: keymap=$BXSHARE/keymaps/x11-pc-us.map
```
--我的bochsrc文件配置
```
###############################################################
# Configuration file for Bochs
###############################################################

# how much memory the emulated machine will have
megs: 32

# filename of ROM images
romimage: file=$BXSHARE/BIOS-bochs-latest
vgaromimage: file=$BXSHARE/VGABIOS-lgpl-latest

#romimage: file=/usr/share/bochs/BIOS-bochs-latest
#vgaromimage: file=/usr/share/vgabios/vgabios.bin
#vgaromimage: file=/usr/local/share/bochs/VGABIOS-lgpl-latest

# what disk images will be used
floppya: 1_44=a.img, status=inserted

# choose the boot disk.
boot: floppy

#where do we send log messages?
log: bochsout.txt

# disable the mouse
mouse: enabled=0

# enable key mapping, using US layout as default.
#keyboard: enabled=1, map=/usr/share/bochs/keymaps/x11-pc-us.map
keyboard: keymap=$BXSHARE/keymaps/x11-pc-us.map
```
-------------------------------------------------------------------------------
2. 在bochs中敲下`dump_cpu`显示了`语法错误信息`

   bochs 2.3.5以上版本没有dump_cpu了。

3. 3.1.1 保护模式的运行环境

 1. 问题: 加入
    ```
    floppya: 1_44=freedos.img, status=inserted
    floppya: 1_44=pm.img, status=inserted

    # choose the boot disk.
    boot: a
    ```
    后输入`bochs`后启动失败，显示的错误信息:
    ```
    No bootable device.
    ```
 2. 原因:
    第2个`floppy`是`floppyb`，我又给写成了`floopya`.
 3. 解决方法:
    改过来即可。
4. 执行`sudo mount -o loop pm.img /mnt/floppy`后出现错误: `mount: 挂载点不存在`
 1. 解决方法:
    在`/mnt`下新建一个`floppy`目录
