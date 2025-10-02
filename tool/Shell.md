# Shell

## 1 Shell 基础


Shell 是文本形式的计算机交互接口，允许通过命令执行程序、管理文件及系统资源

常见的 Shell 有 `Bash`（默认）、`Zsh` 、`Fish` 等  

终端（Terminal）是运行 Shell 的应用程序

当打开终端时，会看到一个提示符

```sh
missing:~$ 
```

  - `missing`：主机名  
  - `~`：当前工作目录（`~` 表示用户主目录）  
  - `$`：普通用户身份（`#` 表示 root 用户）





## 2 Shell 指令

### 2.1 基础指令

#### 2.1.1 打印内容

1. `echo` 输出字符串

```sh
echo Hello
# Hello
```

shell 基于空格分割命令并进行解析，然后执行第一个单词代表的程序，并将后续的单词作为程序可以访问的参数

如果希望传递的参数中包含空格，可以使用单引号或双引号将其包裹起来，要么使用转义符号 `\` 进行处理

```sh
echo "Hello world"
echo 'Hello world'
echo Hello\ world
# Hello world

echo Hello world
# echo Hello world
# echo 会依照空格将内容拆分并打印出每个单词, 因此输出结果依然是 Hello world
```



2. `date` 打印时间

```sh
date
# Fri 10 Jan 2020 11:49:31 AM EST 打印当前时间
```



3. `pwd` 打印地址

```sh
pwd
# /home 打印当前目录地址
```







#### 2.1.2 目录操作

shell 中的路径是一组被分割的目录，在 Linux 和 macOS 上使用 `/` 分割，而在 Windows 上是 `\`

路径 `/` 代表的是系统的根目录，所有的文件夹都包括在这个路径之下，在 Windows 上每个盘都有一个根目录（例如： `C:\`）

如果某个路径以 `/` 开头，那么它是一个**绝对路径**，其他的都是**相对路径** 

相对路径是指相对于当前工作目录的路径



1. `cd` 切换目录

```sh
cd ..
# 返回上级目录, 在路径中, . 表示的是当前目录，而 .. 表示上级目录

cd
# 返回当前用户目录
# /home/user

cd test
# 进入指定目录
# /home/user/test
```



2. `ls` 查看目录

```sh
ls
# 显示当前目录下的子目录和文件

ls ./test
# 显示 test 目录下的所有子目录和文件

ls --help
# 使用 -h 或 --help 标记可以打印帮助信息, 可查看用标记或选项
```

大多数的命令接受标记和选项（带有值的标记），它们以 `-` 开头，并可以改变程序的行为



3. `mkdir` 创建目录

```sh
mkdir demo
# 在当前目录下创建 demo 目录

mkdir -p demo/dir1/dir2
# 递归创建多级目录, 如果上级目录不存在, 则会自动创建
# 当目录已经存在时避免报错

mkdir dir1 dir2 dir3
# 创建多个目录

mkdir demo && cd demo
# 创建目录后进入
```







#### 2.1.3 文件操作

1. `touch` 创建文件

```sh
touch main.cpp
# 创建文件

touch file1.txt file2.txt
# 创建多个文件
```



2. `rm` 删除文件

```sh
rm file.txt
# 删除文件 file.txt

rm file1.txt file2.txt file3.txt
# 删除多个文件

rm -f file.txt
# 强制删除文件

rm -r dirname
# 删除目录及其内容
```





3. `cp` 复制文件/目录

```sh
cp old.txt new.txt
# 将 old.txt 复制为 new.txt, 如果 new.txt 存在则会被覆盖

cp -p old.txt new.txt
# 复制时保留文件的 权限、所有者、时间戳 等信息

cp old.txt /path/dir/
# 复制到指定目录下, 文件名保持不变

cp file1 file2 /path/dir/
# 复制多个文件

cp -r dir1 dir2
# 复制整个目录 dir1 及其内容到 dir2, dir2 若不存在则会自动创建
```



4. `mv` 移动文件

```sh
mv old.txt new.txt
# 将 old.txt 重命名为 new.txt

mv file.txt /path/dir/
# 将文件移动到指定目录下

mv dir /path/dir/
# 将 dir 及其内容移动到 dir/ 目录下

