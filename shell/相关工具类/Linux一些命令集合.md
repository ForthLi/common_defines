# Linux常用命令

Linux下的很多命令，之前接触过，不过不是很熟悉，有些用过，但不是很了解，在这里进行一下汇总和必要的认识学习。

Linux下命令分类:

```
1. 文件管理
2. 文档编辑
3. 文件传输
4. 磁盘管理
5. 磁盘维护
6. 网络通讯
7. 系统管理
8. 系统设置
9. 备份压缩
10. 设备管理

```

还有一些其他的命令，暂时不计。



## 1. 文件管理

### cat

cat命令用于连接文件并打印到标准输出设备上。

使用权限：所有使用者

语法格式:

```shell
cat [-AbeEnstTuv] [--help] [--version] fileName
```

#### 参数说明：

**-n 或 --number**：由 1 开始对所有输出的行数编号。

**-b 或 --number-nonblank**：和 -n 相似，只不过对于空白行不编号。

**-s 或 --squeeze-blank**：当遇到有连续两行以上的空白行，就代换为一行的空白行。

**-v 或 --show-nonprinting**：使用 ^ 和 M- 符号，除了 LFD 和 TAB 之外。

**-E 或 --show-ends** : 在每行结束处显示 $。

**-T 或 --show-tabs**: 将 TAB 字符显示为 ^I。

**-A, --show-all**：等价于 -vET。

**-e：**等价于"-vE"选项；

**-t：**等价于"-vT"选项；

```shell
# 编号输出到 -n
cat -n printf_test.sh 
:<<!
#!/bin/bash
     1	#!/bin/bash
     2	
     3	:<<!
     4		printf命令模仿C程序库(library)里的printf()程序。
     5		语法
     6		printf format-string [arguments...]
     7	!
     ...
 !
 
 # 和 -n 一样，对输出行进行编号：不同点是对空白行不进行编号
 cat -n printf_test.sh 
 
 cat /dev/null > test.txt 
 

```

#### 实例：

把 textfile1 的文档内容加上行号后输入 textfile2 这个文档里：

```
cat -n textfile1 > textfile2
```

把 textfile1 和 textfile2 的文档内容加上行号（空白行不加）之后将内容附加到 textfile3 文档里：

```
cat -b textfile1 textfile2 >> textfile3
```

清空 /etc/test.txt 文档内容：

```
cat /dev/null > /etc/test.txt
```

cat 也可以用来制作镜像文件。例如要制作软盘的镜像文件，将软盘放好后输入：

```
cat /dev/fd0 > OUTFILE
```

相反的，如果想把 image file 写到软盘，输入：

```
cat IMG_FILE > /dev/fd0
```

**注**：

- \1. OUTFILE 指输出的镜像文件名。
- \2. IMG_FILE 指镜像文件。
- \3. 若从镜像文件写回 device 时，device 容量需与相当。
- \4. 通常用制作开机磁片。



### chattr(mac上没有该命令)

Linux chattr命令用于改变文件属性。

这项指令可改变存放在ext2文件系统上的文件或目录属性，这些属性共有以下8种模式：

1. a：让文件或目录仅供附加用途。
2. b：不更新文件或目录的最后存取时间。
3. c：将文件或目录压缩后存放。
4. d：将文件或目录排除在倾倒操作之外。
5. i：不得任意更动文件或目录。
6. s：保密性删除文件或目录。
7. S：即时更新文件或目录。
8. u：预防意外删除。

#### 语法

```
chattr [-RV][-v<版本编号>][+/-/=<属性>][文件或目录...]
```

#### 参数

　　-R 递归处理，将指定目录下的所有文件及子目录一并处理。

　　-v<版本编号> 设置文件或目录版本。

　　-V 显示指令执行过程。

　　+<属性> 开启文件或目录的该项属性。

　　-<属性> 关闭文件或目录的该项属性。

　　=<属性> 指定文件或目录的该项属性。

