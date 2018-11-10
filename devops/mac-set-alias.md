# Mac上设置常用命令的别名

<link rel="stylesheet" type="text/css" href="https://yanwei.github.io/auto-number-title.css" />

```json
{"Author":"yanwei", "LastUpdate":"2018-11-10"}
```

在Terminal执行：

```cmd
$ vi ~/.bash_profile
```

按自己的习惯，设置常用命令的别名或快捷启动命令。如`ls -la`定义成`ll`，设置`jupyter notebook`的快捷启动方式等。

```bash
alias ll='ls -la'
alias py=python3
alias pip=pip3
alias jn='cd ~/Jupyter && jupyter notebook'
```

`esc + :wq`保存文件，并执行如下命令，使设置立即生效。

```cmd
$ source ~/.bash_profile
```
