# 安装脚本Section

## [Icons] section

此可选section定义安装程序要在“开始”菜单和/或其他位置（例如桌面）中创建的所有快捷方式。

这是[Icons]部分的示例：

```
[Icons]
Name: "{group}\My Program"; Filename: "{app}\MYPROG.EXE"; WorkingDir: "{app}"
Name: "{group}\Uninstall My Program"; Filename: "{uninstallexe}"
```

以下是受支持的参数的列表：

### Name（必填）

要创建的快捷方式的名称和位置。此参数中可以使用任何Shell文件夹常量或目录常量。

请记住，快捷方式存储为文本文件，因此此处不能使用普通文件名中不允许的任何字符。另外，由于不可能有两个具有相同名称的文件，因此也就不可能有两个具有相同名称的快捷方式。

举例：

```
[Dirs]
Name: "{group}\My Program"
Name: "{group}\Subfolder\My Program"
Name: "{commondesktop}\My Program"
Name: "{commonprograms}\My Program"
Name: "{commonstartup}\My Program"
```

### Filename（必填）

快捷方式的命令行文件名，通常以目录常量开头。除了文件和文件夹名称之外，还可以指定URL（网站地址）。指定URL后，安装程序将创建一个“Internet快捷方式”（.url）文件，并忽略Parameters，WorkingDir，HotKey和Comment参数。在64位Windows上，请注意，当由64位进程（例如Windows资源管理器）启动快​​捷方式时，{sys}常量将映射到本机64位System目录。无论安装是否以64位安装模式运行，都是如此。若要创建始终指向32位系统目录的快捷方式，请改用{syswow64}。 （这同样适用于WorkingDir和IconFilename参数。）

举例：

```
Filename: "{app}\MYPROG.EXE"
Filename: "{uninstallexe}"
Filename: "{app}\FolderName"
Filename: "http://www.example.com/"
```

### Parameters

快捷方式的可选命令行参数，可以包含常量。

举例：

```
Parameters: "/play filename.mid"
```

### WorkingDir

快捷方式的工作目录（或“开始于”），它指定程序的初始当前目录。此参数可以包含常量。

如果未指定此参数或该参数为空，则安装程序将尝试从Filename参数中提取目录名称。如果失败（不太可能），则工作目录将设置为{sys}。

举例：

```
WorkingDir: "{app}"
```

### HotKey

快捷方式的热键（或“快捷键”）设置，是可以启动程序的按键组合。

注意：如果您更改快捷键并重新安装该应用程序，则Windows可能会继续识别旧的快捷键，直到您注销并重新登录或重新启动系统。

举例：

```
HotKey: "ctrl+alt+k"
```

### Comment

指定快捷方式的“注释”（或“描述”）字段，该字段确定其弹出提示。此参数可以包含常量。

举例：

```
Comment: "This is my program"
```

### IconFilename

要显示的自定义图标的文件名（位于用户系统上）。这可以是包含图标或.ico文件的可执行映像（.exe，.dll）。如果未指定此参数或该参数为空白，则Windows将使用文件的默认图标。此参数可以包含常量。

举例：

```
IconFilename: "{app}\myicon.ico"
```

注意：当安装程序在64位Windows上运行时，它将自动将文件名中的{commonpf32} \值替换为'％ProgramFiles（x86）％\'，以解决64位Windows中的错误：64位Windows将其替换为“％ProgramFiles％\”，这是不正确的。

### IconIndex

IconFilename指定的文件中使用的图标的从零开始的索引。预设为0。如果IconIndex为非零，并且未指定IconFilename或为空白，则其行为就和IconFilename与Filename相同。

举例：

```
IconIndex: 0
```

### AppUserModelID

指定快捷方式的Windows 7（或更高版本）应用程序用户模型ID。在早期Windows版本上被忽略。此参数可以包含常量。

举例：

```
AppUserModelID: "MyCompany.MyProg"
```

### Flags

此参数是一组附加选项。通过用空格分隔多个选项。支持以下选项：

#### closeonexit

设置此标志后，安装程序将设置快捷方式的“退出时关闭”属性。仅当快捷方式指向MS-DOS应用程序时，此标志才有效（具体来说，如果扩展名为.pif）。如果既未指定此标志也未指定dontcloseonexit标志，则安装程序将不会尝试更改“退出时关闭”属性。

#### createonlyiffileexists

设置此标志时，只有在Filename参数指定的文件存在的情况下，安装程序才会尝试创建图标。

#### dontcloseonexit

与closeonexit相同，除了它会导致安装程序取消选中“退出时关闭”属性。

#### excludefromshowinnewinstall

防止新建快捷方式的“开始”菜单项在Windows 7上突出显示，另外还防止新建快捷方式自动固定在Windows 8（或更高版本）上的“开始”屏幕。在早期Windows版本上被忽略。

#### foldershortcut

创建一种特殊的快捷方式，称为“文件夹快捷方式”。通常，当“开始”菜单上存在文件夹的快捷方式时，单击该项会导致打开一个单独的“资源管理器”窗口，显示目标文件夹的内容。相反，“文件夹快捷方式”会将目标文件夹的内容显示为子菜单，而不是打开单独的窗口。

在Windows 7（或更高版本）上运行时，当前忽略此标志，因为文件夹快捷方式在“开始”菜单上不再正确展开。不知道这是Windows 7中的错误还是已删除的功能。

使用此标志时，必须在Filename参数中指定文件夹名称。指定文件名将导致无法使用的快捷方式。

#### preventpinning

防止“开始”菜单项可固定到Windows 7（或更高版本）上的任务栏或“开始”菜单。这也使该条目不适合包含在“开始”菜单的“最常用”（MFU）列表中。在早期Windows版本上被忽略。

#### runmaximized

设置此标志后，安装程序会将图标的“运行”设置设置为“最大化”，以便在启动程序时首先将其最大化。

#### runminimized

设置此标志后，安装程序会将图标的“运行”设置设置为“最小化”，以便在启动程序时首先将其最小化。

#### uninsneveruninstall

指示卸载程序不要删除该图标。

#### useapppaths

设置此标志后，在Filename参数中仅指定文件名（无路径），安装程序将从“HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\App Paths”注册表项中检索路径名，并将其自动添加到文件名中。

举例：

```
Flags: runminimized
```


