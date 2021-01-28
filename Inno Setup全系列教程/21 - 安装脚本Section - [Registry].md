# 安装脚本Section

## [Registry] section

此可选项定义了你希望安装程序在用户系统上创建，修改或删除的所有注册表项/值。

默认情况下，安装程序创建的注册表项和值在卸载时不会删除。如果要卸载程序删除项或值，则必须包括以下所述的uninsdelete*标志之一。

以下是[Registry]部分的示例：

```
[Registry]
Root: HKLM; Subkey: "Software\My Company"; Flags: uninsdeletekeyifempty
Root: HKLM; Subkey: "Software\My Company\My Program"; Flags: uninsdeletekey
Root: HKLM; Subkey: "Software\My Company\My Program\Settings"; ValueType: string; ValueName: "InstallPath"; ValueData: "{app}"
```

以下是受支持的参数的列表：

### Root（必填）

根项。这必须是下列值之一：

```
HKCU    (HKEY_CURRENT_USER)
HKLM    (HKEY_LOCAL_MACHINE)
HKCR    (HKEY_CLASSES_ROOT)
HKU     (HKEY_USERS)
HKCC    (HKEY_CURRENT_CONFIG)
```

另外，允许使用一个特殊值：

HKA（在“管理员安装模式”下等同于HKLM，在HKCU上则等于HKCU）

HKCU和HKA仅应用于与配置文件兼容的设置。

建议不要使用HKCR，而应将Subkey参数设置为“ Software \ Classes”使用HKA。

值（包括HKA）后缀可以为32或64。后缀为32的根项值（例如HKLM32）映射到注册表的32位视图；默认值为0。后缀为64的根项值（例如HKLM64）映射到注册表的64位视图。后缀为64的根项值只能在安装程序在64位Windows上运行时使用，否则将发生错误。在同时支持32位和64位体系结构的安装上，可以通过添加Check:IsWin64参数来避免错误，该参数将导致在32位Windows上运行时静默地跳过该条目。

没有后缀的根项值(例如，HKLM)等于后缀为32的值(例如，HKLM32)，除非安装在64位安装模式下运行，在这种情况下，它等于后缀为64的值(例如，HKLM64)。

举例：

```
Root: HKLM
```

### Subkey（必填）

子项名称，可以包含常量。

举例：

```
Subkey: "Software\My Company\My Program"
```

### ValueType

值的数据类型。必须是以下之一：

```
    none
    string
    expandsz
    multisz
    dword
    qword
    binary
```

如果none（默认设置），则安装程序将创建项，但不创建值。在这种情况下，将忽略ValueData参数。

如果string，则安装程序将创建一个字符串（REG_SZ）值。

如果expandsz，安装程序将创建一个扩展字符串（REG_EXPAND_SZ）值。

如果dword，安装程序将创建一个32位整数（REG_DWORD）值。

如果qword，安装程序将创建一个64位整数（REG_QWORD）值。

如果binary，安装程序将创建一个二进制（REG_BINARY）值。

举例：

```
ValueType: string
```

### ValueName

要修改的值的名称，可以包含常量。如果为空白，它将修改“Default”值。

举例：

```
ValueName: "Version"
```

### ValueData

值的数据。如果ValueType参数是string、expandsz或multisz，这是一个可以包含常量的字符串。如果数据类型是dword或qword，它可以是一个十进制整数(例如:“123”)，一个十六进制整数。(例如:“$7B”)，或解析为整数的常量。如果数据类型是binary，那么这是一个形式为“00 ff 12 34”的十六进制字节序列。如果数据类型为none，则忽略它。

对于字符串、expandsz或multisz类型的值，可以在此参数中使用一个特殊的常量{olddata}。{olddata}被替换为注册表值的先前数据。如果您需要将一个字符串附加到一个现有的值，例如{olddata};{app}，那么{olddata}常量会很有用。如果值不存在或现有值不是字符串类型，则{olddata}常量将被悄悄地删除。如果创建的值是多字符串类型但现有值不是多字符串类型(即REG_SZ或REG_EXPAND_SZ)，则{olddata}也会被静默删除，反之亦然。

