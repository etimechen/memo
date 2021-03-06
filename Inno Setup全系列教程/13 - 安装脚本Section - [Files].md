# 安装脚本Section

## [Files] section

此可选项section定义安装程序要在用户系统上安装的所有文件。

这是[Files]section的部分示例：

```
[Files]
Source: "CTL3DV2.DLL"; DestDir: "{sys}"; Flags: onlyifdoesntexist uninsneveruninstall
Source: "MYPROG.EXE"; DestDir: "{app}"
Source: "MYPROG.CHM"; DestDir: "{app}"
Source: "README.TXT"; DestDir: "{app}"; Flags: isreadme
```

有关一些重要说明，请参阅本主题底部的“备注”部分。

以下是受支持的参数的列表：

### Source（必填）

源文件的名称。如果未指定完全的路径名​​，则编译器将安装源目录的路径加在前面。

可以是通配符，用于在单个条目中指定一组文件。使用通配符时，与之匹配的所有文件都使用相同的选项。如果指定了标志external，则Source必须是用户系统上现有文件（或通配符）的完整路径名（例如“ {src}\license.ini”）。

常量只能在指定了external标志的情况下使用，因为编译器本身不会进行任何常量转换。

举例：

```
Source: "MYPROG.EXE"
Source: "Files\*"
```

### DestDir（必填）

用户系统上文件的安装目录。总是以目录常量之一开始。如果指定的路径在用户系统上不存在，则将自动创建该路径，并在卸载期间自动删除空目录。

举例：

```
DestDir: "{app}"
DestDir: "{app}\subdir"
```

### DestName

当该文件安装在用户系统上时，此参数为该文件指定一个新名字。默认情况下，安装程序使用Source参数中的名称，因此在大多数情况下，无需指定此参数。

举例：

```
DestName: "MYPROG2.EXE"
```

### Excludes

指定要排除的列表，以逗号分隔。此参数不能与external标志结合使用。

可以包含通配符（“\*”和“?”）。请注意，与Source参数不同，Excludes使用了简单的Unix风格的模式匹配惯例。其中的小数点始终很重要，因此“\*.\*”将不排除没有扩展名的文件（而是仅使用“*”）。另外，问号总是精确匹配一个字符，因此“?????”不会排除名称少于五个字符的文件。

如果以反斜杠（“\”）开头，则将其与路径名的开头匹配，否则，它与路径名的末尾匹配。因此，“\foo”将仅在尾部排除名为“foo”的文件。另一方面，“foo”将排除任何位置的任何名为“foo”的文件。

匹配表达式可能包含反斜杠。 “foo\bar”将同时排除“foo\bar”和“subdir\foo\bar”。 “\foo\bar”将仅排除“foo\bar”。

举例：

```
Source: "*"; Excludes: "*.~*"
Source: "*"; Excludes: "*.~*,\Temp\*"; Flags: recursesubdirs
```

### ExternalSize

此参数必须与external标志结合使用，并以字节为单位指定外部文件的大小。如果未指定此参数，则安装程序将在启动时检索文件大小。主要用于启动时不可用的文件，例如，使用disk spanning时位于第二个磁盘上的文件。

举例：

```
ExternalSize: 1048576; Flags: external
```

### CopyMode

你不应在任何新脚本中使用此参数。不建议使用此参数，并在Inno Setup 3.0.5中将其替换为标志：

```
CopyMode: normal -> Flags: promptifolder
CopyMode: alwaysskipifsameorolder -> no flags
CopyMode: onlyifdoesntexist -> Flags: onlyifdoesntexist
CopyMode: alwaysoverwrite -> Flags: ignoreversion
CopyMode: dontcopy -> Flags: dontcopy
```

什么是CopyMode：alwaysskipifsameorolder是现在的默认行为。（以前的默认值为CopyMode：normal。）

### Attribs

指定文件的其他属性。这可以包括以下一项或多项：readonly, hidden, system, notcontentindexed。如果未指定此参数，则安装程序不会为该文件分配任何特殊属性。

```
Attribs: hidden system
```

### Permissions

指定要在文件的ACL（访问控制列表）中授予的其他权限。如果你不熟悉ACL或为什么需要更改它们，建议你不要使用此参数，因为滥用它可能会对系统安全性产生负面影响。

为了使该参数生效，文件必须位于支持ACL的分区（例如NTFS）上，并且当前用户必须能够更改文件的权限。如果不满足这些条件，则不会显示错误消息，并且不会设置权限。

此参数仅在应用程序用户私有的文件上使用。切勿更改共享系统文件上的ACL，否则你可以在用户系统上打开安全漏洞。

不管文件在安装之前是否存在，都将设置指定的权限。

此参数可以包含一个或多个以空格分隔的值，格式为：

```
<user or group identifier>-<access type>
```

[Files]section支持以下访问类型：

