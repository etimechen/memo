# Vim使用

## Vim调试，输出log文件

`vim -V20 2>&1 | tee logfile`

## Normal
_移动光标到行首_ `0`
_普通模式下当前行下插入新的行并进入插入模式_  `o`
_从光标处删除至行尾部_ `d$` 
_撤销_ `u`
_撤销撤销_ `Ctrl+r`
_删除光标所在的整个单词_ `daw`
_移动光标到单词首部_ `b`
_移动光标到单词尾部_ `e`


## vim-devicons插件图标带有方括号的问题解决

查看 `echo g:webdevicons_conceal_nerdtree_brackets` 结果是否为1

在终端输入命令检查您的vim是否具有conceal（应为+conceal）
`vim --version | grep conceal`

检查conceallevel（应为3）
`set conceallevel = 3`

## Vim将内容复制到剪贴板

1. 按v选取vim内容
2. ` ”+y `

##Vim将外部内容粘贴到vim里

` "+p ` 粘贴
` "+gp ` 粘贴并且移动光标到粘贴内容后