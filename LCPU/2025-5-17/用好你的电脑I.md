# 用好你的电脑I

## C++环境配置

我已经配置好了DEV-c++，VS code，Clion

在LCPU中要求使用VS Code（我是Windows），符合要求

## Python环境配置

我已经配置好了Pycharm，VS code

## Conda环境

我已经配置好了Anaconda3

来看一些指令吧：

- pip show：查看包的具体信息。例如 pip show pip。

- pip list：查看已经安装的包。

- pip install：安装指定的包。后面要接包名。

- pip freeze > requirements.txt：把包列表输出到文本文件。

- pip install -r requirements.txt：根据文本文件批量安装包。

- pip uninstall：卸载包。

- pip install --upgrade 包名称==版本号：更新包。

- python -m pip install --upgrade pip：更新 pip。

## 文件


### 文本和文档
- .txt：纯文本文件，可用记事本打开，无格式修饰。
- .doc/.docx：Microsoft Word 文档格式，后者为新版（基于 XML）。
- .xls/.xlsx：Microsoft Excel 表格格式，后者为新版（支持更大数据量）。
- .ppt/.pptx：Microsoft PowerPoint 演示文稿格式，后者为新版（支持更多动画和媒体功能）。
### 源代码文件
C++：
- .cpp：C++ 源文件（常用）。
- .cc/.cxx：C++ 源文件（较少见，部分项目使用）。
- .hpp/.hxx：C++ 头文件（含函数声明、类定义等）。
C：
- .c：C 语言源文件。
- .h：C 语言头文件。
Python：
- .py：Python 脚本文件，可直接运行或导入模块。
### 视频文件
- .mp4：常用多媒体格式，兼容性强，支持 H.264 编码。
- .avi：传统视频格式，基于 AVI 容器，兼容性好但压缩效率较低。
- .mkv：高清视频常用格式，支持多音轨、字幕和无损压缩。
- .ts：传输流格式，常用于直播和蓝光光盘。
- .m3u8：HTTP 直播流索引文件，用于分片视频播放（如流媒体平台）。
### 图片文件
- .jpg/.jpeg：有损压缩图片格式，适合照片（文件小但画质有损失）。
- .png：无损压缩图片格式，支持透明背景，适合图标、图形设计。
- .gif：支持动画和透明背景，色彩数量有限，适合简单动画。
- .bmp：位图格式，未压缩，画质高但文件体积大。
- .webp：新兴格式，压缩比优于 JPEG/PNG，支持透明和动画（需现代浏览器支持）。
- .svg：矢量图格式，放大不失真，适合图标、插画和字体设计。
### 压缩文件
通用格式：
- .zip：跨平台压缩格式，Windows/macOS/Linux 通用，支持加密。
- .rar：高压缩比格式，需 WinRAR 软件解压（免费版可试用）。
- .7z：开源压缩格式，压缩比高于 ZIP/RAR，支持 LZMA2 算法。
Linux 常用：
- .tar.gz：先通过 tar 打包目录，再用 gzip 压缩（常见于开源软件安装包）。
- .tar.xz：类似 .tar.gz，但使用 xz 压缩，压缩比更高。
macOS 专用：
- .dmg：磁盘映像文件，用于分发 macOS 软件（双击挂载后安装）。
光盘 / 虚拟光盘：
- .iso：光盘镜像文件，可刻录为物理光盘或用虚拟光驱加载（如 Windows 磁盘管理）。
### 可执行文件
Windows：
- .exe：Windows 可执行程序，双击直接运行（需注意安全风险，避免来源不明文件）。
Linux/Unix：
无固定后缀，通过文件权限标识可执行性（如 chmod +x script.sh）。
macOS：
- .app：应用程序包（本质为目录结构，右键可查看内容），拖入「应用程序」文件夹安装。
Java 程序：
- .jar：Java 归档文件，包含编译后的类和资源，通过 java -jar 命令运行。
### 脚本文件
Windows：
- .bat/.cmd：批处理脚本，用于自动化执行命令（基于命令提示符）。
- .ps1：PowerShell 脚本，功能更强大，支持复杂逻辑和系统管理。
Linux/Unix：
- .sh：Shell 脚本，可调用系统命令（如 bash script.sh 执行）。

## 文件系统：

FAT32：一种较老的文件系统，广泛用于 USB 闪存驱动器和存储卡。
NTFS：是 Windows 操作系统的默认文件系统，支持大文件和高级功能。
ext4：Linux 操作系统常用的文件系统，支持大文件和高性能。
APFS：苹果公司为 macOS 和 iOS 开发的文件系统，支持加密和快照。

## GItHub安装

1.进入项目的Release界面

2.找到Assets部分

- amd64（x64）架构：
要找到对应的架构，以及后缀为 .exe 或者 .msi 的文件来安装。

- Linux ：
要找到自己的架构，且后缀为 .deb、.tar.gz 等的文件；

- MacOS：
Apple 芯片，则要找到 aarch64、arm64 架构，.dmg 后缀的文件来安装。

3.打开安装包后，就可以开始安装了
