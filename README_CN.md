# NSF2X 中文使用说明

## 软件简介

**NSF2X** 是一个将 Lotus Notes NSF 文件转换为 EML、MBOX 和 Outlook PST 格式的工具。该软件使用 Python 编写，提供图形界面，无需安装 Python 即可运行编译版本。

### 主要功能

- 📧 将 NSF 文件导出为 EML、MBOX 和 PST 格式
- 📎 保持邮件布局和附件完整
- 🔐 支持读取加密邮件，可重新加密为 RC2、3DES、AES128 或 AES256 格式
- 🌐 支持 Outlook 2013/2016/Office 365（即点即用版本）
- 🔧 支持 32 位和 64 位 Lotus Notes 与 Outlook 混合安装
- 📝 支持 Unicode 文件名（含特殊字符的文件名）
- 🌍 多语言支持（目前提供英语、法语、德语界面）

## 系统要求

### 必需软件

1. **Windows 操作系统**（不支持 Linux/macOS）
2. **Lotus Notes**（必须安装并运行）
3. **Microsoft Outlook**（仅在转换为 PST 格式时需要）

### 版本选择

- **32 位 Lotus Notes** → 下载并安装 `x86` 版本
- **64 位 Lotus Notes** → 下载并安装 `amd64` 版本

> 💡 **提示**：PST 转换时，即使 Outlook 的位数与 Lotus Notes 不同，软件也能自动处理。

## 快速开始

### 1. 准备工作

1. **备份 NSF 文件**：将需要转换的 NSF 文件复制到临时位置
   - ⚠️ **重要**：转换过程会修改 NSF 文件（转换为 MIME 格式），务必使用备份文件！
   - 在 Lotus Notes 运行前复制文件，避免文件被锁定

2. **启动 Lotus Notes**
3. **（推荐）启动 Microsoft Outlook**（仅在转换 PST 时需要）

### 2. 运行程序

1. 双击运行 `nsf2x.exe`
2. 输入 Lotus Notes 密码
3. 点击 **"Open Session"**（打开会话）按钮连接 Lotus Notes

### 3. 配置转换选项

#### 输出格式选择

- **EML**：为每个 NSF 文件创建子目录，每封邮件保存为单独的 .eml 文件，保留文件夹层级
- **MBOX**：每 NSF 文件创建一个 .mbox 文件（可配置是否保留文件夹层级）
- **PST**：每 NSF 文件创建一个 .pst 文件，保留文件夹层级

#### 高级选项

点击 **"Options"**（选项）按钮配置：

**MBOX 子文件夹处理**
- **否**：创建单个 MBOX 文件，丢弃子文件夹层级
- **是**：为每个子文件夹创建单独的 MBOX 文件，保留层级

**加密邮件处理**
- **None**：不加密，直接保存
- **RC2 40bit**：弱加密，兼容性好但不推荐
- **3DES 168bit**：中等安全性
- **AES 128 bit**：现代加密标准
- **AES 256 bit**：最高安全性

**错误日志级别**
- **Error**：仅显示错误信息
- **Warning**：显示警告和错误
- **Information**：显示所有信息（详细模式）

**异常处理**
- 设置允许的最大异常数（1/10/100/无限制）

**PST 辅助程序**
- **是**：始终使用外部辅助程序（解决 "File does not exist (259)" 错误）
- **否**：根据位数自动决定

### 4. 设置路径

- **Source Path**（源路径）：选择包含 NSF 文件的目录
- **Destination Path**（目标路径）：选择保存转换后文件的目录

### 5. 开始转换

1. 点击 **"Convert"**（转换）按钮
2. 进度显示在标题栏，错误信息显示在窗口中
3. 转换过程中可随时点击 **"Stop"**（停止）中止

## 各格式详细说明

### EML 格式

- 为每个 NSF 文件创建子目录 `<DestPath>/<NSFFileBasename>`
- 保留 Lotus Notes 中的文件夹层级
- 每封邮件保存为单独的 `.eml` 文件
- 可使用 Outlook、Thunderbird 等邮件客户端打开

### MBOX 格式

**单文件模式**：
- 每个 NSF 文件生成一个 `.mbox` 文件
- 文件名为原 NSF 文件名，扩展名改为 `.mbox`
- ⚠️ 文件夹层级被丢弃，所有邮件平铺存放

**多文件模式**（推荐）：
- 创建文件夹层级结构
- 每个子文件夹对应一个 MBOX 文件
- 适合 Thunderbird 等客户端导入

### PST 格式

- 每个 NSF 文件创建一个 `.pst` 文件
- 保留完整的文件夹层级
- 转换完成后 PST 文件会在 Outlook 中保持打开状态
- 可手动关闭后移动文件，或直接在最终位置创建

> ⚠️ **注意**：如果重复运行相同的源和目标路径，邮件会被复制两次！

## 常见问题与解决

### 转换速度慢

- **EML/MBOX**：速度较快，约为 PST 的 3-5 倍
- **PST**：较慢（10000 封邮件约需 30 分钟）
- 建议大型 NSF 文件在空闲时间转换，可锁屏但保持文件可访问

### "File does not exist error (259)" 错误

这是 Lotus Notes 和 Outlook 之间的兼容性问题。

**解决方法**：
1. 在选项中启用 **"Always use external PST helper function"**
2. 或逐个 NSF 文件转换，每次重启 NSF2X

### 加密邮件处理

NSF2X 可以读取您的 Notes ID 有权访问的所有加密邮件。

**安全警告**：
- EML 和 MBOX 输出文件中的加密会被移除（因为 EML 无法使用 Notes 加密）
- 建议转换后将存档存储在加密磁盘上
- 可使用用户证书重新加密（需在 Microsoft 加密存储中有证书）

### 无法转换的邮件

某些 Lotus Notes 邮件可能格式异常，无法转换为 MIME。这些邮件的主题会显示在日志窗口中，前缀为 `Subject : ...`

**手动恢复方法**：

1. **EML/MBOX 格式**：
   - 在 Lotus Notes 中将这些邮件拖到 Windows 桌面
   - Lotus 会另存为 EML 文件
   - 手动将这些 EML 文件复制到相应目录

2. **PST 格式**：
   - 将邮件拖到桌面保存为 EML
   - 打开命令提示符 (CMD.EXE)
   - 输入：`OUTLOOK.EXE /eml C:\Users\Me\Desktop\Message.eml`
   - 在弹出的 Outlook 窗口中将邮件拖到目标文件夹

## 技术信息

### 工作原理

**EML 转换**：
使用 Lotus Notes 的 C DLL 接口将邮件转换为 MIME 格式（修改 NSF 文件）

**PST 转换**：
1. 先将邮件转为临时 EML 文件
2. 使用扩展 MAPI 的 IConverter 接口导入
3. 或通过 Outlook COM 接口打开 EML 文件并移动

### 位数处理

如果 Lotus Notes 和 Outlook 位数不同：
- NSF2X 会自动导出为临时 EML 文件
- 调用对应位数的外部辅助程序完成 PST 转换

## 版权与许可

本软件基于 [nlconverter](https://github.com/kdeldycke/nlconverter) 开发，采用 **GNU GPL v2** 许可证。

版权所有 (C) 2016 Free Software Foundation  
作者：David Bateman <dbateman@free.fr>

## 获取帮助

- 下载地址：[Releases](../../releases/latest)
- 开发文档：[README.dev](./README.dev)
- 问题反馈：通过 GitHub Issues 提交

---

**⚠️ 免责声明**：使用本软件前请务必备份您的 NSF 文件。作者不对数据丢失或损坏负责。