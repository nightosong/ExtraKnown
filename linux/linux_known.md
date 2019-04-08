[TOC]
## 1 使用命令帮助
### 1.1 概述

命令|解释
---|---
man     | 查看命令参数及使用方法
man -k  | 关键字搜索
info    | 获取详细信息
whatis  | 查找命令简介
whereis | 查找匹配命令位置
which   | 查找命令位置

### 1.2 命令使用
#### 查看命令的简要说明
简述命令的作用

```
whatis command
```
正则匹配

```
whatis -w "comman*"
```
更加详细的说明文档

```
info command
```
#### 使用man命令

查询命令的说明文档 **(Manual)**
```
man command
```
使用page up和page down来上下翻页

在man的帮助手册中，将帮助文档分为了9个类别，对于有的关键字可能存在多个类别中，这时，我们就需要指定特定的类别来查看。

man页面所属的分类标识（常用的是分类1和分类3）

```
1. 用户可以操作的命令或者是可执行文件
2. 系统核心可调用的函数或者工具等
3. 一些常用的函数与数据库
4. 设备文件的说明
5. 设置文件或者某些文件的格式
6. 游戏
7. 惯例与协议。例如linux标准文件系统、网络协议、ASCII码等说明内容
8. 系统管理员可用的惯例条令
9. 与内核有关的文件
```
前面说到使用whatis会显示命令所在的具体的文档类别，我们学习如何使用它

```
eg:
whatis info
info (1)             - read Info documents
info (5)             - readable online documentation
info (n)             - Return information about the state of the Tcl interpreter
```
我们可以指定查看分类1的帮助

```bash
man 1 info
```
查询关键字

```bash
man -k keyword
```
#### 查看路径
查看程序的的binary文件所在路径

```b
which program_name
```
查看程序的搜索路径

```b
whereis program_name
```

#### 查看历史

历史记录
```
history
```
ps：按住“CTRL + R”就可以搜索已经执行过的命令，它可以在你写命令时自动补全。

## 2 文件及目录管理

### 2.1 创建和删除

* 创建：mkdir  **(Make Directory)**
* 删除：rm  **(Remove)**
* 删除非空目录：rm -rf dir
* 删除日志：rm *log（等价：find ./ -name "*log" -exec rm {};）
* 移动：mv  **(Move)**
* 复制：cp（复制目录：cp -r） **(Copy)**

查看当前目录下文件个数
```bash
find src_dir | wc -l
```

复制目录
```
cp -r src_dir dest_dir
```
### 2.2 目录切换

* 找到文件/目录位置： cd  **(Change Directory)**
* 切换到上一个工作目录： cd -
* 切换到home目录： cd or cd ~
* 显示当前路径： pwd  **(Print Working Directory)**
* 更改当前的工作路径为path： cd path

### 2.3 列出目录项

**(List)**

* 显示当前目录下的文件ls
* 按时间排序，以列表的方式显示目录项ls -lrt
* 每项文件前增加id编号： ls | cat -n

可以在.bashrc中设置命令别名：

```
alias lsl='ls -lrt'
alias lm='ls -al | more'
```
这样就可以使用lsl来代替ls -lrt
ps：.bashrc在/home/你的用户名/文件夹下，以隐藏文件的方式储存；可以使用ls -a查看；

### 2.4 查找目录及文件find/locate/lsof

搜寻文件或目录：

```
find src_dir -name filename | xargs file
```
查找目标文件夹中是否有目标文件

```
find src_dir -name filename
```
递归当前目录及子目录删除所有目标文件

```
find src_dir -name filename -exec rm {} \;
```
find是实时查找，如果需要更快的查询，可以用locate；locate会为文件系统建立索引数据库，如果有文件更新，需要定期执行更新命令来更新数据库

```bash
locate filename
updatedb
```

Linux一切皆文件 **(List Open File)**
```
lsof
```

### 2.5 查看文件内容

**cat** **(Catenate)** 由第一行开始显示
```
cat [-bnvAET] [--help] [--version] 文件名称
```
* **-b** : 列出行号，仅针对非空白行做行号显示，空白行不标行号
* **-n** : 列出行号，连空白行也会有行号
* **-v** : 列出一些看不出来的特殊字符
* **-A** : 可列出一些特殊字符而不是空白，相当于-vET的组合
* **-E** : 将结尾的断行字节以 $ 显示出来
* **-T** : 将[Tab]按键以 ^I 显示出来

**tac** 由最后一行开始显示(cat反过来)
```
tac [-brs] [--help] [--version] 文件名称
```

