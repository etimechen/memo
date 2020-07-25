# 安装脚本Section

## [Run] 和 [UninstallRun] section

[Run]section是可选的，它指定在程序成功安装之后，但在安装程序显示最终对话框之前执行的任意数量的程序。[UninstallRun]section也是可选的，它指定在卸载的第一步执行任意数量的程序。除了下面另有说明外，这两个section有相同的语法。

程序按照它们在脚本中出现的顺序执行。默认情况下，当处理一个[Run]/[UninstallRun]条目时，Setup/Uninstall将等待，直到程序终止后再继续下一个，除非使用nowait、shellexec或waituntilidle标志。

注意，默认情况下，如果在[Run]section中执行的程序队列文件将在下一次重新启动时被替换(通过调用MoveFileEx或修改windows .ini)，安装程序将检测到这一点，并在安装结束时提示用户重新启动计算机。如果你不想这样做，设置RestartIfNeededByRun指令为no。

以下是[Run]部分的示例：

```
[Run]
Filename: "{app}\INIT.EXE"; Parameters: "/x"
Filename: "{app}\README.TXT"; Description: "View the README file"; Flags: postinstall shellexec skipifsilent
Filename: "{app}\MYPROG.EXE"; Description: "Launch application"; Flags: postinstall nowait skipifsilent unchecked
```

以下是受支持的参数的列表：

### Filename（必填）

要执行的程序，或要打开的文件/文件夹。如果Filename不是可执行文件（.exe或.com）或批处理文件（.bat或.cmd），则必须在条目上使用shellexec标志。此参数可以包含常量。

举例：

```
Filename: "{app}\INIT.EXE"
```

### Description

仅在[Run]section有效。条目的描述，可以包括常量。该描述用于带有postinstall标志的条目。如果未为条目指定描述，则安装程序将使用默认描述。此描述取决于条目的类型（普通或shellexec）。

举例：

```
Description: "View the README file"
```

### Parameters

程序的可选命令行参数，可以包括常量。

举例：

```
Parameters: "/x"
```

### WorkingDir

程序的初始当前目录。如果此参数未指定或为空，则使用Filename参数中的目录；如果Filename不包含路径，它将使用默认目录。此参数可以包含常量。

举例：

```
WorkingDir: "{app}"
```

### StatusMsg

仅在[Run]section有效。确定程序执行时向导上显示的消息。如果该参数未指定或为空白，将使用默认消息“完成安装...”。这个参数可以包含常量。

举例：

```
StatusMsg: "Installing BDE..."
```

### RunOnceId

仅在[UninstallRun]section有效。如果同一个应用程序安装了不止一次，那么卸载日志文件中的“run”项将会重复。通过给RunOnceId分配一个字符串，可以确保特定的[UninstallRun]条目在卸载期间只执行一次。例如，如果卸载日志中的两个或多个“run”条目的RunOnceId设置为“DelService”，则只执行RunOnceId设置为“DelService”的最新条目;其余的将被忽略。注意RunOnceId比较是区分大小写的。

举例：

```
RunOnceId: "DelService"
```

### Verb

指定要对文件执行的操作。必须与shellexec标志结合使用。常用的动词包括“open”和“print”。如果该参数未指定或为空，则将使用文件类型的默认谓词(通常为“open”)。

举例：

```
Verb: "print"
```

### Flags

此参数是一组附加选项。通过用空格分隔多个选项。支持以下选项：

#### 32bit
当在Filename和WorkingDir参数中使用时，使{sys}常量映射到32位系统目录。这是32位安装模式下的默认行为。

该标志不能与shellexec标志结合使用。

#### 64bit
在Filename和WorkingDir参数中使用时，使{sys}常量映射到64位系统目录。这是64位安装模式安装中的默认行为。

仅当安装程序在64位Windows上运行时才能使用此标志，否则将发生错误。在同时支持32位和64位体系结构的安装中，可以通过添加Check：IsWin64参数来避免该错误，该参数将导致在32位Windows上运行时以静默方式跳过该条目。

该标志不能与shellexec标志结合使用。

#### hidewizard
如果指定了此标志，则在程序运行时向导将被隐藏。

