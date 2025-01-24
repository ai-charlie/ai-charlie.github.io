# Win React Native 环境配置

此教程为windows下的React Native安卓开发环境配置，其他环境请参照 RN官网和RN中文网的安装，如果Android Studio 开启提示设置Http Proxy 的同学也可以直接参考本教程的阅读原文。

## 必需的软件

建议使用Chocolatey包管理，能够避免环境变量的配置。

### Chocolatey

Chocolatey是一个 Windows 上的包管理器，类似于 linux 上的 `yum`和 `apt-get`。 你可以在其官方网站上查看具体的使用说明。

一般来说，使用 Chocolatey 来安装软件的时候，需要以管理员的身份来运行命令提示符窗口。译注：chocolatey 的网站可能在国内访问困难，导致上述安装命令无法正常完成。请使用稳定的翻墙工具。 如果你实在装不上这个工具，也不要紧。下面所需的 python2 和 nodejs 你可以分别单独去对应的官方网站下载安装即可。

### 1、使用cmd.exe安装

运行以下命令：

```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```

### 2、使用PowerShell.exe安装

使用PowerShell，还有一个额外的步骤。您必须确保Get-ExecutionPolicy不受限制。我们建议使用 `Bypass`绕过策略来安装东西或者 `AllSigned`提高安全性。

跑 `Get-ExecutionPolicy`。如果它返回 `Restricted`，则运行 `Set-ExecutionPolicy AllSigned`或 `Set-ExecutionPolicy Bypass -Scope Process`。

现在运行以下命令：

