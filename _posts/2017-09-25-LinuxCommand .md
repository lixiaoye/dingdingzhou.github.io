---
layout:     post
title:      "Linux Command随记"
subtitle:   "第二篇"
date:       2017-09-20 14:00:00
author:     "Dingding"
header-img: "img/post/thrift-header.jpg"
header-mask: 0.3
catalog:    true
tags:
    - linux command
---

# awk
pattern scanning and text processing language

### 优秀博客


[三十分钟学会AWK](http://blog.jobbole.com/109089/)


### 工作流程

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


### 程序框架

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




