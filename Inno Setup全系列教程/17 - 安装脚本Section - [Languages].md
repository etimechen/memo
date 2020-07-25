# 安装脚本Section

## [Languages] section

Inno Setup支持多语言安装。[Languages]section定义了可用于安装程序的语言。

安装程序按以下顺序确定用于其消息的默认语言：

1. 它搜索其LanguageID设置(通常在该语言的.isl文件的[LangOptions]section中指定)与当前用户的UI语言或地区的主语言标识符和子语言标识符(取决于LanguageDetectionMethod的设置)匹配的语言。
2. 如果找不到匹配项，它将仅搜索主要语言标识符匹配项。如果两种或多种可用语言具有相同的主要语言标识符，它将选择[Languages]section中列出的第一种。
3. 例外：如果用户的UI语言或语言环境（取决于LanguageDetectionMethod的设置）为繁体中文，则在此步骤中不考虑简体中文，反之亦然。
4. 如果找不到匹配项，则默认为[Languages]section中指定的第一语言。

如果ShowLanguageDialog [Setup]section指令被设置为yes(默认)，一个选择语言对话框将显示给用户来设置选择的语言。有关详细信息，请参阅[LangOptions]section帮助主题。

以下是[Languages]section的示例。它定义了两种语言：基于标准Default.isl文件的英语和基于第三方翻译的荷兰语。

```
[Languages]
Name: "en"; MessagesFile: "compiler:Default.isl"
Name: "nl"; MessagesFile: "compiler:Languages\Dutch.isl"
```

### Name（必填）

语言的内部名称，您可以将其设置为任何喜欢的名称。可以将其用作[LangOptions]或[Messages]section条目的前缀，以使这些条目仅适用于一种语言。{language}常量返回所选语言的内部名称。

举例：

```
Name: "en"
```

### MessagesFile（必填）

指定要从中读取默认消息的.isl文件的名称。运行安装程序编译器时，文件必须位于安装的源目录中，除非指定了完全限定的路径名​​或路径名以“compiler:”为前缀，否则，它将在Compiler目录中查找文件。

每个消息文件可能包含一个[LangOptions]section，一个[Messages]section和一个[CustomMessages]section。

当指定了多个文件时，将按照指定的顺序读取它们，因此最后一个消息文件将覆盖任何语言选项或先前文件中的消息。主脚本中的任何语言选项或消息都会覆盖消息文件中的语言选项或消息。

举例：

```
MessagesFile: "compiler:Dutch.isl"
MessagesFile: "compiler:Default.isl,compiler:MyMessages.isl"
```

### LicenseFile

指定.txt或.rtf（富文本）格式的可选许可协议文件的名称，该文件在用户选择程序的目标目录之前显示。在运行安装编译器时，该文件必须位于安装的源目录中，除非指定了完全限定的路径名或路径名前面加上“Compiler:”，在这种情况下，它将在Compiler目录中查找该文件。

举例：

```
LicenseFile: "license-Dutch.txt"
```

### InfoBeforeFile

指定.txt或.rtf（富文本）格式的可选“readme”文件的名称，该文件在用户选择程序的目标目录之前显示。在运行安装编译器时，该文件必须位于安装的源目录中，除非指定了完全限定的路径名或路径名前面加上“Compiler:”，在这种情况下，它将在Compiler目录中查找该文件。

举例：

```
InfoBeforeFile: "infobefore-Dutch.txt"
```

### InfoAfterFile

指定.txt或.rtf（富文本）格式的可选“readme”文件的名称，该文件在成功安装后显示。在运行安装编译器时，该文件必须位于安装的源目录中，除非指定了完全限定的路径名或路径名前面加上“Compiler:”，在这种情况下，它将在Compiler目录中查找该文件。

这与isreadme文件的不同之处在于，此文本显示为向导的页面，而不是显示在单独的记事本窗口中。

举例：

```
InfoAfterFile: "infoafter-Dutch.txt"
```
