# vim 命令

## Vim模式

主要的 4 种模式：  

* Normal 模式：默认进入的模式，也是常用的模式  
* Insert 模式：插入模式，像正常的文本编辑器一样输入
* Command 模式：命令模式，在底部输入命令  
* Visual 模式：可视模式，对文本进行选择  

### Normal 模式：光标移动  

* hjkl：上下左右  
* gg：跳到第一行（类似 Home 键）  
* G：跳到最后一行（类似 End 键）  
* \<Ctrl-u>/\<Ctrl-b>:往上翻半页/一页（类似 PageUp 键）  
* \<Ctrl-d>/\<Ctrl-f>:往下翻半页/一页（类似 PageDown 键）  
* {lineno}gg：跳到第 lineno 行  
* zz / zt / zb : 渔村行设置为屏幕居中/屏幕第一行/屏幕最后一行  

* 剪切/复制 选中后x/y
* 粘贴  p
* 下一处单词的开头 w
* 上一处单词的开头 b
* 下一处单词的结尾 e
* 上一处单词的结尾 ge
 wbe大写版本WEB对应的单词是连续的非空字符  

1. insert模式  
Normal模式下通过特定命令进入insert模式  

* i: 代表“insert”，当前光标之前开始输入  
* a: 代表“append”，当前光标之后开始输入  
* o: 下方插入新的一行，然后开始输入  
* I: 在本行的开头开始输入  
* A: 在本行的f  
* o: 下方插入新的一行，然后开始输入  
* I: 在本行的开头开始输入  
* A: 在本行的末尾开始输入  
* O：上方插入新的一行，然后开始输入  
* S: 删除当前行，然后开始输入  

Visual 模式：
    Normal模式下按v进入可视模式
    进入可视模式后可以用Normal模式下的移动命令选择文本
    可视模式下，x/y: 剪切/复制；回到Normal模式下，p: 粘贴
    Normal模式下按V进入行可视模式，一次选中一整行，在需要选中多行时很方便
    * \<Esc>回到Normal模式

基于搜索的移动
行内搜索：
 · f{char}/t{char}: 跳转到本行下一个 char 字符出现处/前
 · ;/,:   快速向后/向前重复 ft 查找
 · F{char}/T{char}: 往前搜索而非往后
文件中搜索：
 · /{pattern}:  跳转到文件中下一个 pattern 出现的地方
 · ?{pattern}:  跳转到本文件中上一个 pattern 出现的地方
 · {pattern} 可以是正则表达式
 · * ： 等价于 /{pattern}, pattern 是当前光标下的单词
 · n/N : 快速重复 / 查找
