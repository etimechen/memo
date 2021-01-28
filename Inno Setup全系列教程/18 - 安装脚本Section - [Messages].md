# 安装脚本Section

## [Messages] section

[Messages]section用于定义安装程序和卸载程序显示的消息。通常，您无需在脚本文件中创建[Messages]section，因为默认情况下，所有消息都是从Inno Setup随附的Default.isl文件（或[Languages]section条目指定的任何文件）中提取的。

但是，可以通过在脚本文件中创建[Messages]section来覆盖特定的消息。为此，首先，您需要知道要更改的消息的ID。可以通过搜索Default.isl轻松找到。例如，假设您想将向导上的“＆Next>”按钮更改为“＆Forward>”。该消息的ID为“ ButtonNext”，因此您将创建一个[Messages]section，如下所示：

```
[Messages]
ButtonNext=&Forward >
```

某些消息采用诸如％1和％2之类的参数。您可以重新排列参数的顺序（即，将％2移到％1之前），还可以根据需要复制参数（例如，“％1 ...％1％2”）。在带有参数的消息上，使用两个连续的“％”字符嵌入单个“％”。 “％n”创建换行符。如果你希望将Inno Setup的所有文本翻译成另一种语言，而不是修改Default.isl或覆盖你创建的每个脚本中的每条消息，请复制Default.isl的副本，并使用另一个名称，例如MyTranslation.isl。在您希望使用MyTranslation.isl的任何安装中，创建指向该文件的[Languages]section条目。

如果存在多个[Languages]section条目，则默认情况下，在脚本中指定[Messages]section条目（与.isl文件相对）将覆盖所有语言的该消息。要将[Messages]section条目仅应用于一种语言，请在其前面加上语言的内部名称并加上句点。例如：

```
en.ButtonNext=&Forward >
```

### 特殊用途的消息

BeveledLabel消息可用于指定一行文本，该文本行显示在向导窗口和卸载程序窗口的左下角。以下是一个示例：

```
[Messages]
BeveledLabel=Inno Setup
```

当在命令行中传递/HELP时，可以使用HelpTextNote消息指定一行或多行文本，这些文本将添加到显示的摘要中的参数列表中。以下是一个示例：

```
[Messages]
HelpTextNote=/PORTABLE=1%nEnable portable mode.
```

这些特殊用途的消息默认为空字符串，因此，如果要使用这些消息，请确保为主脚本中的所有语言提供非空的默认值。