**nl** **(Number of Line)** 显示行号
```
nl [-bnw] 文件名称
```
* **-b** : 指定行号指定的方式，主要分两种
    * -ba : 全部显示行号，包括空行; -
    * -bt : 不显示空行行号(默认值)
* **-n** : 列出行号的表示方法，主要有三种
    * -nln : 行号在屏幕的最左显示
    * -nrn : 行号在自己栏位的最右显示，且不加0
    * -nrz : 行号在自己栏位的最右显示，且加0
* **-w** : 行号栏位的占用位数

**more** 按页显示
```
more 文件名称
```
命令 | 解释
---|---
空格键        | 代表向下翻一页；
回车键        | 代表向下翻『一行』；
/字串         | 代表在这个显示的内容当中，向下搜寻『字串』这个关键字；
:f            | 立刻显示出档名以及目前显示的行数；
q             | 代表立刻离开 more ，不再显示该文件内容。
b 或 [ctrl]-b | 代表往回翻页，不过这动作只对文件有用，对管线无用。

**less** 按页显示
```
less 文件名称
```
命令 | 解释
---|---
空白键    | 向下翻动一页；
[pagedown]| 向下翻动一页；
[pageup]  | 向上翻动一页；
/字串     | 向下搜寻『字串』的功能；
?字串     | 向上搜寻『字串』的功能；
n         | 重复前一个搜寻 (与 / 或 ? 有关！)
N         | 反向的重复前一个搜寻 (与 / 或 ? 有关！)
q         | 离开 less 这个程序；

**head** 取出文件前面几行
```
head [-n number] 文件名称
```
* **-n** : number表示显示几行

**tail** 取出文件后面几行
```
tail [-n number] 文件名称
```
* **-n** : number代表显示几行(默认10行)
* **-f** : 表示持续侦测后面所接的档名，直到按下[ctrl]+c结束侦测

**diff** 查看两个文件间的差别
```
diff 文件1 文件2
```

**touch** 命令
```
touch 文件名称
```
ps:“touch”命令代表了将文件的访问和修改时间更新为当前时间。touch命令只会在文件不存在的时候才会创建它。如果文件已经存在了，它会更新时间戳，但是并不会改变文件的内容。

### 2.6 查找文件内容
使用egrep查询文件内容
```
egrep '03.1\/CO\/AE' TSF_STAT_111130.log.012
egrep 'A_LMCA777:C' TSF_STAT_111130.log.035 > co.out2
```
### 2.7 文件与目录权限更改
* 改变文件的拥有者： chown
* 改变文件读写执行等属性： chmod
* 递归子目录修改： chown -R tuxapp source/
* 增加脚本可执行权限： chmod a+x myscript

### 2.8 给文件增加别名
创建符号链接/硬链接
```
ln src_file ln_file : 硬链接; 删除一个，仍能被找到
ln -s src_file ln_file : 软连接(符号链接); 删除源，另一个无法使用
```

### 2.9 管道和重定向
* 批处理命令连接执行，使用 |
* 串联：使用分号 ；
* 前面成功则执行后面，否则不执行，使用 &&
* 前面失败，则执行后面，使用 ||

样例
```
ls /proc && echo success || echo failed
if ls /proc; then echo success; else echo failed; fi
```
重定向
```
ls proc/*.c > list 2> &l 将标准输出和标准错误重定向到同一文件
ls proc/*.c &>list
```
清空文件
```
:> a.txt
```
重定向
```
echo aa >> a.txt
```

### 2.10 设置环境变量
查看环境变量
```
echo $PATH
```
可用export命令查看PATH值
```
export
```
启动账号后自动执行的是文件是.profile，可通过这个文件设置自己的环境变量
```
PATH=$APPDIR:/opt/app/soft/bin:$PATH:/usr/local/bin:$TUXDIR/bin:$ORACLE_HOME/bin;export PATH
```
环境变量更改后在下次账户重新登录时生效，如果需要立即生效，使用
```
source .bash_profile
```
其他方法
```
1. 修改/etc/profile文件
    vim /etc/profile
    添加
    export PATH="$PATH:App_Bin_Path"
2. 修改~/.bashrc文件
    cd ~
    vim .bashrc
    添加
    export PATH="$PATH:App_Bin_Path"
```

### 2.11 Bash快捷输入或删除
命令|解释
---|---
Ctrl+U | 删除光标到首行的所有字符，在某些设置下，删除全行
Ctrl+W | 删除当前光标到前边的最近一个空格之间的字符
Ctrl+H | backspace，删除光标前边的字符
Ctrl+R | 匹配最相近的一个文件，然后输出