#### full

授予“完全控制”权限，与“修改”权限相同（请参见下文），但是还允许指定的用户/组获得文件的所有权和更改权限。谨慎使用；通常，“修改”权限就足够了。

#### modify

授予“修改”权限，该权限允许指定的用户/组读取，执行，创建，修改和删除文件。

#### readexec

授予“读取和执行”权限，该权限允许指定的用户/组读取和执行文件。

```
Permissions: users-modify
```

### FontInstall

告诉安装程序文件需要安装的字体。此参数的值是存储在注册表或WIN.INI中的字体名称。该名称必须与在资源管理器中双击字体文件时看到的名称完全相同。请注意，安装程序将自动在名称末尾附加“（TrueType）”。

如果文件不是TrueType字体，则必须在Flags参数中指定标志fontisnttruetype。

在将字体安装到{fonts}目录中时，建议使用标志onlyifdoesntexist和uninsneveruninstall。

要成功安装字体，用户必须是Administrators组的成员。

为了与64位Windows兼容，不应将字体安装到{sys}目录中。使用{fonts}作为目标目录。

举例：

```
Source: "OZHANDIN.TTF"; DestDir: "{fonts}"; FontInstall: "Oz Handicraft BT"; Flags: onlyifdoesntexist uninsneveruninstall
```

### StrongAssemblyName

指定文件的强集名称。仅由“卸载”使用。

如果未同时指定gacinstall标志，则忽略此参数。

举例：

```
StrongAssemblyName: "MyAssemblyName, Version=1.0.0.0, Culture=neutral, PublicKeyToken=abcdef123456, ProcessorArchitecture=MSIL"
```

### Flags

此参数是一组附加选项。通过用空格分隔多个选项。支持以下选项：

#### 32bit

当在Source和DestDir参数中使用时，导致{sys}常量映射到32位系统目录，regserver和regtypelib标志将文件视为32位，以及sharedfile标志更新32位SharedDLLs注册表键。

#### 64bit

当在Source和DestDir参数中使用时，导致{sys}常量映射到64位系统目录，regserver和regtypelib标志将文件视为64位，以及sharedfile标志更新64位SharedDLLs注册表键。

#### allowunsafefiles

禁用编译器对不安全文件的自动检查。强烈建议你不要使用此标志，除非你完全确定知道自己在做什么。

#### comparetimestamp

（不推荐；请参见下文）

如果要安装的文件已存在于用户系统上并且至少满足以下条件之一，则指示安装程序继续比较时间戳记：

* 现有文件或正在安装的文件都没有版本信息。
* 该条目上使用了ignoreversion标志。
* 不使用replacesameversion标志，并且现有文件和正在安装的文件具有相同的版本号（由文件的版本信息确定）。

如果现有文件的时间戳记比要安装的文件的时间戳记早，则将替换现有文件。否则，它将不会被替换。

除非万不得已，否则不建议使用此标志，因为它有一个问题：NTFS分区将时间戳存储在UTC中（与FAT分区不同），导致本地时间戳记（Inno Setup的默认设置）在用户更改系统时区或夏令时生效或失效时发生偏移。这可能会导致一种情况，即当用户不希望文件被替换时替换文件，或者在用户希望它们被替换时不替换文件。

#### confirmoverwrite

在替换现有文件之前，始终让用户确认。

#### createallsubdirs

默认情况下，编译器递归搜索源文件名/通配符的子目录时会跳过空目录。该标志使这些目录在安装时创建（就像您为它们创建了[Dirs]条目一样）。

必须与recursesubdirs结合使用。

#### deleteafterinstall

指示安装程序照常安装文件，但是一旦安装完成（或中止），则将其删除。这对于提取脚本的[Run]section中执行的程序所需的临时数据很有用。

该标志不会导致安装期间未替换的现有文件被删除。

该标志不能与isreadme，regserver，regtypelib，restartreplace，sharedfile或uninsneveruninstall标志结合使用。

#### dontcopy

不在正常文件复制阶段将文件复制到用户系统，而是将文件静态编译到安装中。如果文件仅由[Code]section处理并使用ExtractTemporaryFile提取，则此标志很有用。

#### dontverifychecksum

阻止安装程序在提取后验证文件校验和。在已编译到安装程序中的情况下，对要修改的文件使用此标志。

必须与nocompression结合使用。

#### external

该标志指示Inno Setup不要将Source参数指定的文件静态编译到安装文件中，而是从用户系统上的现有文件复制。有关更多信息，请参见Source参数说明。

#### fontisnttruetype

如果条目正在安装带有FontInstall参数的非TrueType字体，请指定此标志。

#### gacinstall

将文件安装到.NET全局程序集缓存中。与共享文件结合使用时，仅在引用计数达到零时才卸载文件。

卸载文件卸载程序使用参数StrongAssemblyName指定的强程序集名称。

