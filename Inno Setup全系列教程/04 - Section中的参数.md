# Section中的参数

除了[Setup]， [Message]， [CustomMessages]， [LangOptions] 和 [Code]以外，所有section的参数按行分隔开来，以下是[Files]section的示例：

```
[Files]
Source: "MYPROG.EXE"; DestDir: "{app}"
Source: "MYPROG.CHM"; DestDir: "{app}"
Source: "README.TXT"; DestDir: "{app}"; Flags: isreadme
```

每个参数由一个名称，一个冒号和一个值组成。参数是可选的，除非另有说明，如果未指定参数，则它们将采用默认值。一行上的多个参数用分号分隔，并且可以按任何顺序列出。

当参数的值包含用户定义的字符串（例如文件名）时，通常将其用双引号（“）引起来。使用引号不是必须的，但这样做可以在值的前面和后面添加空格，以及分号和双引号字符。

使用两个连续的双引号可转义值中带双引号的字符。例如：

```
"This "" contains "" embedded "" quotes"
```

安装编译器将其视为：

```
This " contains " embedded " quotes
```

如果您希望参数的值只是一个单个双引号字符，请使用四个双引号字符：“”“”，最外面两个单引号将引号引起来，内部两个单引号前一个是对后一个的转义。
