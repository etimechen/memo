#使用Mac OS X自带的Vim安装使用Command-T插件
>[Command-T](https://github.com/wincent/command-t)是Vim里一款快速定位文件的实用插件,但在自带的vim里使用时会提示找不到对应的ruby版本.我们需要先安装好Vim插件管理工具[Vundle](https://github.com/VundleVim/Vundle.vim)和OS X软件管理工具[HomeBrew](https://github.com/Homebrew/homebrew)后按以下指引操作

_先感受一下Command-T_

![Command-T](https://raw.githubusercontent.com/wincent/command-t/media/command-t.gif)

* `~/.vimrc` 里添加Command-T安装源 `Plugin 'git://git.wincent.com/command-t.git'`,保存,退出
* `vim`,打开Vim编辑器,输入`:BundleInstall` 回车,待Command-T插件安装完成后退出
* `vim`,打开Vim编辑器,输入`:CommandT` 回车,会提示所需rubyXXX版,当前版本unknown等信息,记住它需要的版本号,我这里显示的是2.0.0p645版
* `brew install ruby-install`,安装ruby安装器.不可以直接安装ruby,因为直接安装ruby为最新版,与Vim对应所需的版本不匹配将无法实用Command-T
* `ruby-install -M https://cache.ruby-lang.org/pub/ruby ruby-2.0.0-p645`,安装错误提示所要求的ruby2.0.0p645版
* `ruby -v`查看版本是否正确
* 进入Command-T安装目录,执行`rake make`
* `vim`,打开Vim编辑器,输入`:CommandT` 回车,应该就可以使用了