mv -f file1 file2
# 用 file1 强制覆盖 file2, 不会提示确认
```







#### 2.1.4 查看文件 cat

`cat` 主要用于查看文件内容、合并多个文件，甚至创建文件

1. 查看文件内容

```sh
cat file.txt
# 显示 file.txt 的全部内容
```



2.  显示行号

```sh
cat -n file.txt
# 显示文件内容时，在每行前添加行号
```



3. 合并多个文件

```sh
cat file1 file2 > merged.txt
# 将 file1 和 file2 的内容合并, 并保存到 merged.txt
```



4. 追加内容

```sh
cat file1 >> file2
# 将 file1 的内容追加到 file2 末尾
```



5. 创建文件并输入内容

```sh
cat > newfile.txt
# 然后输入内容, 按 Ctrl + D 结束
```







#### 2.1.5 字符操作 tr

`tr` 命令用于替换、删除或压缩文本流中的字符，通常与 `echo`、`cat`、`grep`、`awk`、`sed` 等命令组合使用

基础语法：`tr [选项] 'SET1' 'SET2'`

`'SET1'` 和 `'SET2'` 可以为单一字符，也可以为字符串或正则表达式

当 `SET` 为字符串时，会进行逐字符映射

例：

```sh
echo "hello" | tr 'hello' 'hi'
# hiiii
# h -> h, e -> i   l l o 没有对应字符, 取 i
```



1. 字符替换

```sh
cat file.txt | tr ' ' '\n'
# 将文本中的字符转换位换行符

echo "HELLO WORLD" | tr 'A-Z' 'a-z'
echo "hello world" | tr 'a-z' 'A-Z'
# 大小写转换
```



2. 删除字符

```sh
echo "My number is czy2333" | tr -d '0-9'
# My number is czy
# 删除所有数字
```



3. 字符去重

```sh
echo "Hello      World" | tr -s ' '
# Hello World
# 去除多余空格
```







#### 2.1.6 搜索文本 grep

`grep` 主要用于 **搜索文本**，根据正则表达式匹配行并输出



1. 文本匹配

```sh
grep "hello" file.txt
# 从 file.txt 中查找包含 "hello" 的行

grep -i "hello" file.txt
# 忽略匹配字符串的大小写

grep -n "hello" file.txt
# 在匹配的行前面加上行号

grep -x "hello" file.txt
#  当整行为 "hello" 时才相匹配

grep -v "hello" file.txt
# 反向匹配, 显示不包含 "hello" 的行
```



2. 匹配文件

```sh
grep -l "hello" *.txt
# 显示当前目录下包含 "hello" 字符串的文件名, 不显示文件内容
```



3. 正则匹配

```sh
grep -E "hello|world" file.txt
# 使用正则匹配, 匹配 "hello" 或 "world"

grep -P '^([0-9]{3}-|\([0-9]{3}\) )[0-9]{3}-[0-9]{4}$' file.txt
# 使 grep 能够使用更强大的正则表达式语法
# 匹配 (xxx) xxx-xxxx 或 xxx-xxx-xxxx 格式的电话号码
```







#### 2.1.7 文本排序 sort

`sort` 命令用于对文本文件或输入数据进行排序，支持按字母、数值、时间等多种方式排序



1. 基本用法

```sh
sort filename
# 对 filename 文件的内容进行默认的按字典序（ASCII码）升序排序并输出到终端
```



2. 按数值排序

```sh
sort -n filename
# 选项按数值大小排序，而不是按照字典顺序

echo -e "10\n2\n30\n1" | sort -n
# 1
# 2
# 10
# 30
```



3. 逆序排序

```sh
sort -r filename
# 选项会逆序排序

sort -nr filename
# -nr 结合数值排序进行降序排列
```



4. 去重

```sh
sort -u filename
sort filename | uniq
# 去掉重复行并排序

echo -e "apple\nbanana\napple\norange" | sort -u
# apple
# banana
# orange
```



5. 排序后写入文件

```sh
sort filename > sorted.txt
# 将排序结果重定向到 sorted.txt, 避免输出到终端

sort -o filename filename
# 原地排序（即修改原文件）
```



6. 统计重复次数

```sh
sort filename | uniq -c
# uniq -c 统计每个单词出现的次数 数字在前, 字母在后

