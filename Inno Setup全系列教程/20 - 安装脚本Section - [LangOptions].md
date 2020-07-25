# 安装脚本Section

## [LangOptions] section

[LangOptions]section用于定义安装程序和卸载程序使用的特定于语言的设置，例如字体。通常，你不需要在脚本文件中创建[LangOptions]section，因为默认情况下，特定于语言的设置是从文件默认值提取的。包含在Inno设置中的isl(或由[Languages]项指定的任何文件)。

以下是[LangOptions]section的示例。（下面列出的设置是默认设置。）

```
[LangOptions]
LanguageName=English
LanguageID=$0409
LanguageCodePage=0
DialogFontName=
DialogFontSize=8
WelcomeFontName=Verdana
WelcomeFontSize=12
TitleFontName=Arial
TitleFontSize=29
CopyrightFontName=Arial
CopyrightFontSize=8
RightToLeft=no
```

### LanguageName

LanguageName是语言的本地名称（而不是英语名称）。在多语言安装中，它显示在“选择语言”对话框的可用语言列表中。

### LanguageID

LanguageID是语言的数字“语言标识符”。请参阅MSDN上的有效语言标识符列表。它与LanguageCodePage一起用于自动检测默认情况下要使用的最合适的语言的目的，因此请确保已正确设置。它应该始终以“ $”符号开头，因为语言标识符是十六进制的。如果当前不存在该语言的语言标识符，请将其设置为零。

### LanguageCodePage

LanguageCodePage指定编译器用来将语言文件中的任何ASCII文本转换为Unicode文本的“代码页”（字符集）。请注意，.iss文件中的任何文本（例如，该语言的[CustomMessages]section）都不会转换，并且应该已经采用Unicode。

如果当前不存在该语言的代码页，请将LanguageCodePage设置为零，并且在语言文件中仅使用Unicode文本（UTF-8）。

如果将LanguageCodePage设置为零，但在其中一种语言的文件中使用了ASCII文本，则系统代码页将用于将文件中的文本转换为Unicode。

### DialogFontName 和 DialogFontSize

DialogFontName和DialogFontSize指定在对话框中使用的字体名称和字体大小。如果没有DialogFontName设置，则将DefaultDialogFontName [Setup]section指令的值用作字体名称。如果指定的字体名称在用户系统上不存在或为空字符串，则将使用8-point Microsoft Sans Serif或MS Sans Serif字体替换。

### WelcomeFontName 和 WelcomeFontSize

WelcomeFontName和WelcomeFontSize在“欢迎使用和安装完成”向导页面的顶部指定要使用的字体名称和字体大小。如果指定的字体名称在用户系统上不存在或为空字符串，则将使用12-point Microsoft Sans Serif或MS Sans Serif字体替换。

### TitleFontName 和 TitleFontSize

TitleFontName和TitleFontSize指定在背景窗口上显示应用程序名称时要使用的字体名称和字体大小（仅在WindowVisible = yes时可见）。如果用户系统上不存在指定的字体名称，则将使用29-point Arial字体替换。

### CopyrightFontName 和 CopyrightFontSize

CopyrightFontName和CopyrightFontSize指定在背景窗口上显示AppCopyright消息时要使用的字体名称和字体大小（仅在WindowVisible = yes时可见）。如果用户系统上不存在指定的字体名称，则将使用8-point Arial字体替换。

### RightToLeft

RightToLeft指定是否从右向左书写语言。如果设置为yes，则文本的对齐方式和阅读顺序将颠倒（某些故意的例外），并且控件将从右到左排列（“翻转”）。

如果存在多个[Languages]section条目，则默认情况下，在脚本中指定[LangOptions]section指令（与.isl文件相对）将覆盖所有语言的该指令。

```
en.LanguageName=English
```