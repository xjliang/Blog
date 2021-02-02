---
title:  打造高效的工作环境 – SHELL 篇
author: 陈皓
date: 2021-02-02 09:45:30 +0800
categories: [Technology]
tags: [shell]
---

程序员是一个很懒的群体，总想着能够让代码为自己干活，他们不断地把工作生活中的一些事情用代码自动化了，从而让整个社会的效率运作地越来越高。所以，程序员在准备去优化这个世界的时候，都会先要优化自己的工作环境，是所谓 “工欲善其事，必先利其器”。

我们每个程序员都应该打造一套让自己更为高效的工作环境。那怕就是让你少输入一次命令，少按一次键，少在鼠标和键盘间切换一次，都会让程序员的工作变得更为的高效。所以，程序员一般需要一台性能比较好，不会因为开了太多的网页或程序就卡得不行的电脑，还要配备多个显示器，一个显示器写代码，一个查文档，一个测试运行结果，而不必在各种窗口来来回回的切换…… 在大量的窗口间切换经常会迷路，而且也容易出错（分不清线上或测试环境）……

除了硬件上的装备，软件上也是能够提升程序员生产力的地方，**在软件层面提升程序员生产力的东西有一个很重要的事就是命令行和脚本**，使用鼠标和图形界面则会大大降低程序员的生产力。酷壳以前也写过一些，如《[你可能不知道的 Shell](https://coolshell.cn/articles/8619.html)》和《 [应该知道的 Linux 技巧](https://coolshell.cn/articles/8883.html)》，但是 Unix/Linux Shell 就是一个大宝库，怎么写也写不完，不然，怎么会有 “Where is the Shell, there is a way”。



#### 命令行

在不同的操作系统下，都有着很不错的命令行工具，比如 Mac 下的 **Iterm2**，Linux 下的原生命令行，如果你是在 Windows 下工作，问题也不大，因为 Windows 下现在有了 **WSL**。WSL 提供了一个由微软开发的 Linux 兼容的内核接口（不包含 Linux 内核代码），然后可以在其上运行 GNU 用户空间，例如 Ubuntu，openSUSE，SUSE Linux Enterprise Server，Debian 和 Kali Linux。这样的用户空间可能包含 Bash shell 和命令语言，使用本机 GNU/Linux 命令行工具（sed，awk 等），编程语言解释器（Ruby，Python 等），甚至是图形应用程序（使用主机端的 X 窗口系统）。

使用命令行可以完成所有日常的操作，新建文件夹（mkdir）、新建文件（touch）、移动（mv）、复制（cp）、删除（rm）等等。而且使用 Linux/Unix 命令行最好的方式是可以用 `awk`、`sed`、`grep`、`xargs`、`find`、`sort` 等等这样的命令，然后用管道把其串起来，就可以完成一个你想要的功能，尤其是一些简单的数据统计功能。这是 Linux 命令行不可比拟的优势。比如：

- 查看连接你服务器 top10 用户端的 IP 地址：

```
netstat -nat | awk '{print $5}' | awk -F ':' '{print $1}' | sort | uniq -c | sort -rn | head -n 10
```

- 查看一下你最常用的 10 个命令：

```
cat .bash_history | sort | uniq -c | sort -rn | head -n 10 (or cat .zhistory | sort | uniq -c | sort -rn | head -n 10
```

（注：`awk` 和 `sed` 是两大神器，所以，我以前的也有两篇文章来介绍它们 ——《[awk 简明教程](https://coolshell.cn/articles/9070.html)》和《 [sed 简明教程](https://coolshell.cn/articles/9104.html)》，你可以前往一读）

在命令行中使用 **alias** 可以将使用频率很高命令或者比较复杂的命令合并成一个命令，或者修改原生的命令。

下面这几个命令，可能是你天天都在敲的。所以，你应该设置成 alias 来提高效率

```sh
alias nis="npm install --save "

alias svim='sudo vim'

alias mkcd='foo(){ mkdir -p "$1"; cd "$1" }; foo '

alias install='sudo apt get install'

alias update='sudo apt-get update; sudo apt-get upgrade'

alias ..="cd .."

alias ...="cd ..; cd .."

alias www='python -m SimpleHTTPServer 8000'

alias sock5='ssh -D 8080 -q -C -N -f user@your.server'
```



你还可以参考如下的一些文章，看看别人是怎么用好 `alias` 的

- [30 Handy Bash Shell Aliases For Linux / Unix / Mac OS X](https://www.cyberciti.biz/tips/bash-aliases-mac-centos-linux-unix.html)
- [What are your favorite bash aliases?](https://www.digitalocean.com/community/questions/what-are-your-favorite-bash-aliases)
- [23 Handy Bash Shell Aliases For Unix, Linux, and Mac OS X](https://www.linuxtrainingacademy.com/23-handy-bash-shell-aliases-for-unix-linux-and-mac-os-x/)
- [A few more of my favorite Bash aliases](https://brettterpstra.com/2013/03/31/a-few-more-of-my-favorite-shell-aliases/)

命令行中除了原生的命令之外，还有很多可以提升使用体验的工具。下面罗列一些很不错的命令，把原生的命令增强地很厉害:

- [**fasd**](https://github.com/clvv/fasd) 增强了 `cd` 命令 。
- [**bat**](https://github.com/sharkdp/bat) 增强了 `cat` 命令 。如果你想要有语法高亮的 `cat`，可以试试 [**ccat**](https://github.com/jingweno/ccat) 命令。
- [**exa**](https://github.com/ogham/exa) 增强了 `ls` 命令，如果你需要在很多目录上浏览各种文件 ，[**ranger**](https://github.com/ranger/ranger) 命令可以比 `cd` 和 `cat` 更有效率，甚至可以在你的终端预览图片。
- [**fd**](https://github.com/sharkdp/fd) 是一个比 `find` 更简单更快的命令，他还会自动地忽略掉一些你配置在 `.gitignore` 中的文件，以及 `.git` 下的文件。
- [**fzf**](https://github.com/junegunn/fzf) 会是一个很好用的文件搜索神器，其主要是搜索当前目录以下的文件，还可以使用 `fzf --preview 'cat {}'` 边搜索文件边浏览内容。
- `grep` 是一个上古神器，然而，[**ack**](https://beyondgrep.com/)、[**ag**](https://github.com/ggreer/the_silver_searcher) 和 [**rg**](https://github.com/BurntSushi/ripgrep) 是更好的 grep，和上面的 `fd` 一样，在递归目录匹配的时候，会使用你配置在 `.gitignore` 中的规则。
- `rm` 是一个危险的命令，尤其是各种 `rm -rf …`，所以，[**trash**](https://github.com/andreafrancia/trash-cli/) 是一个更好的删除命令。
- `man` 命令是好读文档的命令，但是 man 的文档有时候太长了，所以，你可以试试 [**tldr**](https://github.com/tldr-pages/tldr) 命令，把文档上的一些示例整出来给你看。
- 如果你想要一个图示化的 `ping`，你可以试试 [**prettyping**](https://github.com/denilsonsa/prettyping) 。
- 如果你想搜索以前打过的命令，不要再用 Ctrl +R 了，你可以使用加强版的 [**hstr**](https://github.com/dvorka/hstr) 。
- [**htop**](https://hisham.hm/htop/) 是 top 的一个加强版。然而，还有很多的各式各样的 top，比如：用于看 IO 负载的 [**iotop**](http://guichaz.free.fr/iotop/)，网络负载的 [**iftop**](http://www.ex-parrot.com/~pdw/iftop/), 以及把这些 top 都集成在一起的 [**atop**](https://github.com/Atoptool/atop)。
- [**ncdu**](https://dev.yorhel.nl/ncdu) 比 du 好用多了用。另一个选择是 [nnn](https://github.com/jarun/nnn)。
- 如果你想把你的命令行操作建录制成一个 SVG 动图，那么你可以尝试使用 [**asciinema**](https://asciinema.org/) 和 [**svg-trem**](https://github.com/marionebl/svg-term-cli) 。
- [**httpie**](https://github.com/jakubroztocil/httpie) 是一个可以用来替代 `curl` 和 `wget` 的 http 客户端，`httpie` 支持 json 和语法高亮，可以使用简单的语法进行 http 访问: `http -v github.com`。
- [**tmux**](https://github.com/tmux/tmux) 在需要经常登录远程服务器工作的时候会很有用，可以保持远程登录的会话，还可以在一个窗口中查看多个 shell 的状态。
- [**Taskbook**](https://github.com/klaussinani/taskbook) 是可以完全在命令行中使用的任务管理器 ，支持 ToDo 管理，还可以为每个任务加上优先级。
- [**sshrc**](https://github.com/Russell91/sshrc) 是个神器，在你登录远程服务器的时候也能使用本机的 shell 的 rc 文件中的配置。
- [**goaccess**](https://github.com/allinurl/goaccess) 这个是一个轻量级的分析统计日志文件的工具，主要是分析各种各样的 access log。

关于这些增加命令，主要是参考自下面的这些文章

1. [10 Tools To Power Up Your Command Line](https://dev.to/_darrenburns/10-tools-to-power-up-your-command-line-4id4)
2. [5 More Tools To Power Up Your Command Line (Part 2 Of Series)](https://dev.to/_darrenburns/tools-to-power-up-your-command-line-part-2-2737)
3. [Power Up Your Command Line, Part 3](https://dev.to/_darrenburns/power-up-your-command-line-part-3-4o53)
4. [Power Up Your Command Line](https://darrenburns.net/posts/tools/)
5. [Hacker Tools](https://hacker-tools.github.io/)

#### Shell 和脚本

shell 是可以与计算机进行高效交互的文本接口。shell 提供了一套交互式的编程语言（脚本），shell 的种类很多，比如 **sh**、**bash**、**zsh** 等。

shell 的生命力很强，在各种高级编程语言大行其道的今天，很多的任务依然离不开 shell。比如可以使用 shell 来执行一些编译任务，或者做一些批处理任务，初始化数据、打包程序等等。

现在比较流行的是 **zsh** + [**oh-my-zsh**](https://ohmyz.sh/) + [**zsh-autosuggestions**](https://github.com/zsh-users/zsh-autosuggestions) 的组合，你也可以试试看。其中 zsh 和 oh-my-zsh 算是常规操作了，但是 zsh-autosuggestions 特别有用，可以超级快速的帮你补全你输入过的命令，让命令行的操作更加高效。

另外，**[fish](https://fishshell.com/)** 也是另外一个牛逼的 shell，比如：命令行自动完成（根据历史记录），命令行命令高亮，当你要输入命令行参数的时候，自动提示有哪些参数…… fish 在很多地方也是用起来很爽的。和上面的 oh-my-zsh 有点不分伯仲了。

你也许会说，用 Python 脚本或 PHP 来写脚本会比 Shell 更好更没有 bug，但我要申辩一下:

- 其一，如果你有一天要维护线上机器的时候，或是到了银行用户的系统（与外网完全隔离，而且服务器上没有安装 Python/PHP 或是他们的的高级库，那么，你只有 Shell 可以用了）。
- 其二，而且，如果要跟命令行交互很多的话，Shell 是不二之选，试想一下，如果你要去 100 台远程的机器上查 access.log 日志中有没有某个错误，完成这个工作你是用 PHP/Python 写脚本快还是用 Shell 写脚本快呢？

所以，**我们还要学会只使用传统的 grep/awk/sed 等等这些 POSIX 的原生的系统默认安装的命令**。

当然，要写好一个脚本并不容易，下面有一些小模板供你参考：

处理命令行参数的一个样例

```sh
while [ "$1" != "" ]; do
    case $1 in
        -s  )   shift  
    SERVER=$1 ;;  
        -d  )   shift
    DATE=$1 ;;
  --paramter|p ) shift
    PARAMETER=$1;;
        -h|help  )   usage # function call
                exit ;;
        * )     usage # All other parameters
                exit 1
    esac
    shift
done 
```

命令行菜单的一个样例

```sh
#!/bin/bash
# Bash Menu Script Example

PS3='Please enter your choice: '
options=("Option 1" "Option 2" "Option 3" "Quit")
select opt in "${options[@]}"
do
    case $opt in
        "Option 1")
            echo "you chose choice 1"
            ;;
        "Option 2")
            echo "you chose choice 2"
            ;;
        "Option 3")
            echo "you chose choice $REPLY which is $opt"
            ;;
        "Quit")
            break
            ;;
        *) echo "invalid option $REPLY";;
    esac
done
```

颜色定义，你可以使用 `echo -e "${Blu}blue ${Red}red ${RCol}etc...."` 进行有颜色文本的输出

```sh
RCol='\e[0m'    # Text Reset

# Regular           Bold                Underline           High Intensity      BoldHigh Intens     Background          High Intensity Backgrounds
Bla='\e[0;30m';     BBla='\e[1;30m';    UBla='\e[4;30m';    IBla='\e[0;90m';    BIBla='\e[1;90m';   On_Bla='\e[40m';    On_IBla='\e[0;100m';
Red='\e[0;31m';     BRed='\e[1;31m';    URed='\e[4;31m';    IRed='\e[0;91m';    BIRed='\e[1;91m';   On_Red='\e[41m';    On_IRed='\e[0;101m';
Gre='\e[0;32m';     BGre='\e[1;32m';    UGre='\e[4;32m';    IGre='\e[0;92m';    BIGre='\e[1;92m';   On_Gre='\e[42m';    On_IGre='\e[0;102m';
Yel='\e[0;33m';     BYel='\e[1;33m';    UYel='\e[4;33m';    IYel='\e[0;93m';    BIYel='\e[1;93m';   On_Yel='\e[43m';    On_IYel='\e[0;103m';
Blu='\e[0;34m';     BBlu='\e[1;34m';    UBlu='\e[4;34m';    IBlu='\e[0;94m';    BIBlu='\e[1;94m';   On_Blu='\e[44m';    On_IBlu='\e[0;104m';
Pur='\e[0;35m';     BPur='\e[1;35m';    UPur='\e[4;35m';    IPur='\e[0;95m';    BIPur='\e[1;95m';   On_Pur='\e[45m';    On_IPur='\e[0;105m';
Cya='\e[0;36m';     BCya='\e[1;36m';    UCya='\e[4;36m';    ICya='\e[0;96m';    BICya='\e[1;96m';   On_Cya='\e[46m';    On_ICya='\e[0;106m';
Whi='\e[0;37m';     BWhi='\e[1;37m';    UWhi='\e[4;37m';    IWhi='\e[0;97m';    BIWhi='\e[1;97m';   On_Whi='\e[47m';    On_IWhi='\e[0;107m';
```



取当前运行脚本绝对路径的示例：（注：Linux 下可以用 `dirname $(readlink -f $0)` ）

```sh
FILE="$0"
while [[ -h ${FILE} ]]; do
    FILE="`readlink "${FILE}"`"
done
pushd "`dirname "${FILE}"`" > /dev/null
DIR=`pwd -P`
popd > /dev/null
```

如何在远程服务器运行一个本地脚本

```sh
#无参数
ssh user@server 'bash -s' < local.script.sh

#有参数
ssh user@server ARG1="arg1" ARG2="arg2" 'bash -s' < local_script.sh
```

如何检查一个命令是否存在，用 `which` 吗？最好不要用，因为很多操作系统的 `which` 命令没有设置退出状态码，这样你不知道是否是有那个命令。所以，你应该使用下面的方式。

```sh
# POSIX 兼容:
command -v [the_command]

# bash 环境:
hash [the_command]
type [the_command]

# 示例：
gnudate() {
    if hash gdate 2> /dev/null; then
        gdate "$@"
    else
        date "$@"
    fi
}
```

然后，如果要写出健壮性更好的脚本，下面是一些相关的技巧：

- 使用 `-e` 参数，如：`set -e` 或是 `#!/bin/sh -e`，这样设置会让你的脚本出错就会停止运行，这样一来可以防止你的脚本在出错的情况下还在拼拿地干活停不下来。
- 使用 `-u` 参数，如： `set -eu`，这意味着，如果你代码中有变量没有定义，就会退出。
- 对一些变理，你可以使用默认值。如：`${FOO:-'default'}`
- 处理你代码的退出码。这样方便你的脚本跟别的命令行或脚本集成。
- 尽量不要使用 `;` 来执行多个命令，而是使用 `&&`，这样会在出错的时候停止运行后续的命令。
- 对于一些字符串变量，使用引号括起，避免其中有空格或是别的什么诡异字符。
- 如果你的脚有参数，你需要检查脚本运行是否带了你想要的参数，或是，你的脚本可以在没有参数的情况下安全的运行。
- 为你的脚本设置 `-h` 和 `--help` 来显示帮助信息。千万不要把这两个参数用做为的功能。
- 使用 `$()` 而不是 “ 来获得命令行的输出，主要原因是易读。
- 小心不同的平台，尤其是 MacOS 和 Linux 的跨平台。
- 对于 `rm -rf` 这样的高危操作，需要检查后面的变量名是否为空，比如：`rm -rf $MYDIDR/*` 如果 `$MYDIR` 为空，结果是灾难性的。
- 考虑使用 “find/while” 而不是 “for/find”。如：`for F in $(find . -type f) ; do echo $F; done` 写成 `find . -type f | while read F ; do echo $F ; done` 不但可以容忍空格，而且还更快。
- 防御式编程，在正式执行命令前，把相关的东西都检查好，比如，文件目录有没有存在。

你还可以使用 ShellCheck 来帮助你检查你的脚本。

- https://www.shellcheck.net/

最后推荐一些 Shell 和脚本的参考资料。

各种有意思的命令拼装，一行命令走天涯:

- http://www.bashoneliners.com/
- http://www.shell-fu.org/
- http://www.commandlinefu.com/

下面是一些脚本集中营，你可以在里面淘到各种牛 X 的脚本：

- http://www.shelldorado.com/scripts/
- https://snippets.siftie.com/public/tag/bash/
- https://bash.cyberciti.biz/
- https://github.com/alexanderepstein/Bash-Snippets
- https://github.com/miguelgfierro/scripts
- https://github.com/epety/100-shell-script-examples
- https://github.com/ruanyf/simple-bash-scripts

甚至写脚本都可以使用框架:

- 写 bash 脚本的框架 https://github.com/Bash-it/bash-it

Google 的 Shell 脚本的代码规范：

- https://google.github.io/styleguide/shell.xml

最后，别忘了几个和 shell 有关的索引资源：

- https://github.com/alebcay/awesome-shell
- https://github.com/awesome-lists/awesome-bash
- https://terminalsare.sexy/



来源：[打造高效的工作环境 – Shell 篇 | 酷 壳 - CoolShell](https://coolshell.cn/articles/19219.html)