echo -e "apple\nbanana\napple\norange\nbanana" | sort | uniq -c
#  2 apple
#  2 banana
#  1 orange
```



7. 随机排序

```sh
sort -R filename
# 选项会随机打乱行顺序（不保证真正的随机, 每次结果可能不同）
```







#### 2.1.8 head 和 tail

`tail` 和 `head` 分别可以用于显示文件的**末尾**和**开头**部分



1. head

```sh
head file.txt
# 显示 file.txt 的前 10 行（默认）

head -n 20 file.txt
# 显示 file.txt 的前 20 行

head -c 50 file.txt
# 显示 file.txt 的前 50 字节

head file1.txt file2.txt
# 显示多个文件的前 10 行
```



2. tail

```sh
tail file.txt
# 显示 file.txt 的后 10 行（默认）

tail -n 20 file.txt
# 显示 file.txt 的后 20 行

tail -c 50 file.txt
# 显示 file.txt 的后 50 字节

tail -f log.txt
# 实时监视 log.txt 的变化
# 当 log.txt 更新时, tail -f 会实时输出新内容

tail -f log.txt | grep "ERROR"
# 实时查看 log.txt 中包含 "ERROR" 的行
# 检查 日志 中的报错


# head 和 tail 组合使用
head -n 20 file.txt | tail -n 10
# 先用 head -n 20 取前 20 行
# 再用 tail -n 10 取最后 10 行, 即 11~20 行
```











### 2.2 相关概念

#### 2.2.1 环境变量

当 shell 执行某个指令，但是该指令并不是 shell 所了解的编程关键字，那么它会去咨询==环境变量== `$PATH`

它会列出当 shell 接到某条指令时，进行程序搜索的路径：

```sh
echo $PATH
# /opt/ros/humble/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:...

which echo
# /usr/bin/echo

/bin/echo $PATH
# 等价于 echo $PATH , /usr 是默认路径的一部分, 可以省略
```

当我们执行 `echo` 命令时，shell 了解到需要执行 `echo` 这个程序，随后它便会在 `$PATH` 中搜索由 `:` 所分割的一系列目录，基于名字搜索该程序

当找到该程序时便执行（假定该文件是 *可执行程序*，后续课程将详细讲解）

确定某个程序名代表的是哪个具体的程序，可以使用 `which` 程序

我们也可以绕过 `$PATH`，通过直接指定需要执行的程序的路径来执行该程序



`$ ` 是一个特殊的符号，是用来访问或获取环境变量或 shell 变量值的符号

在 shell 中，变量的定义和引用是通过 `$` 来实现的



其他作用：

* `$?`：显示上一个命令的退出状态

* `$1`，`$2`，...，`$9`：用于引用脚本或函数中的参数

* `$0`：脚本名

* `$$`：显示当前进程的PID（进程ID）

* `$@`：所有参数

* `$#`：参数个数

* `!!`：完整的上一条命令，包括参数

    （当你因为权限不足执行命令失败时，可以使用 `sudo !!` 再尝试一次）



**例：**

```sh
# $?
ls /error_list
echo $?
# 2 , 返回 0 表示命令成功执行, 返回非 0 值表示命令执行失败

echo $USER
# czy2333 获取当前用户
echo $HOME
# /home/czy2333 获取当前用户的主目录
echo $RANDOM
# 15159 生成随机数
```







#### 2.2.2 文件权限

Linux 文件权限决定了 **谁** 可以对 **文件或目录** 执行 **哪些操作**

每个文件或目录都有 **3 种用户类别** 和 **3 种权限**

用户类别：

| 用户类别 | 符号 | 说明                         |
| -------- | ---- | ---------------------------- |
| 所有者   | `u`  | 创建文件的用户               |
| 所属组   | `g`  | 与所有者属于同一用户组的用户 |
| 其他人   | `o`  | 系统中的其他所有用户         |

权限：

| 权限 | 符号 | 作用                                  |
| ---- | ---- | ------------------------------------- |
| 读   | `r`  | 读取文件内容 / 列出目录内容           |
| 写   | `w`  | 修改文件内容 / 创建、删除目录内的文件 |
| 执行 | `x`  | 运行可执行文件 / 进入目录             |



1. 查看文件

```sh
ls -l file.txt
# -rw-r--r--  1 user user  1234 Feb 27 12:00 file.txt
# 第一个参数是该文件的权限

ls -l dir
# total 目录下文件数量
# 不能直接查看目录权限
```

