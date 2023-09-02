
### 让boot.s在bochs上跑起来

#### 方案一
```shell
as86 -0 -a -o boot.o boot.s
ld86 -0 -s -o boot boot.o
bximage
dd bs=32 if=boot of=hd.img skip=1 conv=notrunc
```
1. as86命令生成的目标文件不是纯二进制文件，应该是包含了后续链接要用的信息，所以生成目标文件的大小不是512字节的。
2. 通过ld86链接后生成的boot文件也不是512B长度，是因为文件最开始的部分还有32B的MINIX可执行文件的头结构。
3. 所以最后在生成硬盘映像文件的时候，要跳过前面的32B，只拷贝后面的512B部分

#### 方案二