## 3. 文本处理
shell脚本命令尽量单行书写，不要超过两行

### 3.1. find文件查找

常用查找方式
```
# 查找文件
find src_dir -name filename -print

# 查找所有非目标的文件
find src_dir ! -name filename -print

#指定搜索深度，打印出当前目录的文件
find src_dir -maxdepth find_depth -type f
```
按类型搜索
```
find . -type d -print
ps: -type f 文件 / l 符号链接 / d 目录
```
find 支持的文件检索类型可以区分普通文件和符号链接、目录等，但是二进制文件和文本文件无法直接通过find的类型区分出来；

file命令可以检查文件具体类型;
用命令组合查找本地目录中的所有二进制文件
```
ls -lrt | awk '{print $9}' | xargs file | grep ELF | awk '{print $1}' | tr -d ':'
```
按时间搜索
* -atime 访问时间（单位是天，分钟单位则是-amin）
* -mtime 修改时间（内容被修改）
* -ctime 变化时间（元数据或权限变化）

```
# 最近第7天被访问的文件
find . -atime 7 -type f -print

# 最近7天被访问的文件
find . -atime -7 -type f -print

# 7天前被访问的文件
find . -atime +7 -type f -print

```

按大小搜索
```
# 单位 k M G
# 寻找大于2k的文件
find . -type f -size +2k
```

按权限查找
```
# 找具有可执行权限的所有文件
find .type f -perm 644 -print
```

按用户查找
```
# 找用户weber所拥有的文件
find . -type f -user weber -print
```

查找之后的操作
```
# 删除
find . -type f -name "*.swp" -delete
find . -type f -name "*.swp" | xargs rm

# 执行动作
find . -type f -user root -exec chown weber {} \;
ps: {} 是一个特殊字符串，对于每一个匹配的文件，{}会被替换成相应地文件名
find . -type f -mtime +10 -name "*.txt" -exec cp {} OLD \;

# 结合多个命令
-exec ./commands.sh {} \;
```

-print的定界符
```
默认使用 \n 作为文件的定界符
-print 使用 \0 作为文件的定界符，这样就可以搜索包含空格的文件
```

### 3.2. grep 文本搜索

常用参数 **(General Regular Expression Print)**
* -o 只输出匹配的文本行
* -v 只输出没有匹配的文本行
* -c 统计文件中包含文本的次数
* -n 打印匹配的行号
* -i 搜索时忽略大小写
* -l 只打印文件名

在多级目录中对文本递归搜索
```
grep "class" . -R -n
```

匹配多个模式
```
grep -e "class" -e "vitural" filename
```
grep输出以0作为结尾符的文件名（-z）
```
grep "test" file* -lZ | xargs -0 rm
```
将日志中所有带where条件的sql查找查找出来
```
cat LOG.* | tr a-z A-Z | grep "FROM" | grep "WHERE" > b
```

### 3.3. xargs 命令行参数转换

xargs能够将输入数据转化为特定命令的命令行参数，可以配合很多命令来组合使用

```
#n 是多行文本间的定界符
# 将单行转化为多行输出
cat textname | xargs -n 3
ps: -n 指定每行显示的字段数
```
xargs参数说明
* -d 定义定界符（默认为空格，多行的定界符为n）
* -n 指定输出为多行
* -l {} 指定替换字符串
* -0 指定0为输入定界符

```bash
cat file.txt | xargs -I {} ./command.sh -p {} -1

#统计程序行数
find source_dir/ -type f -name "*.cpp" -print0 |xargs -0 wc -l

#redis通过string存储数据，通过set存储索引，需要通过索引来查询出所有的值：
./redis-cli smembers $1  | awk '{print $1}'|xargs -I {} ./redis-cli get {}
```

### 3.4. sort排序

字段说明
* -n 按数字进行排序
* -d 按字典序进行排序
* -r 逆序排序
* -k N 指定按第N列排序
* -d 忽略空格之类的前导空白字符
```
sort -nrk 1 data.txt
```

### 3.5. uniq 消除重复行
```bash
# 消除重复行
sort unsort.txt | uniq

# 统计各行在文件中出现的次数
sort unsort.txt | uniq -c

# 找出重复行
sort unsort.txt | uniq -d

ps: 可指定需要比较的重复内容，-s开始位置 -w比较字符数
```

### 3.6. 用tr进行转换