参数拆解：

- `d`：第一位代表文件类型，`-` 是普通文件，`d` 是目录
- `rwx`：`2-4` 位为所有者权限，`5-7` 所属组权限，`8-10` 位为其他用户权限，没有权限用 `-` 表示



2. 修改权限

```sh
# 符号模式修改权限
chmod u+x file.txt	# 给所有者添加执行权限
chmod g-w file.txt  # 去掉所属组的写权限
chmod o+r file.txt  # 给予其他人读权限
chmod a-x file.txt  # 取消所有用户的执行权限
# + 为添加权限, - 为移除权限, a 为所有用户

# 数字模式修改权限
chmod 644 file.txt  # 所有者可读写，其他人只能读
chmod 700 script.sh # 仅所有者可读写执行，其他人无权限
chmod 777 file.txt  # 所有人都可读写执行（不安全）
# 用 3 个 八进制数字 表示
# r = 4, w = 2, x = 1
```







#### 2.2.3 重定向与管道

在 shell 中，程序有两个主要的==“流”==：它们的输入流和输出流

当程序尝试读取信息时，它们会从输入流中进行读取，当程序打印信息时，它们会将信息输出到输出流中

一般情况，一个程序的输入输出流都是终端，键盘作为输入，显示器作为输出



**重定向**

可以通过 `< file` 和 `> file` 将输入输出流重新定向到指定文件：

```sh
echo hello > hello.txt
# 将 hello.txt 作为 echo 命令的输出, hello.txt 的内容会被覆盖

echo hello >> hello.txt
# 令 hello.txt 以追加的形式作为输出

cat < hello.txt
# 将 hello.txt 文件的内容作为输入传递给 cat 命令

cat < file1.txt > file2.txt
# 将 file1.txt 文件的内容读取, 将其写入到 file2.txt 中
```



**管道**

使用管道（ pipes ），我们能够更好的利用文件重定向

 `|` 操作符允许我们将一个程序的输出和另外一个程序的输入连接起来

```sh
# 列出当前目录下所有 .txt 后缀的文件, 并显示它们的详细信息
ls -l | grep ".txt"

# 通过 curl 请求获取 http://example.com 网站的内容, 并将该内容的前 5 行输出
curl -s http://example.com | head -n 5
```







#### 2.2.4 sudo

根用户（root user），几乎不受任何限制，他可以创建、读取、更新和删除系统中的任何文件

通常在我们并不会以根用户的身份直接登录系统，因为这样可能会因为某些错误的操作而破坏系统

取而代之的是我们会在需要的时候使用 `sudo` 命令，作用是以  `su`（super user 或 root 的简写）的身份执行一些操作

当遇到拒绝访问（permission denied）的错误时，通常是因为此时必须是根用户才能操作



`sudo` 可以进行的操作：

```sh
# 安装、更新和删除软件
sudo apt install vim  # 安装 Vim 编辑器
sudo apt update       # 更新软件包列表
sudo apt remove vim   # 卸载 Vim

# 启动、停止或重启系统服务
sudo systemctl restart nginx  # 重启 Nginx 服务
sudo systemctl stop apache2   # 停止 Apache 服务
sudo systemctl start ssh      # 启动 SSH 服务

# 管理用户和权限
sudo useradd newuser       # 添加新用户
sudo passwd newuser        # 设置新用户密码
sudo userdel newuser       # 删除用户

# 修改文件权限
sudo chmod 755 /var/www/html  # 修改目录权限
sudo chown user:user file.txt # 修改文件所属用户

# 关机或重启系统
sudo shutdown -h now  # 立即关机
sudo reboot           # 立即重启
```









## 3 Shell 脚本

Shell 脚本是一种基于 Shell 解释执行的脚本程序，它由一系列 Shell 命令组成，通常用于自动化任务

需要使用Shell 解释器（如 `bash`、`sh`、`zsh`）执行，无需编译，适用于 Linux 和 Unix 系统

Shell 脚本的文件通常以 `.sh` 结尾，并且需要赋予可执行权限（`chmod +x script.sh`）后才能运行

本节中，我们会专注于 bash 脚本，因为它最流行，应用更为广泛







### 3.1 赋值

在 bash 中为变量赋值的语法是 `foo=bar`