```
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

### Python 2

打开命令提示符窗口，使用 Chocolatey 来安装 Python 2.

> 注意目前不支持 Python 3 版本。

```
choco install python2
```

python2 下载

```
https://www.python.org/ftp/python/2.7.16/python-2.7.16.amd64.msi
```

### Node

打开命令提示符窗口，使用 Chocolatey 来安装 NodeJS。

```
choco install nodejs.install
```

如果您已在系统上安装了Node，请确保它是Node 8.3或更高版本。如果您的系统上已有JDK，请确保它是版本8或更高版本。

> 您可以在Node的下载页面上找到其他安装选项。

**Node的下载页面**

```
https://nodejs.org/en/download/
```

安装完 node 后建议设置 npm 镜像以加速后面的过程（或使用科学上网工具）。注意：不要使用 cnpm！cnpm 安装的模块路径比较奇怪，packager 不能正常识别！

```
npm config set registry https:npm config set disturl https:
```

### Yarn、React Native 的命令行工具（react-native-cli）

Yarn是 Facebook 提供的替代 npm 的工具，可以加速 node 模块的下载。React Native 的命令行工具用于执行创建、初始化、更新项目、运行打包服务（packager）等任务。

```
npm install -g yarn react-native-cli
```

安装完 yarn 后同理也要设置镜像源：

```
yarn config set registry https:yarn config set disturl https:
```

> 如果你遇到EACCES: permission denied权限错误，可以尝试运行下面的命令（限 linux 系统）： sudo npm install -g yarn react-native-cli.

安装完 yarn 之后就可以用 yarn 代替 npm 了，例如用 `yarn`代替 `npm install`命令，用 `yarn add 某第三方库名`代替 `npm install --save 某第三方库名`。

> 注意：目前 npm5（发文时最新版本为 5.0.4）存在安装新库时会删除其他库的问题，导致项目无法正常运行。请尽量使用 yarn 代替 npm 操作。

### Android Studio

Android Studio安装包：

---

```
链接：https:提取码：pe6g 复制这段内容后打开百度网盘手机App，操作更方便哦
```

Android Studio 第一次打开的时候提醒设置HTTP Proxy，可以参考该

**设置HTTP Proxy详细过程如下**

### 1、设置Http Proxy为No Proxy，关闭Android Studio。

### 1、进入网站http://ping.chinaz.com/，进行 `dl.google.com` ping检查，选择大陆响应时间最短的IP地址

### 2进入cmd对此IP地址进行ping测试，如果可以将（IP地址 dl.google.com）加入hosts文件中

hosts文件地址：

```
C:\WINDOWS\System32\drivers\etc\hosts
```

建议用vs code修改host文件，保存时点击提示中的“以管理员身份重试”即可。

React Native 目前需要Android Studio2.0 或更高版本。

> Android Studio 需要 Java Development Kit [JDK] 1.8 或更高版本。你可以在命令行中输入 javac -version来查看你当前安装的 JDK 版本。如果版本不合要求，则可以到 官网上下载。 或是使用包管理器来安装（比如choco install jdk8或是 apt-get install default-jdk）

jdk 下载地址

```
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
```

Android Studio 包含了运行和测试 React Native 应用所需的 Android SDK 和模拟器。

> 除非特别注明，请不要改动安装过程中的选项。比如 Android Studio 默认安装了 Android Support Repository，而这也是 React Native 必须的（否则在 react-native run-android 时会报 appcompat-v7 包找不到的错误）。

- 确定所有安装都勾选了，尤其是 `Android SDK`和 `Android Device Emulator`。
- 在初步安装完成后，选择 `Custom`安装项：
- 检查已安装的组件，尤其是模拟器和 HAXM 加速驱动。
- 装完成后，在 Android Studio 的欢迎界面中选择 `Configure | SDK Manager`。

`这一步在 React Native 中文网是让我们安装Android 6.0 (Marshmallow)但是在英文的官网中已经更新到了Android 9 (Pie)，可以选择和自己物理真机一样的版本，将选择的 Android 版本和 Android SDK Build-Tools` `相应即可，这样你在用真机调试的时候比较方便。`

- 在 `SDK Platforms`窗口中，选择 `Show Package Details`，然后在 `Android 6.0 (Marshmallow)`中勾选 `Google APIs`、`Android SDK Platform 23`、`Intel x86 Atom_64 System Image`，无需选择 `Google APIs Intel x86 Atom_64 System Image`。Google API在中国基本是用不上的，没有必要安装，占用虚拟机内存。
- 在 `SDK Tools`窗口中，选择 `Show Package Details`，然后在 `Android SDK Build Tools`中勾选 `Android SDK Build-Tools 23.0.1`（这个tools占用内存空间不大，可以全部下载）。然后还要勾选最底部的 `Android Support Repository`.

### ANDROID_HOME 环境变量

确保 `ANDROID_HOME`环境变量正确地指向了你安装的 Android SDK 的路径。

打开 `控制面板` -> `系统和安全` -> `系统` -> `高级系统设置` -> `高级` -> `环境变量` -> `新建`

> 具体的路径可能和下图不一致，请自行确认。

> 你需要关闭现有的命令符提示窗口然后重新打开，这样新的环境变量才能生效。

## 推荐Android Studio虚拟机打开方式

点击在右下角的 Configure 打开 AVD Manager ，这一步遇到问题一般是电脑没有打开虚拟内存，需要重启电脑进入root界面修改设置。

Haxm虚拟内存根据自己电脑的配置情况而定，基本按默认就OK，然后点击Create Virtual Device 新建一个虚拟机，

## 推荐安装的工具

### Gradle Daemon

开启Gradle Daemon可以极大地提升 java 代码的增量编译速度。

```
(if not exist "%USERPROFILE%/.gradle" mkdir "%USERPROFILE%/.gradle") && (echo org.gradle.daemon=true >> "%USERPROFILE%/.gradle/gradle.properties")
```

### 将 Android SDK 的 Tools 目录添加到 `PATH`变量中

你可以把 Android SDK 的 tools 和 platform-tools 目录添加到 `PATH`变量中，以便在终端中运行一些 Android 工具，例如 `android avd`或是 `adb logcat`等。

打开 `控制面板` -> `系统和安全` -> `系统` -> `高级系统设置` -> `高级` -> `环境变量` -> 选中 `PATH` -> 双击进行编辑

> 注意你的具体路径可能和下图不同

## 可选的安装项

### Git

你可以使用 Chocolatey 来安装 `git`:

```
choco install git
```

另外你也可以直接去下载Git for Windows。 在安装过程中注意勾选“Run Git from Windows Command Prompt”，这样才会把 `git`命令添加到 `PATH`环境变量中。

### Genymotion

比起 Android Studio 自带的原装模拟器，Genymotion 是一个性能更好的选择，但它只对个人用户免费。

1. 下载和安装Genymotion（genymotion 需要依赖 VirtualBox 虚拟机，下载选项中提供了包含 VirtualBox 和不包含的选项，请按需选择）。
2. 打开 Genymotion。如果你还没有安装 VirtualBox，则此时会提示你安装。
3. 创建一个新模拟器并启动。
4. 启动 React Native 应用后，可以按下 F1 来打开开发者菜单。

### Visual Studio Emulator for Android

Visual Studio Emulator for Android是利用了 Hyper-V 技术进行硬件加速的免费 android 模拟器。也是 Android Studio 自带的原装模拟器之外的一个很好的选择。而且你并不需要安装 Visual Studio。 在用于 React Native 开发前，需要先在注册表中进行一些修改：

1. 打开运行命令（按下 Windows+R 键）
2. 输入 `regedit.exe`然后回车
3. 在注册表编辑器中找到 `HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Android SDK Tools`条目
4. 右键点击 `Android SDK Tools`，选择 `新建 > 字符串值`
5. 名称设为 `Path`
6. 双击 `Path`，将其值设为你的 Android SDK 的路径。（例如 `C:\Program Files\Android\sdk`）