如果尝试在不存在.NET Framework的系统上使用此标志，则会引发异常。

#### ignoreversion

完全不比较版本信息；替换现有文件，而不考虑其版本号。此标志仅应用于应用程序用户私有文件，而不能用于共享系统文件。

#### isreadme

文件是“README”。安装中只有一个文件可以具有此标志。当文件带有此标志时，用户将在安装完成后询问他/她是否要查看README文件。如果选择是，安装程序将使用文件类型的默认程序打开文件。因此，README文件应始终以.txt，.wri或.doc等扩展名结尾。

请注意，如果安装程序必须重新启动用户的计算机（由于安装了带有标志restartreplace的文件或AlwaysRestart [Setup]section指令为yes），则不会为用户提供查看README文件的选项。

#### nocompression

阻止编译器压缩文件。在你无法从压缩中受益的文件类型（例如JPEG图像）上使用此标志可以加快编译过程并在生成的安装中节省一些字节。

#### noencryption

阻止文件被加密存储。如果启用了加密（使用[Setup]section指令Encryption），但希望能够在用户输入正确的密码之前使用[Code]section支持功能ExtractTemporaryFile提取文件，请使用此标志。

#### noregerror

与regserver或regtypelib标志结合使用时，如果注册失败，安装程序将不会显示任何错误消息。

#### onlyifdestfileexists

仅当用户系统上已经存在相同名称的文件时，才安装该文件。如果您的安装是现有安装的补丁程序，并且您不想安装用户尚未拥有的文件，则此标志可能很有用。

#### onlyifdoesntexist

仅在用户系统上尚不存在的文件才安装。

#### overwritereadonly

始终会覆盖一个只读文件。没有此标志，安装程序将询问用户是否应覆盖现有的只读文件。

#### promptifolder

默认情况下，当要安装的文件的版本号（或时间戳，使用comparetimestamp标志时）比现有文件旧时，安装程​​序将不会替换现有文件。（有关更多详细信息，请参阅本主题底部的“备注”部分。）使用此标志时，安装程​​序将询问用户是否应替换文件，默认是保留现有文件。

#### recursesubdirs

指示编译器或安装程序也在Source目录下的子目录中搜索Source文件名/通配符。

#### regserver

注册DLL/OCX文件。设置此标志后，安装程序将调用DLL/OCX文件导出的DllRegisterServer函数，卸载程序将在删除文件之前调用DllUnregisterServer。与共享文件结合使用时，仅当引用计数达到零时，才会注销DLL/OCX文件。在64位安装模式下，假定该文件是64位映像，并将在64位进程中注册。您可以通过指定32bit标志来覆盖它。

有关更多信息，请参见本主题底部的“备注”。

#### regtypelib

注册类库（.tlb）。卸载程序将注销类库（除非指定了标志uninsneveruninstall）。与regserver标志一样，当与共享文件结合使用时，仅当引用计数达到零时，卸载程序才会取消注册该文件。

在Windows x64版本上运行的64位安装模式下，类库将在64位进程中注册。您可以通过指定32bit标志来覆盖它。

不支持在Windows Itanium版本上以64位模式注册类型库。

有关更多信息，请参见本主题底部的“备注”。

#### replacesameversion

当使用此标志并且该文件在用户系统上已经存在并且与要安装的文件具有相同的版本号时，安装程​​序将比较这些文件，如果内容不同则替换现有文件。

默认行为（即不使用此标志时）是不替换具有相同版本号的现有文件。

#### restartreplace

当需要替换现有文件，并且另一个运行进程正在使用（锁定）现有文件时，安装程​​序将默认显示错误消息。此标志告诉安装程序在下次系统重新启动时（通过调用MoveFileEx或在WININIT.INI中创建一个条目）代替注册要替换的文件。发生这种情况时，将在安装过程结束时提示用户重新启动计算机。

注意：如果用户没有管理员权限，则此标志无效。因此，使用此标志时，建议您将 [Setup]section的PrivilegesRequired指令默认设为admin。

#### setntfscompression

指示安装程序对文件启用NTFS压缩（即使它没有替换文件）。如果由于某种原因（例如，文件系统不支持压缩）而无法设置压缩状态，则不会显示错误消息。

#### sharedfile

指定该文件在多个应用程序之间共享，如果没有其他应用程序在使用它，则仅应在卸载时将其删除。安装到Windows系统目录的大多数文件都应使用此标志，包括.OCX，.BPL和.DPL文件。

Windows的标准共享文件引用计数机制（位于注册表中的HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SharedDLLs）用于跟踪有多少应用程序依赖于该文件。每次安装文件时，文件的引用计数都会增加。（无论安装程序是否实际覆盖磁盘上的文件，都会发生这种情况。）卸载使用该文件的应用程序时，引用计数将减少。如果计数达到零，则删除文件（在用户确认的情况下，除非还指定了uninsnosharedfileprompt标志）。

