# Mac OS X 无法开机启动印象笔记助手的解决办法

>**问题描述**:在系统偏好设置-用户与群组里设置登录项只能找到/Applications/目录下的应用,而印象笔记助手的路径在/Applications/Evernote.app/Contents/Library/LoginItems/目录下,导致无法添加到登录项中

## 解决办法

打开`系统偏好设置` `用户与群组` `登录项`,点击加号后按<kbd>⌘</kbd>+<kbd>shift</kbd>+<kbd>G</kbd>,将路径 `/Applications/Evernote.app/Contents/Library/LoginItems/` 粘贴上去回车,便能选择 `EvernoteHelper.app` 图标

# Homebrew 启动和停止 nginx 服务

端口: 8081

` brew services start nginx `

` brew services stop nginx `

# 查看占用端口对应的进程

`sudo lsof -n -P |grep :8081`

# 终端常用快捷键

* 移至行首 - `Ctrl + a`
* 移至行尾 - `Ctrl + e`
* 删除光标左边的一行 - `Ctrl + u`