通用用法
```bash
# 替换对应字符
echo 12345 | tr 0-9 9876543210
echo 12345 | tr '0-9' '9876543210'

# 制表符转空格
cat text| tr '\t' ' '

# -d 删除字符
cat file | tr -d '0-9'  //删除文件中的所有数字

# -c 求补集
cat file | tr -c '0-9'          //获取文件中所有数字
cat file | tr -d -c '0-9 \n'    //删除非数字数据

# tr压缩字符
# tr -s 压缩文本中出现的重复字符；最常用于压缩多余的空格:
cat file | tr -s ' '
```

tr中可用各种字符类：
* alnum：字母和数字
* alpha：字母
* digit：数字
* space：空白字符
* lower：小写
* upper：大写
* cntrl：控制（非可打印）字符
* print：可打印字符

使用方法：tr [:class:] [:class:]

```
tr '[:lower:]' '[:upper:]'
```

### 3.7. cut 按列切分文本

主要参数
* -b ：以字节为单位进行分割
* -c ：以字符为单位进行分割
* -f ：以域为单位进行分割（与-d同时使用）
* -d ：自定义分隔符，默认为制表符
* -n ：取消分割多字节字符（仅和-b一起使用）
```bash
cat textfile
ab cd ef
AB CD EF

cut -d ' ' -f 1,2 textfile
ab cd
AB cd

cut -d ' ' -f 1-3 textfile
ab cd ef
AB CD EF
```

### 3.8. paste 按列拼接文本

将两个文本按列拼接到一起;
```bash
cat file1
hello world
hello world

cat file2
paste example
paste example

paste file1 file2
hello world paste example
hello world paste example
```

默认的定界符是制表符，可以用-d指明定界符:
```
paste file1 file2 -d ','
hello world,paste example
hello world,paste example
```

### 3.9. wc 统计行和字符的工具

**(Word Count)**

```
$wc -l file // 统计行数
$wc -w file // 统计单词数
$wc -c file // 统计字符数
```

### 3.10. sed 文本替换利器

**(Stream Editor)**

替换每行的首处匹配
```
sed s/src_text/dest_text/ filename
```

全局替换
```
sed s/src_text/dest_text/g filename
```

默认替换后，输出替换后的内容，如果需要直接替换原文件,使用-i:
```
sed -i s/src_text/dest_text/g filename
```

移除空白行
```
sed /^$/d filename
```

变量转换
已匹配的字符串通过标记&来引用.
```
echo this is an example | sed 's/\w+/[&]/g'
$>[this]  [is] [an] [example]
```

子串匹配标记
第一个匹配的括号内容使用标记 1 来引用
```
sed 's/hello\([0-9]\)/\1/'
```

双引号求值
sed通常用单引号来引用；也可使用双引号，使用双引号后，双引号会对表达式求值:
```
sed 's/$var/HLLOE/'
```

当使用双引号时，我们可以在sed样式和替换字符串中指定变量；
```
eg:
p=patten
r=replaced
echo "line con a patten" | sed "s/$p/$r/g"
$>line con a replaced
```

其它示例
字符串插入字符：将文本中每行内容（ABCDEF） 转换为 ABC/DEF:
```
sed 's/^.\{3\}/&\//g' file
```


### 3.11. awk 数据流处理工具

* awk脚本结构及工作方式

```bash
awk ' BEGIN{ statements } statements2 END{ statements } '
```

工作方式
1. 执行begin中语句块；
2. 从文件或stdin中读入一行，然后执行statements2，重复这个过程，直到文件全部被读取完毕；
3. 执行end语句块；

* print 打印当前行

使用不带参数的print时，会打印当前行
```
echo -e "line1\nline2" | awk 'BEGIN{print "start"} {print } END{ print "End" }'
```

print 以逗号分割时，参数以空格定界;
```
echo | awk ' {var1 = "v1" ; var2 = "V2"; var3="v3"; \
print var1, var2 , var3; }'
$>v1 V2 v3
```

使用-拼接符的方式（”“作为拼接符）;
```
echo | awk ' {var1 = "v1" ; var2 = "V2"; var3="v3"; \
print var1"-"var2"-"var3; }'
$>v1-V2-v3
```

特殊变量： NR NF $0 $1 $2
* NR: 表示记录数量，在执行过程中对应当前行号；
* NF: 表示字段数量，在执行过程总对应当前行的字段数；
* $0: 这个变量包含执行过程中当前行的文本内容；
* $1: 第一个字段的文本内容；
* $2: 第二个字段的文本内容；

```
echo -e "line1 f2 f3\n line2 \n line 3" | awk '{print NR":"$0"-"$1"-"$2}'
```

打印每一行的第二和第三个字段
```
awk '{print $2, $3}' file
```

统计文件的行数
```
awk ' END {print NR}' file
```