如果安装程序多次运行，则文件的引用计数将增加一次以上。但是，卸载程序会将引用计数递减相同的次数，因此不会泄漏任何引用（前提是未将UninstallLogMode [Setup]section指令更改为默认设置append）。

使用此标志时，请勿在DestDir参数中指定{syswow64}。请改用{sys}。即使{sys}和{syswow64}在32位安装模式下映射到相同的基础目录，路径名称也必须与其他所有现有安装程序正在使用的名称完全匹配。否则，将为该文件创建第二个引用计数，这可能会导致文件过早地被删除。如果需要在64位安装模式下将共享文件安装到32位系统目录中，请在DestDir参数中指定{sys}，并另外包括32bit标志。

#### sign

该标志指示编译器在存储原始源文件之前对其进行数字签名。如果未设置[Setup]section指令SignTool，则将被忽略。

#### signonce

该标志指示编译器在存储原始源文件之前对其进行数字签名，但前提是尚未对文件进行签名。如果未设置[Setup]section指令SignTool，则将被忽略。

#### skipifsourcedoesntexist

如果源文件不存在，则此标志指示编译器（如果也使用外部标志，则为Setup）以静默方式跳过条目，而不显示错误消息。

#### solidbreak

启用solid compression后，此标志指示编译器在压缩与Source匹配的文件之前完成当前压缩流并开始一个新的压缩流。这使安装程序可以立即搜索文件，而不必先解压缩任何先前的文件。如果发现花费大量时间来解压缩属于未选择组件的文件，则在大型的多组件安装中可能会很有用。

#### sortfilesbyextension

该标志指示编译器压缩先按扩展名排序文件，然后再按路径名排序。如果还使用了solid compression，则可能会减小安装程序的大小。

#### sortfilesbyname

该标志指示编译器压缩先按扩展名排序文件，然后再按路径名排序。如果还使用solid compression，则可能会减小安装程序的大小。如果还使用sortfilesbyextension，则文件首先按扩展名排序。

#### touch

此标志使安装程序将已安装文件的时间戳记设置为TouchDate和TouchTime [Setup]section指令所指定的时间戳记。

#### uninsnosharedfileprompt

卸载共享文件时，如果其引用计数达到零，则自动删除文件，而不是询问用户。必须与sharedfile标志结合使用才能生效。

#### uninsremovereadonly

卸载文件时，从文件中删除所有只读属性，然后再尝试将其删除。

#### uninsrestartdelete

当使用此标志并且卸载时文件正在使用中时，卸载程序将在系统重新启动时将要删除的文件排队，并且在卸载过程结束时询问用户是否要重新启动。当卸载无法以编程方式停止的shell扩展之类的东西时，此标志很有用。请注意，要使此标志生效，需要管理员权限。

#### uninsneveruninstall

不允许删除文件。当安装非常常见的共享文件（例如MFC DLL）在任何情况下都不应删除时，此标志很有用。

请注意，如果将此标志与sharedfile标志结合使用，则在卸载时将永远不会删除该文件，但仍会适当减少引用计数。

#### unsetntfscompression

指示安装程序禁用文件上的NTFS压缩（即使它没有替换文件）。如果由于某种原因（例如，文件系统不支持压缩）而无法设置压缩状态，则不会显示错误消息。

举例：

```
Flags: isreadme
```

### Remarks

如果用户系统上已经存在文件，则默认情况下将根据以下规则替换该文件：

1. 如果现有文件的版本比要安装的文件的版本早（由文件的版本信息确定），则将替换现有文件。
2. 如果现有文件的版本与要安装的文件的版本相同，则将不替换现有文件，除非使用了replacesameversion标志并且两个文件的内容不同。
3. 如果现有文件的版本比要安装的文件新，或者如果现有文件具有版本信息，但是要安装的文件没有，则不会替换现有文件。
4. 如果现有文件没有版本信息，它将被替换。

某些标志（例如onlyifdoesntexist，ignoreversion和promptifolder）会更改上述规则。

如果安装程序由于另一个进程正在使用它而无法替换现有文件，则它将最多进行4次其他替换文件的尝试，每次尝试之前延迟一秒钟。如果所有尝试均失败，将显示错误消息。

安装程序将使用regserver或regtypelib标志注册所有文件，作为安装的最后一步。但是，如果[Setup]section的指令AlwaysRestart为yes，或者如果存在带有restartreplace标志的文件，则所有文件将在下次重新引导时注册（通过在Windows的RunOnce注册表项中创建一个条目）。

当卸载扩展名为.hlp的文件(Windows帮助文件)时，相应的.gid和.fts文件也会自动删除。类似地，当一个.chm (HTML帮助)文件被删除时，任何.chw(生成的索引)文件都会被自动删除。
