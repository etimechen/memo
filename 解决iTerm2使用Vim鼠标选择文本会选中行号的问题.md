# 解决iTerm2使用Vim鼠标选择文本会选中行号的问题

## Vim开启鼠标选择功能的方法

在 `.vimrc` 文件中插入 `set mouse=a` 保存退出再进入Vim后即可用鼠标选择文本且不会选中行号。

但是，当你按下 <kbd>⌘</kbd>+<kbd>C</kbd> 复制时iTerm2会弹出一个框，如果你选择了，鼠标选择就不再生效，所有选择的文本都会附带行号。

如下图所示：

![图片1][img1]

此时需要在`iTerm2` - `Preferences` - `Profiles` - `Terminal` 里将 `Enable mouse reporting` 打上勾就可以了

![图片2][img2]

## 解决Vim和系统之间实现剪贴板共享

在 `.vimrc` 文件中插入 `set clipboard=unnamed` 保存退出再进入Vim后即可。Vim里选择文本后按<kbd>y</kbd>键复制

[img1]: https://github.com/etimechen/memo/blob/master/images/jjisyvsbxzwbhxzhhdwt1.jpg
[img2]: https://github.com/etimechen/memo/blob/master/images/jjisyvsbxzwbhxzhhdwt2.jpg