累加每一行的第一个字段
```
echo -e "1\n 2\n 3\n 4\n" | awk 'BEGIN{num = 0 ;
print "begin";} {sum += $1;} END {print "=="; print sum }'
```

传递外部变量
```
var=1000
echo | awk '{print vara}' vara=$var #  输入来自stdin
awk '{print vara}' vara=$var file # 输入来自文件
```

用样式对awk处理的行进行过滤
```
awk 'NR < 5' #行号小于5
awk 'NR==1,NR==4 {print}' file #行号等于1和4的打印出来
awk '/linux/' #包含linux文本的行（可以用正则表达式来指定，超级强大）
awk '!/linux/' #不包含linux文本的行
```

设置定界符
使用-F来设置定界符（默认为空格）:
```
awk -F: '{print $NF}' /etc/passwd
```

读取命令输出
使用getline，将外部shell命令的输出读入到变量cmdout中:
```
echo | awk '{"grep root /etc/passwd" | getline cmdout; print cmdout }'
```

在awk中使用循环

```
for(i=0;i<10;i++){print $i;}
for(i in array){print array[i];}
eg:以下字符串，打印出其中的时间串:

2015_04_02 20:20:08: mysqli connect failed, please check connect info
$echo '2015_04_02 20:20:08: mysqli connect failed, please check connect info'|awk -F ":" '{ for(i=1;i<=;i++) printf("%s:",$i)}'
>2015_04_02 20:20:08:  # 这种方式会将最后一个冒号打印出来
$echo '2015_04_02 20:20:08: mysqli connect failed, please check connect info'|awk -F':' '{print $1 ":" $2 ":" $3; }'
>2015_04_02 20:20:08   # 这种方式满足需求
```

而如果需要将后面的部分也打印出来(时间部分和后文分开打印):


```
$echo '2015_04_02 20:20:08: mysqli connect failed, please check connect info'|awk -F':' '{print $1 ":" $2 ":" $3; print $4;}'
>2015_04_02 20:20:08
>mysqli connect failed, please check connect info
```