#### nowait
如果指定了此标志，则在继续下一个[Run]条目或完成安装程序之前，它不会等待进程完成执行。不能与waituntildle或waituntil终止一起使用。

#### postinstall
仅在[运行]section有效。指示安装程序在“安装完成”向导页面上创建一个复选框。用户可以取消选中或选中此复选框，从而选择是否应处理此条目。以前，此标志称为showcheckbox。

如果安装程序必须重新启动用户的计算机（由于安装了带有标志restartreplace的文件，或者AlwaysRestart [Setup]section指令为yes），则该复选框将没有机会显示，因此该条目将永远不会被处理。

[Files]section中的条目的isreadme标志现在已过时。如果编译器检测到带有isreadme标志的条目，它将从[Files]条目中删除isreadme标志，并在[Run]条目列表的开头插入一个生成的[Run]条目。生成的[Run]条目运行README文件，并带有标志shellexec，skipifdoesntexist，postinstall和skipifsilent。

#### runascurrentuser
如果指定了此标志，则生成的进程将继承安装程序/卸载程序的用户凭据（通常是完全管理特权）。

当不使用postinstall标志时，这是默认行为。

该标志不能与runasoriginaluser标志结合使用。

#### runasoriginaluser
仅在[Run]section有效。如果指定了此标志，则将使用最初启动安装程序的用户的凭据（通常是非提升的凭据）（即“UAC之前的对话框”凭据）执行生成的过程。

使用postinstall标志时，这是默认行为。

如果用户通过右键单击其EXE文件并选择“以管理员身份运行”来启动安装程序，那么不幸的是，此标志将无效，因为安装程序没有机会使用原始用户凭据运行任何代码。如果安装程序是从已经提升的进程中启动的，则情况也是如此。但是请注意，这不是Inno Setup的特定限制；在这种情况下，基于Windows Installer的安装程序都无法返回到原始用户凭据。

该标志不能与runascurrentuser标志结合使用。

#### runhidden
如果指定此标志，它将在隐藏窗口中启动程序。在执行可能提示用户输入的程序时，切勿使用此标志。

#### runmaximized
如果指定了此标志，它将在最大化的窗口中启动程序或文档。

#### runminimized
如果指定了此标志，它将在最小化的窗口中启动程序或文档。

#### shellexec
如果Filename不是直接可执行文件（.exe或.com文件），则需要此标志。设置此标志后，“文件名”可以是文件夹，也可以是任何注册的文件类型-包括.chm，.doc等。该文件将使用与用户系统上的文件类型相关联的应用程序打开，就像用户在资源管理器中双击该文件一样。

默认情况下，使用shellexec标志时，它不会等到生成的进程终止。如果需要，您必须添加标志waituntilterminated。请注意，如果没有产生新的进程，则它不能也不会等待-例如，如果Filename指定了一个文件夹。

#### skipifdoesntexist
如果在[Run]section中指定了此标志，则如果文件名不存在，安装程序将不会显示错误消息。

如果在[UninstallRun]section中指定了此标志，则如果Filename不存在，则卸载程序将不会显示“无法删除某些元素”警告。

使用此标志时，文件名必须是绝对路径。

#### skipifnotsilent
仅在[Run]section有效。指示安装程序在安装程序未运行（非常）静默的情况下跳过此条目。

#### skipifsilent
仅在[Run]section有效。如果安装程序正在运行（非常）静默，则指示安装程序跳过此条目。

#### unchecked
仅在[Run]section有效。指示安装程序最初取消选中该复选框。如果他/她希望处理输入，则用户仍可以选中该复选框。如果未同时指定postinstall标志，则忽略此标志。

#### waituntilidle

如果指定了此标志，它将等待直到进程正在等待没有输入挂起的用户输入，而不是等待进程终止。 （这将调用Win32的WaitForInputIdle Win32函数。）不能与nowait或waituntilterminated结合使用。

#### waituntilterminated

如果指定了此标志，它将等待直到进程完全终止。 请注意，除非您使用shellexec标志，否则这是默认行为（即您无需指定此标志），在这种情况下，如果您希望它等待就需要指定此标志。 不能与nowait或waituntildle结合使用。


举例：

```
Flags: postinstall nowait skipifsilent
```
