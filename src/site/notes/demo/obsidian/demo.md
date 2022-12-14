---
{"dg-publish":true,"permalink":"/demo/obsidian/demo/","tags":"gardenEntry"}
---


## 任务控制
**进程的控制**
1. 进程结束的原理
2. 进程的管理`jobs` `bg` `kill` 

## 终端多路复用tmux

当您在使用命令行时，您通常会希望同时执行多个任务。举例来说，您可以想要同时运行您的编辑器，并在终端的另外一侧执行程序。尽管再打开一个新的终端窗口也能达到目的，使用终端多路复用器则是一种更好的办法。

像 [`tmux`](https://www.man7.org/linux/man-pages/man1/tmux.1.html) 这类的终端多路复用器可以允许我们基于面板和标签分割出多个终端窗口，这样您便可以同时与多个 shell 会话进行交互。

不仅如此，终端多路复用使我们可以分离当前终端会话并在将来重新连接。

这让您操作远端设备时的工作流大大改善，避免了 `nohup` 和其他类似技巧的使用。

现在最流行的终端多路器是 [`tmux`](https://www.man7.org/linux/man-pages/man1/tmux.1.html)。`tmux` 是一个高度可定制的工具，您可以使用相关快捷键创建多个标签页并在它们间导航。

`tmux` 的快捷键需要我们掌握，它们都是类似 `<C-b> x` 这样的组合，即需要先按下`Ctrl+b`，松开后再按下 `x`。`tmux` 中对象的继承结构如下：

-   **会话** - 每个会话都是一个独立的工作区，其中包含一个或多个窗口
    -   `tmux` 开始一个新的会话
    -   `tmux new -s NAME` 以指定名称开始一个新的会话
    -   `tmux ls` 列出当前所有会话
    -   在 `tmux` 中输入 `<C-b> d` ，将当前会话分离
    -   `tmux a` 重新连接最后一个会话。您也可以通过 `-t` 来指定具体的会话
-   **窗口** - 相当于编辑器或是浏览器中的标签页，从视觉上将一个会话分割为多个部分
    -   `<C-b> c` 创建一个新的窗口，使用 `<C-d>`关闭
    -   `<C-b> N` 跳转到第  _N_ 个窗口，注意每个窗口都是有编号的
    -   `<C-b> p` 切换到前一个窗口
    -   `<C-b> n` 切换到下一个窗口
    -   `<C-b> ,` 重命名当前窗口
    -   `<C-b> w` 列出当前所有窗口
-   **面板** - 像 vim 中的分屏一样，面板使我们可以在一个屏幕里显示多个 shell
    -   `<C-b> "` 水平分割
    -   `<C-b> %` 垂直分割
    -   `<C-b> <方向>` 切换到指定方向的面板，<方向> 指的是键盘上的方向键
    -   `<C-b> z` 切换当前面板的缩放
    -   `<C-b> [` 开始往回卷动屏幕。您可以按下空格键来开始选择，回车键复制选中的部分
    -   `<C-b> <空格>` 在不同的面板排布间切换

扩展阅读： [这里](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) 是一份 `tmux` 快速入门教程， [而这一篇](http://linuxcommand.org/lc3_adv_termmux.php) 文章则更加详细，它包含了 `screen` 命令。您也许想要掌握 [`screen`](https://www.man7.org/linux/man-pages/man1/screen.1.html) 命令，因为在大多数 UNIX 系统中都默认安装有该程序


## 别名alias
#### eg
```Bash
alias alias_name="command_to_alias arg1 arg2"
```
>1. `=`两边是没有空格的，因为 [`alias`](https://www.man7.org/linux/man-pages/man1/alias.1p.html) 是一个 shell 命令，它只接受一个参数。
>2. 在默认情况下 shell 并不会保存别名。为了让别名持续生效，您需要将配置放进 shell 的启动文件里，像是`.bashrc` 或 `.zshrc`，下一节我们就会讲到

## 配置文件(Dotfiles)

对于 `bash`来说，在大多数系统下，您可以通过编辑 `.bashrc` 或 `.bash_profile` 来进行配置。在文件中您可以添加需要在启动时执行的命令，例如上文我们讲到过的别名，或者是您的环境变量。

实际上，很多程序都要求您在 shell 的配置文件中包含一行类似 `export PATH="$PATH:/path/to/program/bin"` 的命令，这样才能确保这些程序能够被 shell 找到。

还有一些其他的工具也可以通过_点文件_进行配置：

-   `bash` - `~/.bashrc`, `~/.bash_profile`
-   `git` - `~/.gitconfig`
-   `vim` - `~/.vimrc` 和  `~/.vim` 目录
-   `ssh` - `~/.ssh/config`
-   `tmux` - `~/.tmux.conf`

我们应该如何管理这些配置文件呢，它们应该在它们的文件夹下，并使用版本控制系统进行管理，然后通过脚本将其 **符号链接** 到需要的地方。

## 远端设备ssh
1. 用户密码登录
2. 通过密钥登录
3. 通过ssh复制文件`scp` `rsync`
4. 端口转发
5. `.ssh/config`配置文件，通过别名登录