### 实例

用chattr命令防止系统中某个关键文件被修改：

```
chattr +i /etc/resolv.conf
lsattr /etc/resolv.conf
```

会显示如下属性

```
----i-------- /etc/resolv.conf
```

让某个文件只能往里面追加数据，但不能删除，适用于各种日志文件：

```shell
chattr +a /var/log/messages
```



### chgrp

Linux chgrp命令用于变更文件或目录的所属群组。

在UNIX系统家族里，文件或目录权限的掌控以拥有者及所属群组来管理。您可以使用chgrp指令去变更文件与目录的所属群组，设置方式采用群组名称或群组识别码皆可。

#### 语法

```
chgrp [-cfhRv][--help][--version][所属群组][文件或目录...] 或 chgrp [-cfhRv][--help][--reference=<参考文件或目录>][--version][文件或目录...]
```

#### 参数说明

　　-c或--changes 效果类似"-v"参数，但仅回报更改的部分。

　　-f或--quiet或--silent 　不显示错误信息。

　　-h或--no-dereference 　只对符号连接的文件作修改，而不更动其他任何相关文件。

　　-R或--recursive 　递归处理，将指定目录下的所有文件及子目录一并处理。

　　-v或--verbose 　显示指令执行过程。

　　--help 　在线帮助。

　　--reference=<参考文件或目录> 　把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同。

　　--version 　显示版本信息。

#### 实例

实例1：改变文件的群组属性：

```
chgrp -v bin log2012.log
```

输出：

```
[root@localhost test]# ll
---xrw-r-- 1 root root 302108 11-13 06:03 log2012.log
[root@localhost test]# chgrp -v bin log2012.log
```

"log2012.log" 的所属组已更改为 bin

```
[root@localhost test]# ll
---xrw-r-- 1 root bin  302108 11-13 06:03 log2012.log
```

说明： 将log2012.log文件由root群组改为bin群组

实例2：根据指定文件改变文件的群组属性

```
chgrp --reference=log2012.log log2013.log
```

输出：

```
[root@localhost test]# ll
---xrw-r-- 1 root bin  302108 11-13 06:03 log2012.log
-rw-r--r-- 1 root root     61 11-13 06:03 log2013.log
[root@localhost test]#  chgrp --reference=log2012.log log2013.log 
[root@localhost test]# ll
---xrw-r-- 1 root bin  302108 11-13 06:03 log2012.log
-rw-r--r-- 1 root bin      61 11-13 06:03 log2013.log
```

说明： 改变文件log2013.log 的群组属性，使得文件log2013.log的群组属性和参考文件log2012.log的群组属性相同



### chmod

