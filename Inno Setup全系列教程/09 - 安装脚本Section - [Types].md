# 安装脚本Section

## [Types] section

此section是可选的。它定义了安装程序将显示在向导的“选择组件”页面上的所有安装类型设置。如果你在[Components]section中定义了组件但未定义Types，则在编译过程中会创建一组默认的Types。如果您使用默认（英语）消息文件，则这些类型与以下示例中的类型相同。这是[Types]部分的示例：

```
[Types]
Name: "full"; Description: "Full installation"
Name: "compact"; Description: "Compact installation"
Name: "custom"; Description: "Custom installation"; Flags: iscustom
```

以下是受支持的参数的列表：

### Name（必填）

此类型的内部名称。在[Components]section中用作组件的参数，以指示安装程序组件所属的类型。

举例：

```
Name: "full"
```

### Description（必填）

此类型的描述，可以包括常量。该描述是在安装过程中显示。

举例：

```
Description: "Full installation"
```

### Flags

此参数是一组附加可选项。通过用空格分隔多个选项。支持以下选项：

#### iscustom

指明安装程序该类型是自定义类型。每当最终用户在安装过程中手动更改组件选择时，安装程​​序都会将安装程序类型设置为自定义类型。请注意，如果您未定义自定义类型，则安装程序将仅允许用户选择安装程序类型，并且他不可以再手动选择/取消选择组件。

只有一种类型可以包含此标志。

举例：

```
Flags: iscustom
```