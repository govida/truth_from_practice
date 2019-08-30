# Linux

## ps -ef

| UID  | PID   | PPID  | C    | STIME     | TTY     | TIME    | CMD               |
| ---- | ----- | ----- | ---- | --------- | ------- | ------- | ----------------- |
| zzw  | 14124 | 13991 | 0    | 10:24下午 | ttys001 | 0:00.07 | /usr/local/python |

- UID：该进程归UID所有
- PID：进程ID
- PPID：该进程的父进程ID，特殊的进程0、1、2，[斯人若彩虹，遇上方可知](https://www.cnblogs.com/HKUI/articles/9557727.html)
- C：CPU调度情况，用于计算调度优先级
- STIME：进程启动时间
- TTY：启动者的终端机位置
- TIME：使用掉的CPU时间
- CMD：具体执行命令

命令参数

- -e：等价-A，显示全部进程
- -f：显示UID、PPID、C、STIME等全栏位（full）

## awk

### 基础

> awk '{print $2,  \$3, "lalala"  }' test.txt

- 输出每行的第二列
- 默认以空白符为分隔符，连续的若干空白符视为一个分隔符
- 输出不存在的列时，为空
- 自定义字符一定要加引号
- $ 数字 代表内置变量，不能加引号，否则会当作自定义字符输出
- $0 代表当前行

> awk '{print $NF}' test.txt

- 输出每行的最后一列
- $(NF-1) 代表倒数第二行

> awk '{print }' test.txt

- 输出当前行

### BEGIN、END

> awk 'BEGIN{print "123"} {print} END{print "bbb"}' test.txt

- BEGIN：在处理脚本前执行
- END：在处理脚本后执行
- 必须大写

