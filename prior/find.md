find命令
=======

find [起始目录] 寻找条件 操作
find PATH OPTION [-exec COMMAND { } \;]

PATH表示开始查找的目录，然后依次递归查找。
OPTION表示查找的条件，条件可以用逻辑操作组合。
找到之后执行指定的命令

### 逻辑组合:
* and：逻辑与，在命令中用“-a”表示，是系统缺省的选项，表示只有当所给的条 件都满足时，寻找条件才算满足。例如：

	find –name ’tmp’ –xtype c -user ’inin’ # 该命令寻找三个给定条件都满足的所有文件

* or：逻辑或，在命令中用“-o”表示。该运算符表示只要所给的条件中有一个满足 时，寻找条件就算满足。例如：

	find –name ’tmp’ –o –name ’mina*’ # 该命令查询文件名为’tmp’或是匹配’mina*’的所有文件。

* not：逻辑非，在命令中用“!”表示。该运算符表示查找不满足所给条件的文件 。例如：

	find ! –name ’tmp’ # 该命令查询文件名不是’tmp’的所有文件。

* 需要说明的是：当使用很多的逻辑选项时，可以用括号把这些选项括起来。为了避免Shell本身对括号引起误解，在话号前需要加转义字符“\”来去除括号的意义。例：

	find \(–name ’tmp’ –xtype c -user ’inin’ \)

### OPTION

* -name ’字串’ 查找文件名匹配所给字串的所有文件，字串内可用通配符 *、?、[ ]。

-lname ’字串’ 查找文件名匹配所给字串的所有符号链接文件，字串内可用通配符 *、?、[ ]。

-gid n 查找属于ID号为 n 的用户组的所有文件。

-uid n 查找属于ID号为 n 的用户的所有文件。

-group ’字串’ 查找属于用户组名为所给字串的所有的文件。

-user ’字串’ 查找属于用户名为所给字串的所有的文件。

-empty 查找大小为 0的目录或文件。

-path ’字串’ 查找路径名匹配所给字串的所有文件，字串内可用通配符*、?、[ ]。

-perm 权限 查找具有指定权限的文件和目录，权限的表示可以如711，644。

-size n[bckw] 查找指定文件大小的文件，n 后面的字符表示单位，缺省为 b，代表512字节的块。

-type x 查找类型为 x 的文件，x 为下列字符之一：

b 块设备文件

c 字符设备文件

d 目录文件

p 命名管道(FIFO)

f 普通文件

l 符号链接文件(symbolic links)

s socket文件

-xtype x 与 -type 基本相同，但只查找符号链接文件。

以时间为条件查找

-amin n 查找n分钟以前被访问过的所有文件。

-atime n 查找n天以前被访问过的所有文件。

-cmin n 查找n分钟以前文件状态被修改过的所有文件。

-ctime n 查找n天以前文件状态被修改过的所有文件。

-mmin n 查找n分钟以前文件内容被修改过的所有文件。

-mtime n 查找n天以前文件内容被修改过的所有文件。

-print：将搜索结果输出到标准输出。

例子：在root以及子目录查找不包括目录/root/bin的，greek用户的，文件类型为普通文件的，3天之前的名为test-find.c的文件，并将结构输出，find命令如下：

find / -name "test-find.c" -type f -mtime +3 -user greek -prune /root/bin -print

当然在这其中，-print是一个默认选项，我们不必刻意去配置它。

我们再看一下exec选项：

-exec：对搜索的结构指令指定的shell命令。注意格式要正确："-exec 命令 {} \;"

在}和\之间一定要有空格才行;

{}表示命令的参数即为所找到的文件;命令的末尾必须以“ \;”结束。

例子：对上述例子搜索出来的文件进行删除操作，命令如下：

find / -name "test-find.c" -type f -mtime +3 -user greek -prune /root/bin -exec rm {} \;

find命令指令实例：

find . - name ‘main*’ - exec more {} \;

% 查找当前目录中所有以main开头的文件，并显示这些文件的内容。

find . \(- name a.out - o - name ‘*.o’\)> - atime +7 - exec rm {} \;

% 删除当前目录下所有一周之内没有被访问过的a .out或*.o文件。

% 命令中的“.”表示当前目录，此时 find 将从当前目录开始，逐个在其子目录中查找满足后面指定条件的文件。

% “\(” 和 “\)” 表示括号()，其中的 “\” 称为转义符。之所以这样写是由于对 Shell 而言，(和)另有不同的含义，而不是这里的用于组合条件的用途。

% “-name a.out” 是指要查找名为a.out的文件;

% “-name ‘*.o’” 是指要查找所有名字以 .o 结尾的文件。

这两个 -name 之间的 -o 表示逻辑或(or)，即查找名字为a.out或名字以 .o结尾的文件。

% find命令在当前目录及其子目录下找到这佯的文件之后，再进行判断，看其最后访问时间 是否在7天以前(条件 -atime +7)，若是，则对该文件执行命令 rm(-exec rm {} \;)。

其中 {} 代表当前查到的符合条件的文件名，\;则是语法所要求的。

% 上述命令中第一行的最后一个 \ 是续行符。当命令太长而在一行写不下时，可输入一个 \，之后系统将显示一个 >，指示用户继续输入命令。

地址: http://www.chinaz.com/server/2009/0807/85796.shtml