# 在Ubuntu和Mac上开启VIM语法高亮

<link rel="stylesheet" type="text/css" href="../auto-number-title.css" />

```json
{"Author":"yanwei", "LastUpdate":"2017-11-23"}
```

## Ubuntu

很简单，Ubuntu默认安装后预装的是一个tiny版本的VIM，删掉后重装完整版即可默认打开语法高亮。

```
$ sudo apt remove vim-common
$ sudo apt install vim
```

现在在用vi打开C、Python等源文件，就能看到语法高亮效果了。

## macOS

```
$ vi ~/.vimrc
```

如果没有`.vimrc`文件，会自动创建。然后加入以下内容，`:wq`保持即可。

```
set ai                  " auto indenting
set history=100         " keep 100 lines of history
set ruler               " show the cursor position
syntax on               " syntax highlighting
set hlsearch            " highlight the last searched term
filetype plugin on      " use the file type plugins
```
