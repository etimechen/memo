#Double Commander在Mac OS X下的配置及使用

##将Beyond Compare配置到Differ里

* 打开Beyond Compare安装命令行工具,点击 `Beyond Compare` - `Install Command Line Tools...`
* 打开Double Commander点击 `Double Commander` - `Preferences...` - `Tools` - `Editor` - `Differ` ,将 `Use external program` 打勾
* 在`Path to program to execute` 里输入 `/usr/local/bin/bcompare`

![图片1][img1]

* 点击 `Toolbar` - `Insert new button` , 选择 `for an internal command` - `as last element`

![图片2][img2]

* 在Command菜单里选择 `cm_CompareContents` ,Icon,Tooltip会自动生成
* 在Parameters里输入 `%pl %pr`
* 点击 `Apply` - `OK`
* 在Double Commander里选择两个文件,点击工具栏上新增的按钮图标测试效果
* 如果想用快捷键调用此功能,点击 `Double Commander` - `Preferences...` - `Keys` - `Hot keys`
* 找到 `cm_CompareContents`,点击下面的 `Add hotkey`,按键盘上你想定义的快捷键,我设置的是<kbd>⌘</kbd>+<kbd>shift</kbd>+<kbd>M</kbd>,仅作参考

![图片3][img3]

##在工具栏上设定常用目录

>我们有时候需要将一些常用的目录放置到工具栏,以方便点击访问,像Finder左侧边栏一样.虽然这个功能可以以"Directory Hotlist"达到目的,但是想将这些常用目录以图标的形式放置到工具栏上也可以很好的实现.

* 打开Double Commander点击 `Double Commander` - `Preferences...`
* 点击 `Toolbar` - `Insert new button` , 选择 `for an internal command` - `as last element`
* 在Command菜单里选择 `cm_CompareContents` ,Icon,Tooltip会自动生成
* 在Parameters里输入 `/Users/`

![图片4][img4]

* 点击 `Apply` - `OK`
* 在Double Commander里点击工具栏上新增的按钮图标测试效果,应该会跳转到/Users/目录下



[img1]: file:///Volumes/Media%20Data/百度云同步盘/MarkDown/images/241CDF2F-4D9B-4ACC-837E-7707BC472AAB.png
[img2]: file:///Volumes/Media%20Data/百度云同步盘/MarkDown/images/EDA033CA-653C-40C1-B4CC-4D0579A0334B.png
[img3]: file:///Volumes/Media%20Data/百度云同步盘/MarkDown/images/91AC473C-D454-4D47-AA54-6ECE5070F15A.png
[img4]: file:///Volumes/Media%20Data/百度云同步盘/MarkDown/images/51141458-11FD-428E-BC4C-6D4BF78B9FBF.png
    