创建了一个名为 `foo` 的变量，并将 `bar` 赋值给 `foo` 变量

访问变量中存储的数值，其语法为 `$foo`



注意：

* `foo = bar` （使用空格隔开）是不能正确工作的，因为解释器会调用程序 `foo` 并将 `=` 和 `bar` 作为参数

    在 shell 脚本中使用空格会起到分割参数的作用，有时候可能会造成混淆，请务必多加检查

* Shell 中的字符串通过 `'` 和 `"` 分隔符来定义，但是它们的含义并不相同
    * 以 `'` 定义的字符串为原义字符串，其中的变量不会被转义
    * 而 `"` 定义的字符串会将变量值进行替换



**例：**

```sh
foo=bar
echo "$foo"
# 打印 bar
echo '$foo'
# 打印 $foo
```







### 3.2 注释

在 Shell 脚本中，注释的写法如下



1. 单行注释

使用 `#` 号开头：

```sh
# 这是一个单行注释
echo "Hello, World!"  # 这也是注释, Shell 只执行 echo 命令
```



2. 多行注释

Shell 没有直接支持多行注释的语法，但可以使用其他方法来实现

```sh
# 使用多行 #
# 这是第一行注释
# 这是第二行注释
# 这是第三行注释

# 使用 : <<EOF
: <<EOF
这是一段多行注释
Shell 不会执行这里的内容
可以写很多行
EOF
```

说明：

- `:` 是 Shell 的空命令，相当于 `true`，不会执行 `<<EOF` 之后的内容
- `EOF` 也可以换成其他标识符，如 `COMMENT`（任意合法字符串），但首尾必须一致







### 3.3 判断

#### 3.3.0 判断符

Shell 可以使用大部分其他语言中的运算符，但条件判断符出现了一些改变

用于在 Shell 脚本中判断某些条件是否成立，从而决定是否执行某些操作

1. 数值比较：
    * `-eq`：等于
    * `-ne`：不等于
    * `-lt`：小于
    * `-le`：小于或等于
    * `-gt`：大于
    * `-ge`：大于或等于



2. 字符串比较：
    * `=`：等于（）
    * `!=`：不等于
    * `-z`：字符串长度为零
    * `-n`：字符串长度不为零



3. 文件测试：
    * `-e`：文件存在
    * `-d`：目录存在
    * `-f`：普通文件存在
    * `-r`：文件可读
    * `-w`：文件可写
    * `-x`：文件可执行







#### 3.3.1 if

注意：

* 在条件语句后需要跟 `then`

* 可以根据需求使用 `if` 结构， `if-else` 结构， `if-elif` 结构， `if-elif-else` 结构

    `elif` 相当于 `else if` 可以使用多次

```sh
if [ condition1 ]; then
    # 如果 condition 为 true 执行的命令
elif [ condition2 ]; then
    # 如果 another_condition 为 true 执行的命令
# elif [ condition3 ]; then
#	...
else
    # 如果以上条件都不成立时执行的命令
fi

# 例
x=5
if [ $x -gt 10 ]; then
    echo "x is greater than 10"
elif [ $x -eq 5 ]; then
    echo "x is equal to 5"
else
    echo "x is less than 5"
fi
```







#### 3.3.2 case

注意：

* 相当于其他编程语言中的 `switch` 语句
* 每个模式后面必须跟着 `;; `来结束该模式的匹配
* 表示默认匹配，即当没有任何模式匹配时执行的命令

```sh
case $variable in
    pattern1)
        # 如果变量匹配 pattern1 执行的命令
        ;;
    pattern2)
        # 如果变量匹配 pattern2 执行的命令
        ;;
    *)
        # 默认情况下执行的命令
        ;;
esac

# 例
fruit="apple"
case $fruit in
    "apple")
        echo "This is an apple."
        ;;
    "banana")
        echo "This is a banana."
        ;;
    *)
        echo "Unknown fruit"
        ;;
esac
```







#### 3.3.3 判断语句

当判断一个条件成立是否成立时，需要用 `[]` 或 `[[]]` 将条件框起来

* `[ ... ]` 是旧式的条件判断，兼容性更好，但功能较少，不支持 `&&` 和 `||` 这样的逻辑运算符

- `[[ ... ]]` 是一种扩展的条件判断语法，支持更多的运算符和逻辑操作

