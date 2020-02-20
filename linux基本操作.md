# Linux基本操作

### 查看文件主要命令cat

- cat命令：是一个文本查看和链接工具，从第一个字节2开始正向查看内容，使用与小文件，以第一行开始，显示没有停顿，如果文档长的话只能看到最后一页在屏幕上，当文件过多时，就会出现一个问题因为文本在屏幕上迅速闪过，用户来不及看清内容，因此，当文件较大的时候常用more来代替，以避免屏幕滚动太快无法看清，

  说明：把文档串联后输出到基本输出或者（用>filename 重定向到另一个文件中）

  主要参数：

  ```shell
  -b 和-n相似只不过此参数对空白行不编号，对非空行输出行编号，行号从一开始
  -s 当遇到连续空行时只用一行空行
  -e 等价于vE
  -E 每行结束时显示$
  -n 由1开始对所有输出的行数进行编号，对输出的所有行编号，cat -n filename与nl filename相同
  -t 与 -vT 等价
  -T 将跳字符显示为^I
  -v 使用 ^ 和 M- 引用，除了 LFD 和 TAB 之外，cat -v;显示一些控制字符
  ```

  cat主要有三大功能：

  　　1. 一次显示整个文件。如：# cat filename

  　　2. 从键盘创建一个文件（只能创建新文件，不能编辑已有文件）。如：# cat > filename

    3. 将几个文件合并为一个文件。如:

       ```shell
       # cat file1 file2 > file3
       ```

  　　4. cat 有创建文件的功能，创建文件后，要以EOF或STOP结束:

       ```shell
       # cat >  linuxsir.org.txt  << EOF             注：创建linuxsir.org.txt文件；
       > 我来测试 cat 创建文件，并且为文件输入内容；       注：这是为linuxsir.org.txt文件输入内容；
       > 北南南北 测试；                               注：这是为linuxsir.org.txt文件输入内容；
       > EOF                                         注：退出编辑状态；
       # cat linuxsir.org.txt                        注：查看一下linuxsir.org.txt文件的内容；
       我来测试 cat 创建文件，并且为文件输入内容； 北南南北 测试；
       ```

  　　5. **cat 还有向已存在的文件追加内容的功能；**

       ```shell 
       # cat  linuxsir.txt                           注：查看已存在的文件linuxsir.txt 内容；
       I am BeiNanNanBei From LinuxSir.Org .         注：内容行
       我正在为cat命令写文档                            注：内容行
       # cat >> linuxsir.txt << EOF                  注：我们向linuxsir.txt文件追加内容；
       > 我来测试cat向文档追加内容的功能；                注：这是追回的内容
       > OK？
       > OK～
       > 北南 呈上
       > EOF                                         注：以EOF退出；
       # cat linuxsir.txt                            注：查看文件内容，看是否追回成功。
       I am BeiNanNanBei From LinuxSir.Org .
       我正在为cat命令写文档
       我来测试cat向文档追加内容的功能； 
       OK？
       OK～
       北南 呈上
       ```

  　　6. **追加文件内容：**

       cat 连接多个文件的内容并且输出到一个新文件中

       

