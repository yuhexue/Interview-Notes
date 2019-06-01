### 查看进程

ps命令，显示所有运行中的进程：

```shell
# ps aux | less`
```

-A：显示所有进程

a：显示终端中包括其它用户的所有进程

x：显示无控制终端的进程

任务：查看系统中的每个进程。

```
# ps -A``# ps -e`
```

任务：查看非root运行的进程

```
# ps -U root -u root -N`
```

任务：查看用户vivek运行的进程

```
ps -u vivek
```

任务：top命令

top命令提供了运行中系统的动态实时视图。在命令提示行中输入top：

# top

https://www.cnblogs.com/yjd_hycf_space/p/7730690.html



https://www.cnblogs.com/-zyj/p/5763303.html