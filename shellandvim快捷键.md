# 目录
[shell快捷键](#shell快捷键)  
[vim快捷键](#vim快捷键)

## shell快捷键
- 删除    
`ctrl + k`        删除光标后面所有字符  
`ctrl + u`		删除光标前面所有字符    
`ctrl + w`		删除光标前一个单词  
`ctrl + y`		粘贴由上一次ctrl+u/ctrl+d/ctrl+w删除的单词

- 移动  
`ctrl + a`      将光标移动到命令行开头  
`ctrl + e`      将光标移动到命令行结尾处   

- 替换  
`ctrl + t`       将光标当前字符与前面一个字符替换  

- 历史命令编辑  
`ctrl + r `      输入单词搜索历史命令

- 复制粘贴  
`ctrl + insert`	复制
`shift + insert`	粘贴

- 其他  
`ctrl + l`        清屏相当于命令clear  
`ctrl + c `       另起一行  

## vim快捷键

### 命令格式

vim的命令采用下面的格式。

```
[OPERATOR][NUMBER][MOTION]
```

Operator是动词。

- d – Delete (等同于cut命令)
- c – Change
- y – Yank
- p – Insert last deleted text after cursor (put command)
- r – Replace
- v - 可视化选择

Motion表示操作的上下文。

- w – 直到下一个单词的起始位置前面。
- s - sentence
- p - paragraph
- t - tag
- b - block
- e – 直到当前单词的最后一个位置。
- ^ - 跳至行首的第一个字符  
- $ – 直到当前行的最后一个位置。
- ) – 下一个句子的开始。
- ( – 当前句子的开始。
- } – 下一段的开始。
- { – 当前段的开始。
- ] – 下一段部分（section）的开始
- `[` – 当前部分（section）的开始
- `H` – 当前屏幕的顶部行
- `L` – 当前屏幕的最后一行

Count是可选的，表示command和motion的重复次数。

- i - inside
- a - around
- NUM: number (e.g.: 1, 2, 10)

实例

- dw 删除一个词
- d4w 删除四个词
- d$ 删除当前行
- dd 删除当前行（d$的快捷方式）
- d2$ 删除两行
- cis - Change inside sentence，删除当前句子，并进入insert模式
- yip - yank inside paragrah 复制当前段落

### 撤销命令

- u 撤销上个命令

### 移动光标
- `ctrl-f	`		上翻一页  
- `ctrl-b` 			下翻一页  
- `ctrl-u` 			上翻半页  
- `ctrl-d` 			下翻
- h – Left
- k – Up
- l – Right
- j – Down
- G 移动到文件最后一行
- gg 移动到文件第一行
- ctrl + g 查看当前文件总行数
- % 移动到当前代码区块的开始/结尾（匹配`()`，`[]`，`{}`）

### 多行缩进
- 按v进入visual状态，选择多行，用>或<缩进或缩出 

### 插入文字

- i 当前位置前面
- a 当前位置后面
- o 当前行下方新增一行
- O 当前行上方新增一行

### 删除

- x 删除当前字符

### 搜索，替换

- :/cat 搜索光标位置后面
- :?dogs 搜索光标位置前面
- n 移动到下一个匹配
- N 移动到上一个匹配
- :%s/old/new 只替换下一个
- :%s/old/new/g 替换所有
- :s/old/new/gc 索整个文件，将所有的old替换为new，每次都要你确认是否替换

### 执行shell命令

- :!ls -al

### 复制，粘贴，剪切

### 选择文本

- v+光标移动 （按字符选择）高亮选中所要的文本，然后进行各种操作（比如，d表示删除）。
- V （按行选择）
- v+选中的内容+c 更改选中的文字

### 复制：y(ank)

- y 用v命令选中文本后，用y进行复制
- yy 复制当前行，然后用p进行复制
- 5yy 复制从当前行开始的5行
- y_ 等同于yy
- Y 等同于yy
- yw 复制当前单词
- y$ 从当前位置复制到行尾
- y0 从当前位置复制到行首
- y^ 从当前位置复制到第一个非空白字符
- yG 从当前行复制到文件结束
- y20G 从当前行复制到第20行
- y?bar 复制至上一个出现bar的位置

### 粘贴

- p 在光标位置之后粘贴
- P 在光标位置之前粘贴

### 剪切

- v + 选中的内容 + d 剪切

### 剪贴板

（1） 简单复制和粘贴

vim提供12个剪贴板，它们的名字分别为vim有11个粘贴板，分别是`0`、`1`、`2`、`...`、`9`、`a`、`“`。如果开启了系统剪贴板，则会另外多出两个：`+`和`*`。使用`:reg`命令，可以查看各个粘贴板里的内容。

```bash
:reg
```

在vim中简单用y只是复制到`“`（双引号)粘贴板里，同样用p粘贴的也是这个粘贴板里的内容。

（2）复制和粘贴到指定剪贴板

要将vim的内容复制到某个粘贴板，需要退出编辑模式，进入正常模式后，选择要复制的内容，然后按"Ny完成复制，其中N为粘贴板号（注意是按一下双引号然后按粘贴板号最后按y），例如要把内容复制到粘贴板a，选中内容后按"ay就可以了。

要将vim某个粘贴板里的内容粘贴进来，需要退出编辑模式，在正常模式按"Np，其中N为粘贴板号。比如，可以按"5p将5号粘贴板里的内容粘贴进来，也可以按"+p将系统全局粘贴板里的内容粘贴进来。

（3）系统剪贴板

Vim支持系统剪贴板，需要打开clipboard功能。使用下面的命令，检查当前版本的Vim，是否支持clipboard。

