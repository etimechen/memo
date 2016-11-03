#SublimeText 插件 - PlainTasks使用方法

##怎样使用PlainTasks

##项目

* 项目标题是在任何地方使用分号结束的一行文本(使用分号结束才有颜色,并且用<kbd>⌘</kbd>+<kbd>R</kbd>或在Windows下用<kbd>Ctrl</kbd>+R能通过项目标题快速定位)
* 项目可以嵌套
* 项目可以代码折叠(需要编辑器支持)

##任务

###新增任务:

* <kbd>⌘</kbd>+<kbd>enter</kbd>(Windows用<kbd>Ctrl</kbd>+<kbd>enter</kbd>)新增一个任务;
* <kbd>⌘</kbd>+<kbd>i</kbd>(Windows用<kbd>Ctrl</kbd>+<kbd>i</kbd>)也可以新增一个任务;
* 如果你在一个新行里用PlainTasks创建一个新任务,这个新任务将创建在本行上;
* 如果你在已经存在任务的行里创建新任务,这个任务将加在当前任务的下面;
* 如果你在已经存在文本的行里创建新任务,它将把文本转换为任务.

###完成任务:

* <kbd>⌘</kbd>+<kbd>D</kbd>(Windows用<kbd>Ctrl</kbd>+<kbd>D</kbd>)将光标所在行的任务标记为完成;
* 再按<kbd>⌘</kbd>+<kbd>D</kbd>(Windows用<kbd>Ctrl</kbd>+<kbd>D</kbd>)它将退为未完成;
* <kbd>Ctrl</kbd>+<kbd>C</kbd>(Windows用<kbd>alt</kbd>+<kbd>C</kbd>)将任务标记为取消;
* 同样,再按<kbd>Ctrl</kbd>+<kbd>C</kbd>(Windows用<kbd>alt</kbd>+<kbd>C</kbd>)它将退为未完成;
* 完成的任务不能标记为取消,需要先改为未完成再改成取消;
* 取消的任务可以标记为完成.

###标签：

* 用@符号就可以定义一个标签,例如: @tag

###网址:

* <kbd>⌘</kbd>+<kbd>shift</kbd>+<kbd>U</kbd>(Windows用<kbd>Ctrl</kbd>+<kbd>shift</kbd>+<kbd>U</kbd>)通过浏览器打开当前光标所在的网址

###文件链接:

* 你可以用圆点加斜杠(或者反斜杠)加文件名的形式创建一个文件链接,例如: `.\filename\` 或者 `./another filename/`

    它只支持一行一个文件链接,文件名可以是相对路径或者绝对路径

* 如果跳转到指定链接文件的行号和列数,在文件名后用分号表示,例如: `.\filename:11:8`

    上面例子表示链接到filename文件的第11行第8列

* 在SublimeText 3,你可以用 > 来指定链接文件里的符号,例如: `.\filename>symbol`
* 在SublimeText 2,你可以用双引号来指定链接文件里的文本,例如: `.\filename"any text"`
* <kbd>Ctrl</kbd>+<kbd>O</kbd>(Windows用<kbd>alt</kbd>+<kbd>O</kbd>)在SublimeText里打开链接文件
* SublimeText 3的链接可以指向目录,打开指向目录的链接将会把此目录加入到侧边栏的工程里,例如: `.\..\PlainTasks\`
* 其它创建文件链接的语法格式:

    ```
    [](install.txt)
    [](path ":11:8")
    [](path ">symbol")
    [](path "any text")
    [[..\PlainTasks.py]]
    [[path::11:8]]
    [[path::*symbol]]
    [[path::any text]]
    [[path]] ":11:8"
    [[path]] ">symbol"
    [[path]] "any text"
    ```
    *注:path为文件路径*

###归档:

* <kbd>⌘</kbd>+<kbd>shift</kbd>+<kbd>A</kbd>(Windows用<kbd>Ctrl</kbd>+<kbd>shift</kbd>+<kbd>A</kbd>)归档状态为"完成"的任务.
    它会将所有完成的任务放到文件的底下的"Archive"项目里.归档项目用水平分隔线和其它项目分隔开来.
* <kbd>⌘</kbd>+<kbd>shift</kbd>+<kbd>O</kbd>(Windows用<kbd>Ctrl</kbd>+<kbd>shift</kbd>+<kbd>O</kbd>)将以Org-Mode形式归档.
    它将删除光标后的整个归档列表,并将归档列表加入到单独的归档文件中,例如: filename.TODO → filename_archive.TODO

###创建新的任务文档:

>提示:输入`--`再按<kbd>tab</kbd>键可以生成任务列表的分割线,像这样的: --- ✄ -----------------------

* 打开Command Palette (Mac用<kbd>⌘</kbd>+<kbd>shift</kbd>+<kbd>P</kbd>,Windows用<kbd>Ctrl</kbd>+<kbd>shift</kbd>+<kbd>P</kbd>)
* 输入 `task`,选择 `Tasks: New document`项

###优先级:

* 输入 c, 再按<kbd>tab</kbd>键,它会变成这样 @critical(红色背景)
* 输入 h, 再按<kbd>tab</kbd>键,它会变成这样 @high(橙色背景)
* 输入 l, 再按<kbd>tab</kbd>键,它会变成这样 @low(黑色背景)
* 输入 t, 再按<kbd>tab</kbd>键,它会变成这样 @today(黄色背景)
* c,h,l,t不可与其他字符连续,需要用空格隔开,否则不起作用

###时间跟踪:

* 输入 s, 按两下<kbd>tab</kbd>键,它将生成一个任务开始时间,这个日期时间为当前日期时间;当任务标记为完成或取消时,PlainTasks会计算任务所花时间并显示到归档任务里.
* 输入 tg, 按两下<kbd>tab</kbd>键,它将生成一个任务开关时间,你可以暂停或恢复到任务开始,时间会改为重新开始任务时的时间.首先,你要开始任务,然后通过标记toggle暂停任务,下一次toggle时恢复任务.
* 输入 cr, 按两下<kbd>tab</kbd>键,它将生成一个任务的创建时间,用<kbd>⌘</kbd>+<kbd>shift</kbd>+<kbd>enter</kbd>(Windows用<kbd>Ctrl</kbd>+<kbd>shift</kbd>+<kbd>enter</kbd>)创建一个新任务自动附加创建时间标签
* 输入 d, 按一下<kbd>tab</kbd>键,它将生成一个任务的超期时间@due(),如果你再按一下<kbd>tab</kbd>键,它就插入当前日期时间,和@due( 0)一样的意思.你可以输入短日期,然后按<kbd>tab</kbd>键自动生成默认格式.短日期要是这样的格式: `@due(年-月-日 小时:分钟)` .不能用连续的字符格式,例如: 20160913,但是可以用这种格式: `_年.月.日_`
    - 年,月,分钟,小时可以省略为以下形式:
        - @due(1)           → 下个月的第一天
        - @due(5)           → 本月第五天(如果今天就是第五天就为下个月的第五天)
        - @due(2-3)         → 今年的2月3日(如果今天就是...同上,你懂的)
        - @due(31 23:)      → 当月或下个月第31天23时,分钟为当前时间的分钟,要确保当月有31号才行
        - @due(16.1.1 1:1)  → 2016年1月1日1点1分, @due(16-01-01 01:01)
    - 用一两个特殊符号来表示相对的时间周期,格式: `+[+][number][DdWw][h:m]` ,number 的设置和字母d或字母w一样的,用来表示天或周
        -  @due(+)   → 明天,和 @due( +1) 或者 @due( +1d) 一样
        -  @due(+w)  = @due( +7)
        -  @due(+3w) = @due( +21d)
        -  @due(++)  → 如果任务有 @created(date),那么就根据创建任务的日期加1天,否则就和 @due(+) 一样
        -  @due(+2:)    = @due( +2.)    →  当前的时间加两个小时
        -  @due(+:555)  = @due( +.555)  →  当前的时间加555分钟
        -  @due(+2 12:) = @due( +2 12.) →  当前的日期时间加2天12小时
* <kbd>Ctrl</kbd>+<kbd>space</kbd>(Linux用<kbd>alt</kbd>+<kbd>/</kbd>) 显示标签列表

###文件类型支持:

PlainTasks能自动识别以下文件类型:

* TODO
* *.todo
* *.todolist
* *.taskpaper
* *.tasks

###你可以定制这些:

* 新增和完成任务
* 支持的文件类型列表
* 快捷键
* 任务的日期格式
* 颜色主题
* 外观
* 文件类型的图标
* 其它
想知道更多的定制方法,请访问[https://github.com/aziz/PlainTasks](https://github.com/aziz/PlainTasks)查看Readme文件或者查看Sublime里PlainTasks安装目录下的Readme.md

###有用的编辑器工具:

* 用 <kbd>⌘</kbd>+<kbd>control</kbd>+<kbd>▲</kbd>/<kbd>▼</kbd> (Windows用<kbd>Ctrl</kbd>+<kbd>shift</kbd>+<kbd>▲</kbd>/<kbd>▼</kbd>)上下移动任务.
* 用 <kbd>⌘</kbd>+R (Windows用<kbd>Ctrl</kbd>+<kbd>R</kbd>)查看项目列表并可在项目之间快速跳转
* F6 开启或关闭拼写检查


















    