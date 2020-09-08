# linux-find命令


<!--more-->

> 同学们在面试的时候，不免经常被问到linux如何查询**某目录下包含某字符的文件**，一般可以采用`find+xargs+grep`

Linux `find` 命令用来在**指定目录下查找文件**。如果使用该命令时，不设置任何参数，则 find 命令将在当前目录下查找子目录与文件，并且将查找到的子目录和文件全部进行显示。

#### **语法**

```shell
find path -option [ -print ] [ -exec -ok command ] {} \
```

**参数说明** :

`find` 根据下列规则判断 `path` 和 `expression`，在命令列上第一个 `- ( ) , !` 之前的部份为 `path`，之后的是 `expression`。如果 `path` 是**空字串则使用当前路径**，如果 `expression` 是空字串则使用 `-print` 为预设 `expression`。

`expression` 中可使用的选项有二三十个之多，在此只介绍最常用的部份：

- `-mount, -xdev` : 只检查和指定目录在同一个文件系统下的文件，避免列出其它文件系统中的文件
- `-amin n` : 在过去 `n` 分钟内被读取过
- `-anewer file` : 比文件 `file` 更晚被读取过的文件
- `-atime n` : 在过去 `n` 天内被读取过的文件
- `-cmin n` : 在过去 `n` 分钟内被修改过
- `-cnewer file` : 比文件 `file` 更新的文件
- `-ctime n` : 在过去 `n` 天内被修改过的文件
- `-empty` : 空的文件 
- `- gid n or -group name` : `gid` 是 `n` 或是 `group` 名称是 `name`
- `-ipath p, -path p` : 路径名称符合 `p` 的文件，`ipath` 会忽略大小写
- `-name name, -iname name` : 文件名称符合 `name` 的文件。`iname` 会忽略大小写
- `-size n` : 文件大小 是 `n` 单位，`b` 代表 `512` 位元组的区块，`c` 表示字元数，`k` 表示 `kilo bytes`，`w` 是二个位元组。
- `-type` ：文件类型
  - `c` : 文件类型是 `c` 的文件。
  - `d`: 目录
  - `c`: 字型装置文件
  - `b`: 区块装置文件
  - `p`: 具名贮列
  - `f`: 一般文件
  - `l`: 符号连结
  - `s`: socket

- `-pid n` : `process id` 是 `n` 的文件

你可以使用 ( ) 将运算式分隔，并使用下列运算。

```shell
exp1 -and exp2
! expr
-not expr
exp1 -or exp2
exp1, exp2
```

#### **实例**

将目前目录及其子目录下所有后缀是 `c` 的文件列出来

```shell
find . -name "*.c"
```

将目前目录其其下子目录中所有一般文件列出

```shell
find . -type f
```

将目前目录及其子目录下所有最近 20 天内更新过的文件列出

```shell
find . -ctime -20
```

查找 `/var/log` 目录中更改时间在 7 日以前的普通文件，并在删除之前询问它们：

```shell
find /var/log -type f -mtime +7 -ok rm {} \
```

查找前目录中文件属主具有读、写权限，并且文件所属组的用户和其他用户具有读权限的文件：

```shell
find . -type f -perm 644 -exec ls -l {} \
```

为了查找系统中所有文件长度为 0 的普通文件，并列出它们的完整路径：

```shell
find / -type f -size 0 -exec ls -l {} \
```

#### 查找包含某字符的文件

查找包含某关键词的所有文件命令如下：

```shell
find -type f -name '*' | xargs grep '关键词'
```

查找固定后缀文件，如包含关键词`xxx`的所有后缀名为`.json`的文件：

```shell
find -type f -name '*.json' | xargs grep 'xxx'
```


