# 安装脚本Section

## [Components] section

此section是可选的。它定义了安装程序将在向导的“选择组件”页面上显示的所有组件，用于自定义安装程序类型。组件本身不执行任何操作：需要“链接”到其他安装条目。请参阅组件和任务参数。

[Components]section的示例：

```
[Components]
Name: "main"; Description: "Main Files"; Types: full compact custom; Flags: fixed
Name: "help"; Description: "Help Files"; Types: full
Name: "help\english"; Description: "English"; Types: full
Name: "help\dutch"; Description: "Dutch"; Types: full
```

上面的示例生成四个组件：如果用户选择名称为“full”或“compact”的类型，则将安装“主要”组件，只有用户选择“full”时才安装“帮助”组件。

以下是受支持的参数的列表：

### Name（必填）

组件的内部名称。

组件名称中的\或/字符称为组件级别。级别为1或更高的任何组件都是子组件。在子组件之前列出的，比子组件少1的组件是父组件。具有与子组件相同的父组件的其它组件是同级组件。

如果未选择父组件，则不能选择子组件。如果未选择任何子组件，则父组件为未选中状态，除非Components参数直接引用父组件，或者父组件包含checkablealone标志。

如果同级组件具有exclusive标志，只能单选。

举例：

```
Name: "help"
```

### Description（必填）

组件的描述，可以包括常量。此描述在安装过程中显示给最终用户。

举例：

```
Description: "Help Files"
```

### Types

用空格分隔的列表，此组件所属的类型。如果用户从该列表中选择一种类型，则将安装此组件。

如果不使用fixed标志（请参见下文），则安装程序将忽略此列表中的任何自定义类型（使用iscustom标志的类型）。

举例：

```
Types: full compact
```

### ExtraDiskSpaceRequired

该组件所需的额外磁盘空间，类似于[Setup]部分的ExtraDiskSpaceRequired指令。

举例：

```
ExtraDiskSpaceRequired: 0
```

### Flags

此参数是一组附加选项。通过用空格分隔多个选项。支持以下选项：

#### checkablealone

指定当没有子组件时可以检查组件。默认情况下，如果没有Components参数直接引用该组件，则取消选中所有组件的子组件将导致取消选中该组件。

#### dontinheritcheck

指定当组件的父组件被选中时，该组件不应自动变为选中状态。对顶级组件没有影响，并且不能与exclusive标志结合使用。

#### exclusive

指示安装程序此组件与也具有Exclusive标志的同级组件互斥。

#### fixed

指示安装程序最终用户无法在安装过程中手动选择或取消选择此组件。

#### restart

指示安装程序询问用户是否需要重启系统（例如，由于带有重新启动标志的[Files]section条目），都要求用户重新启动系统。像AlwaysRestart一样，但是按组件执行。

#### disablenouninstallwarning

指示安装程序不要警告用户该组件已经安装在他/她的计算机上后，取消选择该组件后不会将其卸载。根据组件的复杂程度，您可以尝试使用[InstallDelete]section和此标志来自动“卸载”取消选择的组件。

举例：

```
Flags: fixed
```