对于multisz类型的值，可以在此参数中使用称为{break}的特殊常量来嵌入换行符（空值）。

举例：

```
ValueData: "1.0"
```

### Permissions

指定要在注册表项的ACL（访问控制列表）中授予的其他权限。如果您不熟悉ACL或为什么需要更改它们，建议您不要使用此参数，因为滥用它可能会对系统安全性产生负面影响。

为了使此参数生效，当前用户必须能够更改注册表项的权限。如果不满足这些条件，则不会显示错误消息，并且不会设置权限。

此参数应仅在应用程序专用的注册表项上使用。切勿在HKEY_LOCAL_MACHINE\SOFTWARE之类的顶级密钥上更改ACL，否则，你可能会在用户系统上打开安全漏洞。

无论安装之前是否存在注册表项，都将设置指定的权限。如果ValueType为none且使用deletekey标志或deletevalue标志，则不设置权限。

在Windows的Itanium版本上，此参数仅对32位注册表项有效。 （在Windows x64版本上没有这种限制。）

此参数可以包含一个或多个以空格分隔的值，格式为：

```
<user or group identifier>-<access type>
```

[Registry]section支持以下访问类型：

#### full

授予“完全控制”权限，与“modify”相同（请参见下文），但是还允许指定的用户/组获得注册表项的所有权并更改其权限。谨慎使用；通常，modify就足够了。

#### modify

授予“修改”权限，该权限允许指定的用户/组读取，创建，修改和删除值和子项。

#### read

授予“读取”权限，该权限允许指定的用户/组读取值和子项。

举例：

```
Permissions: users-modify
```

### Flags

此参数是一组附加选项。通过用空格分隔多个选项。支持以下选项：

#### createvalueifdoesntexist

指定此标志后，安装程序将仅在不存在相同名称的值时创建该值。如果数据类型为none，或者指定了deletevalue标志，则此标志无效。

#### deletekey

指定此标志后，安装程序将首先尝试删除整个项（如果存在），包括其中的所有值和子项。如果ValueType不为none，则它将创建一个新的项和值。

为防止灾难，如果子项为空或仅包含反斜杠，则在安装过程中将忽略此标志。

#### deletevalue

指定此标志后，安装程序将首先尝试删除该值（如果存在）。如果ValueType不是none，它将创建项（如果还不存在）和新值。

#### dontcreatekey

指定此标志后，如果用户系统上尚不存在项，则安装程序将不会尝试创建项或任何值。如果项不存在，则不会显示任何错误消息。

#### noerror

如果安装程序由于任何原因无法创建项或值，则不显示错误消息。

#### preservestringtype

仅当ValueType参数为string或expandsz时才适用。当指定此标志且值不存在或现有值不是字符串类型(REG_SZ或REG_EXPAND_SZ)时，将使用ValueType指定的类型创建该标志。如果值确实存在并且是字符串类型，那么它将被替换为与先前存在的值相同的值类型。

#### uninsclearvalue

卸载程序后，将值的数据设置为空字符串（类型REG_SZ）。该标志不能与uninsdeletekey标志结合使用。

#### uninsdeletekey

卸载程序后，删除整个项，包括其中的所有值和子项。在Windows本身使用的项上使用此项显然不是一个好主意。你只应在应用程序专用的项上使用。

为防止灾难，如果子项为空或仅包含反斜杠，则在安装过程中将忽略此标志。

#### uninsdeletekeyifempty

程序卸载后，如果其中没有值或子项，删除该项。该标志可以与uninsdeletevalue结合使用。

为防止灾难，如果子项为空或仅包含反斜杠，则在安装过程中将忽略此标志。

#### uninsdeletevalue

卸载程序后删除该值。该标志可以与uninsdeletekeyifempty结合使用。

注意:在1.1之前的Inno安装版本中，您可以将此标志与数据类型none一起使用，它将作为“删除项如果为空”标志。此技术不再被支持。现在必须使用uninsdeletekeyifempty标志来完成此操作。

举例：

```
Flags: uninsdeletevalue
```