基于标记的移动
 · m{mark} :  把当前位置标记为 mark
 · `{mark} :  跳转到名为 mark 的标记位置
 mark 是 a-z 的字符
 常用场景：当需要临时离开当前光标处，做一些事情后再快速地回来
 我比较习惯用的标记是 mm
 内置标记：
 · `` ：上次跳转前的位置
 ·`. : 上次修改的位置
 · `^ ：上次插入的位置
其他实用的跳转
 · ^/$ : 跳转到本行的开始/结尾
 · % ：跳到时匹配的配对符（括号等处）

Operator * Motion = Action
{operator}{motion} : 一次编辑动作
常见操作符：   例子
 · c : 代表“change”，修改，删除内容 · dgg : 删除到时第一行
   并进入插入模式   · ye  : 复制到时单词结尾
 · d ：代表“delete”，删除  · d$  : 删除到行尾
 · y : 代表“yank”，复制   · dt; : 删除直到分号为止的内容
 · v : 代表“visual”，选中文本，进入
   可视模式

   “操作符+移动”是非常重要的操作逻辑，他允许你组合出各种动作
   大部分操作符连续按两次（cc/dd/yy）：将其作用在这一行上
 · dd  : 删除这一行   · cc 删除这一行，并进入编辑状态

重复操作： . 命令
 · . ：重复上一次修改
 · u : 撤销上一次修改
 · \<Ctrl-r> : 重做上一次修改
 . 命令非常适合用于需要多次重复某一个修改动作的场景
 省去了重复输入命令，大大提高效率

批量操作： 数字 \* 动作  
{count}{action} : 重复 count 次 action 动作  
动作可以是移动动作或是编辑动作  
 · 4j : 向下移动 4 行  
 · 3dw : 删除3个单词  
 · 2yy : 复制2行  
 · 4p  ：粘贴4次  
数字 \* 动作，是一种重要的批量操作方式，命令直观，语义明确  
 · . 命令可以直观地看到每一次的变化，在合适的时候停止
 · 数字 * 动作则需要知道动作的次数

技巧：使用相对行号确定移动范围
当涉及行操作时，使用相对行号能够更直观地确定范围
:set relativenumber: 开启
:set norelativenumber: 关闭

{operator}{textobjects}操作逻辑  
textobjects : 语义化的文本片段  
格式： i/a * 对象  
常见的对象：  
 · w/W, s, p : 单词、句子、段落  
 · (/), [/], {/}, </>, '/" : 配对符定义的对象  
i 代表“inner"，内部；a 代表”a”，额外包括周围的空格或配对符  
例：Vim is a (screen-based) editor.  
光标在句首第一个字母V时，iw表示Vim三个字母，而aw表示Vim加上后面的空格  
另外，i(表示括号内的内容，即screen-based，而a(表示包括两端的括号，即
(screen-based).  
文本对象提供了为文本赋予了结构化的含义，允许我们以一个语义对象作为操作单元  
[count]{operator}{textobjects}  
 · diw ：删除一个单词  
 · ci( : 修改小括号内部  
 · yi{ : 复制大括号内部  
通过组合 operator 与 textobjects，可以对不同的语义实施不同的操作，不仅十分灵活，而且语义明确，容易记忆  
配合 . 命令或[count] 可以简单地完成多次对特定语义对象的操作  

扩展  
{operator}{montion} 与 {oprator}{textobjects}  
可以通过自定义operator,motion,textobjects 进行扩展，实现更强大的操作  
 · vim-easymotion : 扩展 motion，更强大的跳转功能  
 · vim-surround : 定义了添加配对符{括号、引号等)的operator  
 · vim-commentary : 定义了添加注释的 operator  
 · targets.vim : 扩展 textobjects ，定义了新的文本对象、如函数参数等  

操作符与命令补充  
 · gu / gU /g~ : 操作符，转小写/转大写/翻转大小写  
 · 单独使用~，会改变当前光标处字母的大小写  
 · J ： join，连接两行  
 · \<Ctrl-a> / \<Ctrl-x> : 增加数字/减少数字  
 · g\<Ctrl-A> : 创建递增序列  
 · </> : 左右缩进

寄存器  
Vim 提供了许多寄存器用于存放内容，可以理解为剪贴板,
一个字符对应一个寄存器（如 a-z，0-9）  
特别的寄存器：  
 · “ ：默认寄存器，平时复制、删除的内容都放在里面  
 · % ：当前文件名  
 · . ：上一次插入的内容  
 · : ：上一次执行的命令  
通过 :reg {register} 查重对应寄存器中的内容  

指定寄存器  
在复制/删除/粘贴等操作前加上 "{register} 就可以指定本次操作所用的寄存器  
只要涉及寄存器操作的都可以这样指定  
 · "ayy ：将这一行复制到 a 寄存器中  
 · "bdiw : 将单词删除，保存到 b 寄存器中  
 · "cp : 将 c 寄存器中在内容粘贴出来。  
常见用途：将想要持久保存的文本放到特定寄存器里，随时进行粘贴，避免被覆盖  
寄存器字符大写：添加（append）而非覆盖  

宏
宏（Macro）：录制一系列键盘操作，并允许我们重放这些操作
操作序列存储在指定的寄存器中
 · q{register} : 开始录制宏，存在寄存器 register 中
 · 录制过程中按 q 退出录制
 · @{register} : 重放寄存器 register 中的操作
 · @@ : 重放上一次宏操作
常见用法：q{register} 录制一段操作，@{register} 重放，然后一直 @@ 重放
注意： . 命令对宏不生效， . 命令只记录上一次修改，而宏可能包含多次修改

建议：让你的宏对连续重放友好
 1 让你的光标移动更加 general
 2 假设你要在多个特定的位置朝廷特定的操作，一个好的习惯是在宏录制的最后，跳转到时一个需要编辑的位置
   即，宏包括【编辑动作】 * 【跳转到时一个需要编辑的位置】
   这位一来，连续重放就可以实现对所有需要编辑的位置进行编辑操作

通过大写的寄存器，在宏的后面添加命令
如果宏是重放友好的，允许我们使用 [count]@{register} 直接重放 count 次

命令模式
Ex 命令格式
:[range] {excommand} [args]
 · range：作用的范围，不给的话默认是本行
 · excommand ：特殊的命令，适用于 Command 模式
 · args ：后续的参数，视命令而定
 一些 Ex Command ([x] 为寄存器，是可选项) :
 · :[range] delete [x] : 删除 range 中的行（到寄存器 x），delete 可简写为 d
 · :[range] yank [x] : 复制 range 中在行（到寄存器 x）， yank 可简写为 y
 · :[range] print : 将 range 中行打印出来，print 可简写为 p

range 与 address：指定范围
range 由一个或两个 address 构成，即 {address} 或 {address},{address}
address 可以是：
 · {lineno} : 行号，如 3 代表第三行（0 代表第一行上面的虚拟行）
 · $ : 最后一行
 · . : 光标所在行
 · /{pattern}/ : 下一个 pattern 所在的行
address 可以做加减法， .+3 代表光标往下第三行，$-3 代表倒数第4行

address 组合成 range
address 组合成 range (可以混用）：  例子：
 · 1,3 : 文件的1-3行   · 1，3 delete ：删除1~3行
 · ., .+4 : 当前=当前住下4行（共5行） · ., .+4 yank 复制当前-当前住下4行
 · $-3,$: 文件的最后4行   。 $-3,$ print : 打印文件的最后4行
% ： 特殊的 range ，代表当前文件的所有行
'</'> : 可视模式中选中范围的开头和结尾（可视模式下直接按 : 可以直接设置）

行的复制、移动、粘贴
 · :[range] copy {address} : 把 range 中的行复制到 address 后面
 · :[range] move {address} : 把 range 中的行移动到 address 后面
 · :[address] put [x] : 把寄存器 x 的内容粘贴到 address 后面
0 作为虚拟行的 address， 可以用来将内容 插入第一行

批量操作：normal 命令
格式 :[range] normal {commands}
含义：对 range 中的所有行执行 Normal 模式下的命令 commands
 · 将 range 设置为 %，可以对整个文件的所有行执行
 · :[range] normal . : 配合 . 命令，效果拨群
   常用做法：先做一次修改操作，再用 normal 命令在指定的行上完成操作
 · . 命令只能记录一次修改，用宏可以实现记录多个操作
   : [range] normal @{register}
   常用做法：先把想要的操作录制成宏，再用normal 命令在指定的行上重放宏

批量操作： global 命令
格式 : [range] global/{pattern}/[cmd]
含义：对 range 中包含 pattern 的所有行执行 Command 模式下的 Ex 命令
[cmd] : Ex 命令，不给的话默认是打印（print）
注意， normal 命令也是 Ex 命令！
:[range] global/{pattern}/normal {commands} : 对 range 中所有带 pattern 的行，
执行 Normal 模式下的命令 commands
例子：
 · :% global /TODO/delete : 删除所有带 RODO 的行

替换命令
:[range]s/{pattern}/{string}/[flags]
将 pattern 替换为 string
flags :
 · g：替换每一行的所有匹配
 · i：忽视大小写
 · c：替换前进行确认
 · n：计数而不是替换
:%s/Vim/gn : 统计文件中所有 Vim 出现的次数（此时替换为什么无所谓，加了 n 就不会
执行替换操作

分屏
split [filename] : split命令可以上下分屏，也可以指定文件名。
 使用 Ctrl+w 键进行切换 split 命令也可以简化为sp
vsplit [filename] : vsplit 命令可以左右分屏。

Vim 中常见的补全
命令  补全类型
<C-n>  普通关键字
<C-x><C-n> 当前缓冲区关键字
<C-x><C-i> 包含文件关键字
<C-x><C-j> 标签文件关键字
<C-x><C-k> 字典查找
<C-x><C-l> 整行补全
<C-x><C-f> 文件名补全
<C-x><C-o> 全能（Omni）补全

Vim 更换配色
使用 :colorscheme 显示当前的主题配色，默认是 default
用 :colorscheme <Ctrl+d> 可以显示所有的配色
用 :colorscheme 配色名 就可以修改配色
从网络上寻找更好看的配色
搜索vim colorscheme 可以找到很多 vim 配色网站 例如
<https://github.com/flazz/vim-colorschemes>
