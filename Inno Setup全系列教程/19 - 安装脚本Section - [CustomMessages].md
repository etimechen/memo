# 安装脚本Section

## [CustomMessages] section

[CustomMessages]section用于定义{cm:...}常量的自定义消息值。有关更多信息，请参见常量文档。使用{cm:...}常量从[CustomMessages]section中获取描述的任务示例：

```
[CustomMessages]
CreateDesktopIcon=Create a &desktop icon

[Tasks]
Name: desktopicon; Description: "{cm:CreateDesktopIcon}"
```

消息可以接受从%1到%9的参数。您可以重新排列参数的顺序(例如，将%2移动到%1之前)，如果需要，还可以复制参数(例如，复制参数)。“%1…%1%2”)。对于带参数的消息，使用两个连续的"%"字符嵌入一个"%"。"%n"创建一个换行符。

在有多个[语言]section条目的情况下，在脚本中指定一个[CustomMessages]section条目(而不是.isl文件)将默认覆盖所有语言的该消息。若要仅对一种语言应用[CustomMessages]section条目，请在其前面加上该语言的内部名称和句点。例如：

```
nl.CreateDesktopIcon=Maak een snelkoppeling op het &bureaublad
```

当前，Inno Setup随附的所有语言的.isl文件都为每种语言定义并翻译了以下自定义消息（此处显示了它们的英语值）：

```
NameAndVersion=%1 version %2
AdditionalIcons=Additional icons:
CreateDesktopIcon=Create a &desktop icon
CreateQuickLaunchIcon=Create a &Quick Launch icon
ProgramOnTheWeb=%1 on the Web
UninstallProgram=Uninstall %1
LaunchProgram=Launch %1
AssocFileExtension=&Associate %1 with the %2 file extension
AssocingFileExtension=Associating %1 with the %2 file extension...
AutoStartProgramGroupDescription=Startup:
AutoStartProgram=Automatically start %1
AddonHostProgramNotFound=%1 could not be located in the folder you selected.%n%nDo you want to continue anyway?
```

你可以在自己的脚本中使用这些预定义的自定义消息。使用UninstallProgram的示例：

```
[Icons]
Name: "{group}\{cm:UninstallProgram,My Program}"; Filename: "{uninstallexe}"
```