```sh
# [[]]
if [[ $num -ge 10 && $num -le 20 ]]; then
    echo "num 在 10 到 20 之间"
fi

# []
if [ $num -ge 10 ] && [ $num -le 20 ]; then
    echo "num 在 10 到 20 之间"
fi
```









### 3.4 循环

#### 3.4.1 for

注意：

* Shell 中的 `for` 循环相当于 c++ 中的高级 `for` 用于遍历一组值，通常用于固定次数的迭代，或者遍历一个列表或范围
* Shell 中可以使用 `{l..r}` 来表示一组 `l` 到 `r` 的数字范围

```sh
# for
for variable in value1 value2 value3; do
    # 执行的命令
done

# 例
# 遍历范围
for i in {1..5}; do
    echo "Number $i"
done

# 遍历列表
for fruit in apple banana cherry; do
    echo "我喜欢 $fruit"
done
```







#### 3.4.2 while

`while` 循环会在条件为真时反复执行指定的命令，直到条件为假

```sh
while [ condition ]; do
    # 执行的命令
done

# 例
while [ $x -le 5 ]; do
    echo "Number $x"
    ((x++))  # x 自增
done
```





#### 3.4.3 until

`until` 循环和 `while` 循环类似，但它的条件是==直到条件成立==

也就是说，`until` 循环会在条件为假时执行，直到条件为真

```sh
until [ condition ]; do
    # 执行的命令
done

# 例
x=1
until [ $x -gt 5 ]; do
    echo "Number $x"
    ((x++))  # x 自增
done
```







#### 3.4.4 跳转语句

Shell 中依然可以使用 `break` 和 `continue`

1. `break`：用于退出当前循环（无论是哪种循环）

```sh
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        break  # 当i等于5时退出循环
    fi
    echo "数字是 $i"
done
```



2. `continue`：用于跳过当前循环的剩余部分，进入下一次循环

```sh
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        continue  # 当i等于5时跳过当前循环
    fi
    echo "数字是 $i"
done
```







### 3.5 函数

`bash` 支持函数，它可以接受参数并基于参数进行操作

下面这个函数是一个例子，它会创建一个文件夹并使用 `cd` 进入该文件夹

```sh
mcd () {
    mkdir -p "$1"
    cd "$1"
}
```









### 3.6 返回码

返回码或退出状态是脚本和命令之间交流执行状态的方式

返回值 0 表示正常执行，其他所有非 0 的返回值都表示有错误发生

退出码可以搭配 `&&`（与操作符）和 `||`（或操作符）使用，用来进行条件判断，决定是否执行其他程序

它们都属于短路运算符，同一行的多个命令可以用 `;` 分隔

程序 `true` 的返回码永远是 `0`，`false` 的返回码永远是 `1`

让我们看几个例子

```sh
false || echo "Oops, fail"
# Oops, fail

true || echo "Will not be printed"
#

true && echo "Things went well"
# Things went well

false && echo "Will not be printed"
#

false ; echo "This will always run"
# This will always run
```







### 3.7 替换

**命令替换**

当通过 `$( CMD )` 这样的方式来执行 `CMD` 这个命令时，它的输出结果会替换掉 `$( CMD )`

例如，如果执行 `for file in $(ls)` ，shell 首先将调用 `ls` ，然后遍历得到的这些返回值



**进程替换**

Shell 允许在命令中将命令的输出作为文件提供给另一个命令

`<( CMD )` 会执行 `CMD` 并将结果输出到一个临时文件中，并将 `<( CMD )` 替换成临时文件名（而不是文件系统中的实际文件）

后续的命令可以像操作普通文件一样处理这些输出

例如， `diff <(ls foo) <(ls bar)` 会显示文件夹 `foo` 和 `bar` 中文件的区别，`diff` 必须接受两个文件做参数



这段脚本会遍历我们提供的参数，使用 `grep` 搜索字符串 `foobar`，如果没有找到，则将其作为注释追加到文件中

```sh
#!/bin/bash
# 命令替换
echo "Starting program at $(date)" # date会被替换成日期和时间
echo "Running program $0 with $# arguments with pid $$"

# 进程替换
touch foo/x bar/y
# 比较文件夹 foo 和 bar 中包含文件的不同
diff <(ls foo) <(ls bar)
# 输出
# < x
# ---
# > y
```





