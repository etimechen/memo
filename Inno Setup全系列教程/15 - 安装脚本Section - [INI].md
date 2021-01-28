# 安装脚本Section

## [INI] section

此可选项定义了你希望安装程序在用户系统上设置的所有.INI文件条目。

这是[INI]部分的示例：

举例：

```
[INI]
Filename: "MyProg.ini"; Section: "InstallSettings"; Flags: uninsdeletesection
Filename: "MyProg.ini"; Section: "InstallSettings"; Key: "InstallPath"; String: "{app}"
```

以下是受支持的参数的列表：

### Filename（必填）

你要安装程序修改的.INI文件的名称，其中可以包含常量。如果此参数不包含路径，它将写入Windows目录。如果此参数为空，它将写入Windows目录中的WIN.INI。

举例：

```
Filename: "{app}\MyProg.ini"
```

### Section（必填）

在其中创建条目的section的名称，其中可以包括常量。

举例：

```
Section: "Settings"
```

### Key

要设置的键的名称，可以包含常量。如果此参数未指定或为空，则不会创建任何密钥。

举例：

```
Key: "Version"
```

### String

分配给键的值，可以使用常量。如果未指定此参数，则不会创建任何密钥。

举例：

```
String: "1.0"
```

### Flags

此参数是一组附加选项。通过用空格分隔多个选项。支持以下选项：

#### createkeyifdoesntexist

仅当文件中不存在密钥时，才分配给密钥。如果未指定此标志，则将设置密钥，而不管其是否已经存在。

#### uninsdeleteentry

程序卸载后删除条目。可以将其与uninsdeletesectionifempty标志结合使用。

#### uninsdeletesection

卸载程序后，删除条目所在的整个部分。在Windows本身使用的部分（例如WIN.INI中的某些部分）上使用它显然不是一个好主意。你只应在应用程序专用的部分上使用它。

#### uninsdeletesectionifempty

与uninsdeletesection相同，但是仅当其中没有键时才删除该部分。可以将其与unsindeleteentry标志结合使用。

举例：

```
Flags: uninsdeleteentry
```

