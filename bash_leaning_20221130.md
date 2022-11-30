# 1、命令格式
- 命令行环境中，主要通过使用 Shell 命令，进行各种操作。Shell 命令基本都是下面的格式。
```shell
$ command [ arg1 ... [ argN ]]

# 示例
$ ls -l
```
- command是具体的命令或者一个可执行文件，arg1 ... argN是传递给命令的参数，它们是可选的
- 有些参数是命令的配置项，这些配置项一般都以一个连词线`-`开头，比如上面的`-l`。同一个配置项往往有**长和短**两种形式，比如`-l`是短形式，`--list`是长形式，它们的作用完全相同。短形式便于手动输入，长形式一般用在脚本之中，可读性更好，利于解释自身的含义
```shell
# 短形式
$ ls -r

# 长形式
$ ls --reverse
```
- `-r`是短形式，`--reverse`是长形式，作用完全一样。前者便于输入，后者便于理解。

- Bash 单个命令一般都是一行，用户按下回车键，就开始执行。有些命令比较长，写成多行会有利于阅读和编辑，这时可以在每一行的结尾加上反斜杠，Bash 就会将下一行跟当前行放在一起解释。
```shell
$ echo foo bar

# 等同于
$ echo foo \
bar
```
## 1.3、空格
- `Bash` 使用空格（或 Tab 键）区分不同的参数
- `foo`和`bar`之间有一个空格，所以 `Bash` 认为它们是两个参数。
- 如果参数之间有多个空格，`Bash` 会自动忽略多余的空格
```shell
$ command foo bar

# a和test之间有多个空格，Bash 会忽略多余的空格
$ echo this is a     test
this is a test
```

## 1.4、分号
- 分号`（;）`是命令的结束符，使得一行可以放置多个命令，上一个命令执行结束后，再执行第二个命令
```shell
$ clear; ls
```
- Bash 先执行clear命令，执行完成后，再执行ls命令。
- 使用分号时，第二个命令总是接着第一个命令执行，不管第一个命令执行成功或失败。

## 1.5、命令组合符`&&`和`||`
- 如果Command1命令运行**成功**，则继续运行Command2命令。
```shell
Command1 && Command2
```
- 如果Command1命令运行**失败**，则继续运行Command2命令。
```shell
Command1 || Command2
```
- 示例说明
```shell
# 只要cat命令执行结束，不管成功或失败，都会继续执行ls
$ cat filelist.txt ; ls -l filelist.txt

# 只有cat命令执行成功，才会继续执行ls命令。
# 如果cat执行失败（比如不存在文件flielist.txt），那么ls命令就不会执行
$ cat filelist.txt && ls -l filelist.txt

# 只有mkdir foo命令执行失败（比如foo目录已经存在），才会继续执行mkdir bar命令。
# 如果mkdir foo命令执行成功，就不会创建bar目录了
$ mkdir foo || mkdir bar
```

## 1.6、type命令
- `type`命令用来判断命令的来源(命令是内置命令，还是外部程序)
- `type`命令告诉我们，`echo`是内部命令，`ls`是外部程序`（/bin/ls）`, `type`命令本身也是内置命令
```shell
$ type echo
echo is a shell builtin

$ type ls
ls is hashed (/bin/ls)

$ type type
type is a shell builtin
```

- 如果要查看一个命令的所有定义，可以使用`type`命令的`-a`参数
- `echo`命令既是内置命令，也有对应的外部程序
```shell
$ type -a echo
echo is shell builtin
echo is /usr/bin/echo
echo is /bin/echo
```

- `type`命令的`-t`参数，可以返回一个**命令的类型**：
  - 别名（alias），关键词（keyword），函数（function），内置命令（builtin）和文件（file）
- bash是文件，if是关键词
```shell
$ type -t bash
file

$ type -t if
keyword
```