### 3.8 通配符

当执行脚本时，我们经常需要提供形式类似的参数

bash 使我们可以轻松的实现这一操作，它可以基于文件扩展名展开表达式

这一技术被称为 shell 的**通配**（*globbing*）

- 通配符 - 当你想要利用通配符进行匹配时，你可以分别使用 `?` 和 `*` 来匹配一个或任意个字符

    例如，对于文件 `foo`，`foo1`，`foo2`，`foo10` 和 `bar`

    `rm foo?` 这条命令会删除 `foo1` 和 `foo2`

     `rm foo*` 则会删除除了 `bar` 之外的所有文件

- 花括号 `{}` - 当你有一系列的指令，其中包含一段公共子串时，可以用花括号来自动展开这些命令

    这在批量移动或转换文件时非常方便

```sh
# convert 图像处理
convert image.{png,jpg}
# 会展开为
convert image.png image.jpg

cp /path/to/project/{foo,bar,baz}.sh /newpath
# 会展开为
cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

# 也可以结合通配使用
mv *{.py,.sh} folder
# 会移动所有 *.py 和 *.sh 文件

mkdir foo bar

# 下面命令会创建 foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h 这些文件
touch {foo,bar}/{a..h}
```









## 4 Shell 工具

### 4.1 awk

#### 4.1.1 相关概念

`awk` 是一个 **文本处理工具**，可对匹配行进行进一步操作，如字段提取、计算、格式化输出等



`grep` 和 `awk` 的区别：

| 比较项        | grep                                     | awk                                              |
| ------------- | ---------------------------------------- | ------------------------------------------------ |
| 主要功能      | 主要用于**搜索**匹配的行                 | 主要用于**文本处理**和**数据提取**               |
| 适用场景      | 在文件或标准输入中查找符合正则表达式的行 | 按列处理数据、提取字段、格式化输出、计算统计数据 |
| 字段处理      | 不能按字段提取，只能匹配整行             | 可以按字段提取 (`$1, $2, ...` 表示第 1、2 列)    |
| 数学计算      | 不能进行数学运算                         | 可以进行数值计算 (`$1 + $2` 等)                  |
| 逻辑控制      | 不能进行复杂逻辑判断                     | 支持 `if-else`、`for`、`while` 等控制结构        |
| 处理单词/字符 | 只能匹配整个模式                         | 可以按字段处理，并执行复杂操作                   |
| 修改内容      | 只能搜索，不能修改原始数据               | 可以修改原始数据，并输出处理后的结果             |



```sh
awk '{print $1}' file.txt
# 输出 file.txt 每行的第一个字段（默认以空格或制表符分隔）

awk '{print $1 $3}' file.txt
# 以空格作为分隔符, 输出第一列和第三列

awk '{print $1 "," $3}' file.txt
# 以逗号 , 作为分隔符, 输出第一列和第三列

awk '/hello/ {print $0}' file.txt
# 输出包含 "hello" 的行

awk '$3 > 50 {print $1, $3}' file.txt
# 条件判断, 如果第三列值大于 50, 则输出第一列和第三列

awk '{sum += $2} END {print "Total:", sum}' file.txt
# 计算列的总和, 计算第二列的总和

awk '{printf "Name: %s, Score: %d\n", $1, $2}' file.txt
# 格式化输出, 使用 printf 格式化输出，第一列是 Name，第二列是 Score
```









### 4.x 执行策略

PowerShell 的执行策略（Execution Policy）是一种安全机制

用于控制 PowerShell 脚本（`.ps1` 文件）的运行权限，防止恶意脚本未经授权执行

```powershell
# 了解当前执行策略
Get-ExecutionPolicy
# 禁止所有脚本运行 Restricted
# 所有脚本必须签名 AllSigned
# 允许运行本地脚本 RemoteSigned
# 允许运行所有脚本 Unrestricted
# 跳过所有检查 Bypass


# 临时更改策略
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass

# 永久更改策略
Set-ExecutionPolicy RemoteSigned -Force  # 允许本地脚本，远程脚本需签名
Set-ExecutionPolicy Unrestricted -Force  # 允许所有脚本（不安全）

# 检测不到 'Get-ExecutionPolicy'
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -Command "Get-ExecutionPolicy"
```

