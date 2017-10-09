---
layout:     post
title:      "Linux Command随记"
subtitle:   "第二篇"
date:       2017-09-25 12:00:00
author:     "Dingding"
header-img: "img/post/thrift-header.jpg"
header-mask: 0.3
catalog:    true
tags:
    - linux command
---

# awk
pattern scanning and text processing language

* 优秀博客

[三十分钟学会AWK](http://blog.jobbole.com/109089/)


* 工作流程

```flow
st=>start: Excute AWK commands from BEGIN block
e=>end: Excute AWK commands from END block

read=>operation: Read a line from input stream
execute=>operation: Execute AWK commands on a line
cond=>condition: Is it End of File

st->read->execute->cond
cond(yes)->e
cond(no,right)->read
```


* 程序框架

注意:BEGIN/End是awk的关键字,必须大写

```

BEGIN {awk-commands}

/pattern/ {awk-commands}

END {awk-commands}
```


# scp
secure copy (remote file copy program)

*  scp  [parameters] [[user@]host1:]file1 ... [[user@]host2:]file2
*  scp [参数] [原路径] [目标路径]



# uniq
report or omit repeated lines,由于只能检测出相邻的向同行，一般与sort命令结合使用。

* -c  prefix lines by the number of occurrences
* -d  only print duplicate lines, one for each group
* -i  ignore differences in case when comparing
* -u  only print unique lines


# cat
concatenate files and print on the standard output

* 显示文件 cat filename
* 创建文件 cat filename
* 合并文件 cat file1 file2 > file3
* 追加文件 cat file1 >> file2


# wc
* -c  print the byte counts  
      不可见字符也会算在内
* -l  print the newline counts  
      统计文件行数，文件结束符也算作了一个newline character
      
# !!  --  ~
* -- :无论如何，将其之后的 argument 视为一个文件名  
    rm --f -f :删除一个名为"-f"的文件
* !! :代表上一次输入shell的内容
* ~  :代表用户的主目录

# 重定向
```
* >  :如果文件不存在，就创建文件；如果文件存在，就将其清空
* >> :如果文件不存在，就创建文件；如果文件存在，则将新的内容追加到文件的末尾
```

# ln
创建链接,包括软连接和硬链接

* -s : ln -s source dest  
创建软连接,常用语将某个执行命令加入到PATH内的某个目录中





