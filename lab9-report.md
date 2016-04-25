# ShellShock 攻击实验实验报告

Shellshock，又称Bashdoor，是在Unix中广泛使用的Bash shell中的一个安全漏洞，首次于2014年9月24日公开。许多互联网守护进程，如网页服务器，使用bash来处理某些命令，从而允许攻击者在易受攻击的Bash版本上执行任意代码。这可使攻击者在未授权的情况下访问计算机系统。这个实验的主要目的，是利用bash的漏洞，获得root权限。

### bash的执行代码漏洞

在bash7.1 及其之前的版本中，只要设定这样的环境变量，`export foo=’() { :; }; echo Hello World’ `，就能使得当用户运行bash时，打印出Hello World语句。这是因为bash的漏洞，使得在设置环境变量后，bash错误地执行了函数体后面的内容。

### 实验的具体内容
在C语言中，设定当前程序的effective uid为real uid，之后，随意执行一条命令。将该程序的owner设置为root后，执行`export foo=’() { :; };bash’`命令，接着执行该程序，就可以获得root的bash。

### 实验截图
![实验截图](https://raw.githubusercontent.com/srssyd/aos-paper-note/master/lab9-screenshot.png)

### 该漏洞的其他危害

由于目前仍有不少web server使用cgi，使得该漏洞变得更加危险。比如，通过更改user-agent，就能使得攻击者在远程服务器上恶意地执行bash命令，进而获得服务器的控制权。