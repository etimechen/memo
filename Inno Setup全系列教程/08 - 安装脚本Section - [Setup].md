# 安装脚本Section

## [Setup] section

此section包含安装程序和卸载程序使用的全局设置。您创建的任何安装都需要包含指令。这是[Setup]的示例：

```
[Setup]
AppName=My Program
AppVersion=1.5
DefaultDirName={autopf}\My Program
DefaultGroupName=My Program
```

默认情况下，指令值中的任何开始或结束空格都将被忽略。可以通过将指令的值括在双引号（“）中来避免这种情况。

可以在[Setup]section中放置以下指令：

`(bold = required)`

### 编译器相关

* ASLRCompatible
* Compression
* CompressionThreads
* DEPCompatible
* DiskClusterSize
* DiskSliceSize
* DiskSpanning
* Encryption
* InternalCompressLevel
* LZMAAlgorithm
* LZMABlockSize
* LZMADictionarySize
* LZMAMatchFinder
* LZMANumBlockThreads
* LZMANumFastBytes
* LZMAUseSeparateProcess
* MergeDuplicateFiles
* Output
* OutputBaseFilename
* OutputDir
* OutputManifestFile
* ReserveBytes
* SignedUninstaller
* SignedUninstallerDir
* SignTool
* SignToolMinimumTimeBetween
* SignToolRetryCount
* SignToolRetryDelay
* SignToolRunMinimized
* SlicesPerDisk
* SolidCompression
* SourceDir
* TerminalServicesAware
* UsedUserAreasWarning
* UseSetupLdr
* VersionInfoCompany
* VersionInfoCopyright
* VersionInfoDescription
* VersionInfoOriginalFileName
* VersionInfoProductName
* VersionInfoProductTextVersion
* VersionInfoProductVersion
* VersionInfoTextVersion
* VersionInfoVersion

### 安装器相关

功能性：这些指令会影响安装程序的操作，或者保存用于卸载器以后使用。

* AllowCancelDuringInstall
* AllowNetworkDrive
* AllowNoIcons
* AllowRootDirectory
* AllowUNCPath
* AlwaysRestart
* AlwaysShowComponentsList
* AlwaysShowDirOnReadyPage
* AlwaysShowGroupOnReadyPage
* AlwaysUsePersonalGroup
* AppendDefaultDirName
* AppendDefaultGroupName
* AppComments
* AppContact
* AppId
* AppModifyPath
* AppMutex
* AppName
* AppPublisher
* AppPublisherURL
* AppReadmeFile
* AppSupportPhone
* AppSupportURL
* AppUpdatesURL
* AppVerName
* AppVersion
* ArchitecturesAllowed
* ArchitecturesInstallIn64BitMode
* ChangesAssociations
* ChangesEnvironment
* CloseApplications
* CloseApplicationsFilter
* CreateAppDir
* CreateUninstallRegKey
* DefaultDialogFontName
* DefaultDirName
* DefaultGroupName
* DefaultUserInfoName
* DefaultUserInfoOrg
* DefaultUserInfoSerial
* DirExistsWarning
* DisableDirPage
* DisableFinishedPage
* DisableProgramGroupPage
* DisableReadyMemo
* DisableReadyPage
* DisableStartupPrompt
* DisableWelcomePage
* EnableDirDoesntExistWarning
* ExtraDiskSpaceRequired
* InfoAfterFile
* InfoBeforeFile
* LanguageDetectionMethod
* LicenseFile
* MinVersion
* OnlyBelowVersion
* Password
* PrivilegesRequired
* PrivilegesRequiredOverridesAllowed
* RestartApplications
* RestartIfNeededByRun
* SetupLogging
* SetupMutex
* ShowLanguageDialog
* TimeStampRounding
* TimeStampsInUTC
* TouchDate
* TouchTime
* Uninstallable
* UninstallDisplayIcon
* UninstallDisplayName
* UninstallDisplaySize
* UninstallFilesDir
* UninstallLogMode
* UninstallRestartComputer
* UpdateUninstallLogAppName
* UsePreviousAppDir
* UsePreviousGroup
* UsePreviousLanguage
* UsePreviousPrivigeles
* UsePreviousSetupType
* UsePreviousTasks
* UsePreviousUserInfo
* UserInfoPage

外观性：这些指令仅影响安装程序的外观。

* AppCopyright
* BackColor
* BackColor2
* BackColorDirection
* BackSolid
* FlatComponentsList
* SetupIconFile
* ShowComponentSizes
* ShowTasksTreeLines
* WindowShowCaption
* WindowStartMaximized
* WindowResizable
* WindowVisible
* WizardImageAlphaFormat
* WizardImageFile
* WizardImageStretch
* WizardResizable
* WizardSizePercent
* WizardSmallImageFile
* WizardStyle

### 过时的

以下指令已过时，不应在任何新脚本中使用。

* AlwaysCreateUninstallIcon
* DisableAppendDir
* DontMergeDuplicateFiles
* MessagesFile
* UninstallIconFile
* UninstallIconName
* UninstallStyle
* WizardImageBackColor
* WizardSmallImageBackColor
