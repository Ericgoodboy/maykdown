### nohup

nohup命令是由Command参数和任何相关的Arg组成，忽略所有挂断信号，在注销后使用nohup命令运行后台中的程序，要运行后台中的nohup命令添加&到末尾

nohup是no hang up的意思

nohup命令：如果你正在运行一个进程，而且你觉得在退出帐户时该进程还不会结束，那么可以使用nohup命令。该命令可以在你退出帐户/关闭终端之后继续运行相应的进程。

在缺省情况下该作业的所有输出都被重定向到一个名为nohup.out的文件中。

### 案例

```shell
nohup command > myout.file 2>&1 &
# 在这个例子中，0 - stdin 1 - stdout 2 -stderr
```

