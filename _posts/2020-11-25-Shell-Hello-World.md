---
title: Shell Hello World
date: 2020-11-25
categories:
- Shell
---

`SHELL`: 命令/程序设计语言;<br>
SHELL本身是C语言编写的应用程序, 作为用户控制Linux操作系统的桥梁;<br>
最早的SHELL: 肯-汤普森为Unix编写的Bourne Shell (`sh`);<br>
最流行的SHELL: Bourne Again Shell (`bash`);<br>
Shell脚本的开头行: `#!/bin/bash` (#!指定解释该脚本文件的Shell程序);<br>

Hello World 小脚本 `hello.sh`<br>
```shell
#!/bin/bash
echo "Hello, World!"
```
给予脚本可执行权限 `chmod +x hello.sh`<br>
执行 `./hello.sh`<br>
结果<br>
```shell
Hello, World!
```