以逆序的形式打印行：(tac命令的实现）:
```
seq 9| \
awk '{lifo[NR] = $0; lno=NR} \
END{ for(;lno>-1;lno--){print lifo[lno];} } '
```

awk结合grep找到指定的服务，然后将其kill掉
```
ps -fe| grep msv8 | grep -v MFORWARD | awk '{print $2}' | xargs kill -9;
```

awk实现head、tail命令
```
head
awk 'NR<=10{print}' filename
tail
awk '{buffer[NR%10] = $0;} END{for(i=0;i<11;i++){ \
print buffer[i %10]} }  filename
```

打印指定列
awk方式实现
```
ls -lrt | awk '{print $6}'
```

cut方式实现
```
ls -lrt | cut -f6
```

打印指定文本区域
确定行号
```
seq 100| awk 'NR==4,NR==6{print}'
```

确定文本
打印处于start_pattern 和end_pattern之间的文本:
```
awk '/start_pattern/, /end_pattern/' filename
```

示例:
```
seq 100 | awk '/13/,/15/'
cat /etc/passwd| awk '/mai.*mail/,/news.*news/'
```

awk常用内建函数:
* index(string,search_string):返回search_string在string中出现的位置
* sub(regex,replacement_str,string):将正则匹配到的第一处内容替换为replacement_str;
* match(regex,string):检查正则表达式是否能够匹配字符串；
* length(string)：返回字符串长度
```
echo | awk '{"grep root /etc/passwd" | getline cmdout; print length(cmdout) }'
```

printf 类似c语言中的printf，对输出进行格式化:
```
seq 10 | awk '{printf "->%4s\n", $1}'
```


### 3.12. 迭代文件中的行、单词和字符

1. 迭代文件中的每一行

```bash
cat textfile | (while read line; do echo $line; done)

cat textfile | awk '{print}'
```

2. 迭代一行中的每一个单词

```bash
cat textfile | (while read line; do(for word in $line; do echo $word; done); done)
```

3. 迭代每一个字符

```bash
${string:start_pos:num_of_chars}：从字符串中提取一个字符；(bash文本切片）
${#word}: 返回变量word的长度


for((i=0;i<${#word};i++))
do
echo ${word:i:1);
done

ps: 以ASCII字符显示文件:
$od -c filename
```

## 4. 磁盘管理

### 4.1. 查看磁盘空间

查看磁盘空间利用情况 **(Disk Free)**  

```bash 
df -h
# ps: -h是human的缩写，指以易读的方式显示结果
```
* **-a** ：列出所有的文件系统，包括系统特有的 /proc 等文件系统；
* **-k** ：以 KBytes 的容量显示各文件系统；
* **-m** ：以 MBytes 的容量显示各文件系统；
* **-h** ：以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；
* **-H** ：以 M=1000K 取代 M=1024K 的进位方式；
* **-T** ：显示文件系统类型, 连同该 partition 的 filesystem 名称 (例如 ext3) 也列出；
* **-i** ：不用硬盘容量，而以 inode 的数量来显示

查看当前目录空间利用情况 **(Disk Usage)**
```bash
du -sh
# ps: -s指递归整个目录的大小
```

查看当前目录所有子文件夹排序后的大小
```bash
for i in `ls`; do du -sh $i; done | sort
# 或者
du -sh `ls` | sort
```
* **-a** ：列出所有的文件与目录容量，因为默认仅统计目录底下的文件量而已。
* **-h** ：以人们较易读的容量格式 (G/M) 显示；
* **-s** ：列出总量而已，而不列出每个各别的目录占用容量；
* **-S** ：不包括子目录下的总计，与 -s 有点差别。
* **-k** ：以 KBytes 列出容量显示；
* **-m** ：以 MBytes 列出容量显示；

磁盘分区 **(Fixed Disk)**
```
fdisk [-l] [装置名称]
```
* **-l** ：输出后面接的装置所有的分区内容。若仅有 fdisk -l 时， 则系统将会把整个系统内能够搜寻到的装置的分区均列出来。

列出块设备 **(List Block)**
```
lsblk
```

磁盘格式化
```
mkfs [-t 文件系统格式] 装置文件名

# 将disk1格式化为ext3文件系统
mkfs -t ext3 disk1
```
* **-t** : 可以接收的文件系统格式,例如ext3、ext2、vfat(系统有支持才会生效)

磁盘检验 **(File System Check)**
```
fsck [-t 文件系统] [-ACay] 装置名称
```
* **-t** : 给定档案系统的型式，若在 /etc/fstab 中已有定义或 kernel 本身已支援的则不需加上此参数
* **-s** : 依序一个一个地执行 fsck 的指令来检查
* **-A** : 对/etc/fstab 中所有列出来的 分区（partition）做检查
* **-C** : 显示完整的检查进度
* **-d** : 打印出 e2fsck 的 debug 结果
* **-p** : 同时有 -A 条件时，同时有多个 fsck 的检查一起执行
* **-R** : 同时有 -A 条件时，省略 / 不检查
* **-V** : 详细显示模式
* **-a** : 如果检查有错则自动修复
* **-r** : 如果检查有错则由使用者回答是否修复
* **-y** : 选项指定检测每个文件是自动输入yes，在不确定那些是不正常的时候，可以执行 # fsck -y 全部检查修复。

磁盘挂载
```
mount [-t 文件系统] [-L Label名] [-o 额外选项] [-n] 装置文件名 挂载点
```

磁盘卸除
```
umount [-fn] 装置文件名或挂载点
```
* **-f** : 强制卸除
* **-n** : 不升级/etc/mtab情况下卸除

### 4.2. 打包/解包

(Tape Archive)

tar 命令
* -c: 打包选项
* -x: 解包选项
* -v: 显示打包进度
* -f: 指定包的文件名
* -r: 指增加文件
* -u: 指更新文件
* -t: 指列出文件列表
* -z: 解压gz文件
* -j: 解压bz2文件
* -J: 解压xz文件
```bash
tar -cvf filename.tar filename
tar -xvf filename.tar
```

### 4.3. 压缩/解压缩

zip和unzip命令
```bash
zip filename.zip filename
# 生成 filename.zip
unzip filename.zip
```

gzip和gunzip命令
```bash
gzip filename
# 生成 filename.gz
gunzip filename.gz
```

## 5. 进程管理工具
### 5.1 查询进程

查询正在运行的进程信息 **(Process Status)**
```bash
ps -ef
```

查询进程ID
```bash
pgrep
# eg： 查询进程名中含有xx的进程
pgrep -l xx
```

以完整的格式显示所有的进程
```bash
ps -ajx
```

显示进程信息，并实时更新
```
top
```

查看端口port_id占用的进程状态
```
lsof -i: port_id
```

查看用户username的进程打开的文件
```
lsof -u username
```

查看进程process_xx进程当前打开的文件
```
lsof -c process_xx
```

查看指定进程ID为process_id所打开的文件
```
lsof -p process_id
```

查询指定目录下被进程开启的文件
```
lsof +d dest_dir
# +d 递归目录
```

### 5.2. 终止进程

杀死指定PID的进程（Process ID）
```
kill PID
kill -9 PID
```

杀死job工作
```
kill %job_number
```

### 5.3. 进程监控
查看系统中使用CPU、使用内存最多的进程
* P: 根据CPU占用百分比进行排序
* T: 根据CPU占用时间进行排序
* M: 根据占用内存大小进行排序
* i: 只显示正在运行的进程
* q: 退出top
```
top
[command]
```

### 5.4. 分析进程栈

pmap 命令，输出进程内存的状况，可用来分析线程堆栈
```
pmap PID
```

## 6.性能监控

### 6.1. 监控CPU

sar(System Activity Reporter)
```
sar [options] [-A] [-o file] [t] [n]
```
* -A : 所有报告的总和
* -u : 输出CPU使用情况的统计信息
* -v : 输出inode、文件和其他内核表的统计信息
* -d : 输出每一个块设备的活动信息
* -q : 查看运行队列中的进程数、系统上进程大小、平均负载等
* -r : 输出内存和交换空间的统计信息
* -b : 显示I/O和传送速率的统计信息
* -a : 文件读写情况
* -c : 输出进程统计信息，美妙创建的进程数
* -R : 输出内存页面的统计信息
* -y : 终端设备活动情况
* -w : 输出系统交换活动信息

```
eg:
# 每10秒采样一次，连续采样3次
sar -u -o test 10 3 
```

### 6.2. 其他

当系统sar不可用时，可用以下工具代替：

linux系统下有vmstat，unix系统下有prstat

eg：查看cpu、内存使用情况： vmstat n m（n为监控频率，m为监控次数）

使用watch工具监控变化：watch -n 1

## 7. 网络工具

### 7.1. 查询网络服务和端口

netstat命令用于显示各种网络相关信息，如网络连接、路由表、接口状态、masquerade连接、多播成员等

列出所有端口
```
netstat -a
```

列出所有tcp端口
```
netstat -at
```

列出所有有监听的服务状态
```
netstat -l
```

nc命令 **(Net Cat)**
```
端口扫描 抓包 传送文件
```

### 7.2. 网络路由

查看路由状态
```
route -n
```

发送ping包到地址IP
```
ping IP
```

探测前往地址IP的路由途径
```
traceroute IP
```

DNS查询，寻找域名domain对应的IP
```
host domain
```

反向DNS查询
```
host IP
```

### 7.3. 镜像下载

直接下载文件或者网页
```
wget url
```

常用选项
* -limit-rate：下载限速
* -o：指定日志文件；输出都写入日志
* -c： 断点续传

### 7.4. ftp sftp lftp ssh

SSH登录
```
ssh USER@host
```

ftp/stfp 文件传输
```
stfp USER@host
```
常用选项
* get filename： 下载文件
* put filename： 上传文件
* ls ： 列出host当前路径的所有文件
* cd ： 在host上更改当前路径
* lls： 列出本地主机上当前路径的所有文件
* lcd： 在本地主机上更改当前路径

lftp同步文件夹
```
lftp -u user:pass host
lftp user@host:'~>' mirror -n

```

### 7.5. 网络复制

将本地localpath指向的文件上传到远程主机的dest_path路径
```bash
scp localpath USER@host:dest_path
```

下载远程主机dest_path路径下的整个文件系统到本地localpath
```
scp -r USER@host:dest_path localpath
```

## 8. 用户管理工具

### 8.1. 用户

添加用户
```
useradd -m username
```
用户目录为/home/username

设置密码
```
passwd username
```

删除用户
```
userdel -r username
```

账号切换 **(Substitute User)** **(Switch User)**
```
su username
```

查询用户
```
who
whoami
```

### 8.2. 用户的组

查看当前用户所属的组
```
groups
```

将用户加入到组
```
usermod -G groupname username
```

变更用户所属的根组
```
usermod -g groupname username
```

查看系统所有组
```
more /etc/passwd
more /etc/group
```

### 8.3. 用户权限

查看文件所属字段
```
ls -l
```

更改读写权限
```
chmod usermark(+/-)permissionmark pathname
```
usermark取值：
* u： 用户
* g： 组
* o： 其他用户
* a： 所有用户

permissionmark取值：
* r： 读
* w： 写
* x： 执行

```
chmod abc pathname
```
a b c 均属于 1-7，每位通过4(读)2(写)1(执行)来表示

更改文件或目录的拥有者
```
chown username filename
chown -R username pathname
```

更改文件或目录的所属群组 **(Change Group)**
```
chgrp [-cfhRv] [--help] [--version] [所属群组] [文件或目录...]
```
参数说明：
* **-c**或--changes 效果类似"-v"参数，但仅回报更改的部分。
* **-f**或--quiet或--silent 　不显示错误信息。
* **-h**或--no-dereference 只对符号连接的文件作修改，而不更动其他任何相关文件。
* **-R**或--recursive 　递归处理，将指定目录下的所有文件及子目录一并处理。
* **-v**或--verbose 　显示指令执行过程。
* **--help** 　在线帮助。
* **--reference**=<参考文件或目录> 把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同。
* **--version** 　显示版本信息。

### 8.4. 环境变量

bashrc与profile都用于保存用户的环境信息，bashrc用于交互式non-loginshell，而profile用于交互式login shell。

/etc/profile，/etc/bashrc 是系统全局环境变量设定 

~/.profile， ~/.bashrc用户目录下的私有环境变量设定

当登入系统获得一个shell进程时，其读取环境设置脚本分为三步:

1. 首先读入的是全局环境变量设置文件/etc/profile，然后根据其内容读取额外的文档，如/etc/profile.d和/etc/inputrc
2. 读取当前登录用户Home目录下的文件~/.bash_profile，其次读取~/.bash_login，最后读取~/.profile，这三个文档设定基本上是一样的，读取有优先关系
3. 读取~/.bashrc

 ~/.profile 与 ~/.bashrc的区别:

* 这两者都具有个性化定制功能
* ~/.profile可以设定本用户专有的路径，环境变量，等，它只能登入的时候执行一次
* ~/.bashrc也是某用户专有设定文档，可以设定路径，命令别名，每次shell script的执行都会使用它一次

## 9. 系统管理及IPC资源管理

### 9.1. 系统管理

查询系统版本

**(Unix Name)**
```
# linux
uname -a
lsb_release -a

# unix
more /etc/release

```

查询硬件信息
```
# cpu
sar -u 5 10
cat /proc/cpuinfo
cat /proc/cpuinfo | grep processor | wc -l

# 内存
cat /proc/meminfo

# 内存page大小
pagesize

# 架构
arch
```

设置系统时间 **(Date)**

```
# 显示系统时间
date

# 设置系统日期和时间
date -s 2019-04-03 16:28:00

# 设置时区
tzselect

# 强制把系统时间写入CMOS
clock -w

# 格式化输出当前时间
date +%Y%m%d.%H%M%S

```

显示日历 **(Calender)**
```
# 显示当前月份
cal

# 显示过去/未来月份
cal 04 2019
```

### 9.2. IPC资源管理

查看系统使用的IPC资源
```
# 查看IPC
ipcs

# 查看系统使用的IPC共享内存资源
ipcs -m

# 查看系统使用的IPC队列资源
ipcs -q

# 查看系统使用的IPC信号量资源
ipcs -s

```

检测和设置系统资源限制
```
# 显示当前所有系统资源limit信息
ulimit -a

# 对生成的core文件的大小不进行限制
ulimit -c unlimited

```

## 9.3. 系统目录结构

目录 | 解释
--- | ---
/bin    | 存放经常使用的目录 **(Binary)**
/boot   | 存放启动Linux时的一些核心文件，包括一些连接文件及镜像文件
/dev    | 存放Linux的外部设备 **(Device)**
/etc    | 存放所有的系统管理所需的配置文件和子目录 **(Etcetera)**
/home   | 用户的主目录，每个用户的目录名以用户的账号命名
/lib    | 存放系统最基本的动态链接共享库 **(Library)**
/lost+found | 一般情况下为空，当系统非法关机时，这里会存放一些文件
/media  | Linux会自动识别一些设备并挂载到该目录下，比如U盘、光驱
/mnt    | 提供给用户临时挂载文件系统用 **(Mount)**
/opt    | 给主机额外安装软件所摆放的目录
/proc   | 虚拟目录，是系统内存的映射，可直接访问该目录获取系统信息 **(Process)**
/root   | 系统管理员用户主目录
/sbin   | 存放系统管理员使用的系统管理程序 **(Super Binary)**
/srv    | 该目录存放一些服务启动之后需要提取的数据 **(service)**
/sys    | **(System)**
/tmp    | 存放临时文件 **(temporary)**
/usr    | 应用程序与文件的存放目录 **(Unix System Resource)**
/usr/bin    | 系统用户使用的应用程序
/usr/sbin   | 超级用户使用的管理程序和系统守护程序
/usr/src    | 内核源代码默认的存放目录
/var    | 存放经常被修改的目录或文件,包括各种日志文件 **(Variable)**
/run    | 临时文件系统，存储系统启动以来的信息，重启后会被清除