```bash
$ vim --version
```

星号（`*`）和加号（`+`）粘贴板是系统粘贴板。在windows系统下， * 和 + 剪贴板是相同的。对于 X11 系统， * 剪贴板存放选中或者高亮的内容， + 剪贴板存放复制或剪贴的内容。打开clipboard选项，可以访问 + 剪贴板；打开xterm_clipboard，可以访问 * 剪贴板。 * 剪贴板的一个作用是，在vim的一个窗口选中的内容，可以在vim的另一个窗口取出。

复制到系统剪贴板
- `"*y`
- `"+y`
- `"+2yy` – 复制两行
- `{Visual}"+y` - copy the selected text into the system clipboard
- `"+y{motion}` - copy the text specified by {motion} into the system clipboard
- `:[range]yank +` - copy the text specified by `[range]` into the system clipboard

剪切到系统剪贴板
- `"+dd` – 剪切一行

从系统剪贴板粘贴到vim
- `"*p`
- `"+p`
- `Shift+Insert`
- `:put +` - Ex command puts contents of system clipboard on a new line
- `<C-r>`+ - From insert mode (or commandline mode)

`"+p`比 Ctrl-v 命令更好，它可以更快更可靠地处理大块文本的粘贴，也能够避免粘贴大量文本时，发生每行行首的自动缩进累积，因为`Ctrl-v`是通过系统缓存的stream处理，一行一行地处理粘贴的文本。

### 退出编辑  
`:w` 				将缓冲区写入文件，即保存修改  
`:wq` 			保存修改并退出  
`:q `				退出，如果对缓冲区进行过修改，则会提示   
`:q!` 			强制退出，放弃修改  

### 多文件编辑  
`vim file1..` 	同时打开多个文件  
`:args 	`		显示当前编辑文件  
`:next `			切换到下个文件  
`:prev 	`		切换到前个文件  
`:next！` 		不保存当前编辑文件并切换到下个文件  
`:prev！` 		不保存当前编辑文件并切换到上个文件  
`:wnext` 			保存当前编辑文件并切换到下个文件  
`:wprev` 			保存当前编辑文件并切换到上个文件  
`:first` 			定位首文件  
`:last` 			定位尾文件  
`ctrl+^` 			快速在最近打开的两个文件间切换  
`:split[sp]` 		把当前文件水平分割  
`:split file` 	把当前窗口水平分割, file  
`:vsplit file`    把当前窗口垂直分割, file  
`:new file`       同split file  
`:close `			关闭当前窗口  
`:only `			只显示当前窗口, 关闭所有其他的窗口  
`:all `			打开所有的窗口  
`:vertical all` 	打开所有的窗口, 垂直打开  
`:qall `			对所有窗口执行：q操作  
`:qall!` 			对所有窗口执行：q!操作  
`:wall` 			对所有窗口执行：w操作  
`:wqall` 			对所有窗口执行：wq操作  
`ctrl-w h `		跳转到左边的窗口  
`ctrl-w j `		跳转到下面的窗口  
`ctrl-w k `		跳转到上面的窗口  
`ctrl-w l` 		跳转到右边的窗口  
`ctrl-w t `		跳转到最顶上的窗口  
`ctrl-w b` 		跳转到最底下的窗口  

### VIM启动项  
`-o[n]` 以水平分屏的方式打开多个文件  
`-O[n]` 以垂直分屏的方式打开多个文件  

### 自动排版  
在粘贴了一些代码之后，vim变得比较乱，只要执行`gg=G`就能搞定 

### 查看diff
`:w !diff % -`     在vim中查看保存的版本和已编辑的相同文件的差异
`vimdiff file1 file2`    终端下输入该命令进入vim，垂直分隔窗口进行比较  
`vimdiff -o file1 file2`  水平分隔窗口进行比较  

### vim注释多行
注释多行   
1. 进入命令行模式，按ctrl + v进入 visual block模式，然后按j, 或者k选中多行，把需要注释的行标记起来
2. 按大写字母I，再插入注释符，例如//
3. 按esc键就会全部注释了

取消多行注释   
1. 进入命令行模式，按ctrl + v进入 visual block模式，按字母l横向选中列的个数，例如 // 需要选中2列
2. 按字母j，或者k选中注释符号(只要选中注释符号就可以了)
3. 按d键就可全部取消注释

在使用vim的时候，使用: TlistToggle 命令切换函数列表的开、关。 按住ctrl键然后按两下w键在正常编辑区域和tags区域中切换。 在tags区域中，把光标移动到变量、函数名称上，然后敲回车，就会自动在正常编辑区域中定位到指定内容了，很方便的。

### 代码折叠打开与折合
如果使用了indent方式，vim会自动的对大括号的中间部分进行折叠，我们可以直接使用这些现成的折叠成果。
indent 对应的折叠代码有：
`-zc`      折叠
`-zC`    对所在范围内所有嵌套的折叠点进行折叠
`-zo`      展开折叠
`-zO`     对所在范围内所有嵌套的折叠点展开
`-[z`      到当前打开的折叠的开始处。
`-]z`      到当前打开的折叠的末尾处。
`-zj`       向下移动。到达下一个折叠的开始处。关闭的折叠也被计入。
`-zk`     向上移动到前一折叠的结束处。关闭的折叠也被计入。


### 快速导航
- 跳转到最近打开的文件(ctrl o, ctr i)

### 行的折叠
:set wrap折叠，:set nowrap取消折叠

### 粘贴模式
: set paste  
: set nopaste