见[chomod](file:///Users/lijinhu_2/Desktop/私人文件/shell/相关工具类/chmod使用.md)



### chown

Linux/Unix 是多人多工操作系统，所有的文件皆有拥有者。利用 chown 将指定文件的拥有者改为指定的用户或组，用户可以是用户名或者用户ID；组可以是组名或者组ID；文件是以空格分开的要改变权限的文件列表，支持通配符。 。

一般来说，这个指令只有是由系统管理者(root)所使用，一般使用者没有权限可以改变别人的文件拥有者，也没有权限把自己的文件拥有者改设为别人。只有系统管理者(root)才有这样的权限。

**使用权限** : root

#### 语法

```
chown [-cfhvR] [--help] [--version] user[:group] file...
```

**参数** :

- user : 新的文件拥有者的使用者 ID
- group : 新的文件拥有者的使用者组(group)
- -c : 显示更改的部分的信息
- -f : 忽略错误信息
- -h :修复符号链接
- -v : 显示详细的处理信息
- -R : 处理指定目录以及其子目录下的所有文件
- --help : 显示辅助说明
- --version : 显示版本

#### 实例

将文件 file1.txt 的拥有者设为 runoob，群体的使用者 runoobgroup :

```shell
chown runoob:runoobgroup file1.txt
```

将目前目录下的所有文件与子目录的拥有者皆设为 runoob，群体的使用者 runoobgroup:

```shell
chown -R runoob:runoobgroup *
```



### ckksum

Linux cksum命令用于检查文件的CRC是否正确。确保文件从一个系统传输到另一个系统的过程中不被损坏。

CRC是一种排错检查方式，该校验法的标准由CCITT所指定，至少可检测到99.998%的已知错误。

指定文件交由指令"cksum"进行校验后，该指令会返回校验结果供用户核对文件是否正确无误。若不指定任何文件名称或是所给予的文件名为"-"，则指令"cksum"会从标准输入设备中读取数据。

#### 语法

```
cksum [--help][--version][文件...]
```

**参数**：

- --help：在线帮助。
- --version：显示版本信息。
- 文件…:需要进行检查的文件路径

#### 实例

使用指令"cksum"计算文件"testfile1"的完整性，输入如下命令：

```
$ cksum testfile1       
```

以上命令执行后，将输出校验码等相关的信息，具体输出信息如下所示：

```
1263453430 78 testfile1         //输出信息 
```

上面的输出信息中，"1263453430"表示校验码，"78"表示字节数。

**注意：**如果文件中有任何字符被修改，都将改变计算后CRC校验码的值。



### cmp

Linux cmp命令用于比较两个文件是否有差异。

当相互比较的两个文件完全一样时，则该指令不会显示任何信息。若发现有所差异，预设会标示出第一个不同之处的字符和列数编号。若不指定任何文件名称或是所给予的文件名为"-"，则cmp指令会从标准输入设备读取数据。

#### 语法

```
cmp [-clsv][-i <字符数目>][--help][第一个文件][第二个文件]
```

**参数**：

- -c或--print-chars 　除了标明差异处的十进制字码之外，一并显示该字符所对应字符。
- -i<字符数目>或--ignore-initial=<字符数目> 　指定一个数目。
- -l或--verbose 　标示出所有不一样的地方。
- -s或--quiet或--silent 　不显示错误信息。
- -v或--version 　显示版本信息。
- --help 　在线帮助。

#### 实例

要确定两个文件是否相同，请输入：

```
cmp prog.o.bak prog.o 
```

这比较 prog.o.bak 和 prog.o。如果文件相同，则不显示消息。如果文件不同，则显示第一个不同的位置；例如：

```
prog.o.bak prog.o differ: char 4, line 1 
```

如果显示消息 cmp: EOF on prog.o.bak，则 prog.o 的第一部分与 prog.o.bak 相同，但在 prog.o 中还有其他数据。



### diff

Linux diff命令用于比较文件的差异。

diff以逐行的方式，比较文本文件的异同处。如果指定要比较目录，则diff会比较目录中相同文件名的文件，但不会比较其中子目录。

#### 语法

```
diff [-abBcdefHilnNpPqrstTuvwy][-<行数>][-C <行数>][-D <巨集名称>][-I <字符或字符串>][-S <文件>][-W <宽度>][-x <文件或目录>][-X <文件>][--help][--left-column][--suppress-common-line][文件或目录1][文件或目录2]
```

**参数**：

-<行数> 　指定要显示多少行的文本。此参数必须与-c或-u参数一并使用。

-a或--text 　diff预设只会逐行比较文本文件。

-b或--ignore-space-change 　不检查空格字符的不同。

- -B或--ignore-blank-lines 　不检查空白行。
- -c 　显示全部内文，并标出不同之处。
- -C<行数>或--context<行数> 　与执行"-c-<行数>"指令相同。
- -d或--minimal 　使用不同的演算法，以较小的单位来做比较。
- -D<巨集名称>或ifdef<巨集名称> 　此参数的输出格式可用于前置处理器巨集。
- -e或--ed 　此参数的输出格式可用于ed的script文件。
- -f或-forward-ed 　输出的格式类似ed的script文件，但按照原来文件的顺序来显示不同处。
- -H或--speed-large-files 　比较大文件时，可加快速度。
- -l<字符或字符串>或--ignore-matching-lines<字符或字符串> 　若两个文件在某几行有所不同，而这几行同时都包含了选项中指定的字符或字符串，则不显示这两个文件的差异。
- -i或--ignore-case 　不检查大小写的不同。
- -l或--paginate 　将结果交由pr程序来分页。
- -n或--rcs 　将比较结果以RCS的格式来显示。
- -N或--new-file 　在比较目录时，若文件A仅出现在某个目录中，预设会显示：
- Only in目录：文件A若使用-N参数，则diff会将文件A与一个空白的文件比较。
- -p 　若比较的文件为C语言的程序码文件时，显示差异所在的函数名称。
- -P或--unidirectional-new-file 　与-N类似，但只有当第二个目录包含了一个第一个目录所没有的文件时，才会将这个文件与空白的文件做比较。
- -q或--brief 　仅显示有无差异，不显示详细的信息。
- -r或--recursive 　比较子目录中的文件。
- -s或--report-identical-files 　若没有发现任何差异，仍然显示信息。
- -S<文件>或--starting-file<文件> 　在比较目录时，从指定的文件开始比较。
- -t或--expand-tabs 　在输出时，将tab字符展开。
- -T或--initial-tab 　在每行前面加上tab字符以便对齐。
- -u,-U<列数>或--unified=<列数> 　以合并的方式来显示文件内容的不同。
- -v或--version 　显示版本信息。
- -w或--ignore-all-space 　忽略全部的空格字符。
- -W<宽度>或--width<宽度> 　在使用-y参数时，指定栏宽。
- -x<文件名或目录>或--exclude<文件名或目录> 　不比较选项中所指定的文件或目录。
- -X<文件>或--exclude-from<文件> 　您可以将文件或目录类型存成文本文件，然后在=<文件>中指定此文本文件。
- -y或--side-by-side 　以并列的方式显示文件的异同之处。
- --help 　显示帮助。
- --left-column 　在使用-y参数时，若两个文件某一行内容相同，则仅在左侧的栏位显示该行内容。
- --suppress-common-lines 　在使用-y参数时，仅显示不同之处。

#### 实例1：比较两个文件

```shell
[root@localhost test3]# diff log2014.log log2013.log 
3c3
< 2014-03
---
> 2013-03
8c8
< 2013-07
---
> 2013-08
11,12d10
< 2013-11
< 2013-12
```

上面的"3c3"和"8c8"表示log2014.log和log20143log文件在3行和第8行内容有所不同；"11,12d10"表示第一个文件比第二个文件多了第11和12行。

#### 实例2：并排格式输出

```shell
[root@localhost test3]# diff log2014.log log2013.log  -y -W 50
2013-01                 2013-01
2013-02                 2013-02
2014-03               | 2013-03
2013-04                 2013-04
2013-05                 2013-05
2013-06                 2013-06
2013-07                 2013-07
2013-07               | 2013-08
2013-09                 2013-09
2013-10                 2013-10
2013-11               <
2013-12               <
[root@localhost test3]# diff log2013.log log2014.log  -y -W 50
2013-01                 2013-01
2013-02                 2013-02
2013-03               | 2014-03
2013-04                 2013-04
2013-05                 2013-05
2013-06                 2013-06
2013-07                 2013-07
2013-08               | 2013-07
2013-09                 2013-09
2013-10                 2013-10
                      > 2013-11
                      > 2013-12
```

**说明：**

- "|"表示前后2个文件内容有不同
- "<"表示后面文件比前面文件少了1行内容
- ">"表示后面文件比前面文件多了1行内容









###### Tips:

本章节参考网址 [菜鸟-Linux语法大全](https://www.runoob.com/linux/linux-command-manual.html)

