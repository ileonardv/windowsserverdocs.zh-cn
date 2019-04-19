---
title: 要开始使用在 Android 上的远程桌面
description: Basic 为 Android 远程桌面客户端设置步骤。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 42b4b4ffb73bd9d5d1397d32bd36c41d7e404dd7
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297426"
---
# 要开始使用在 Android 上的远程桌面

>适用于： Windows 10、 Windows 8.1、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2

你可以使用 Android 远程桌面客户端要从 Android 设备直接使用 Windows 应用和桌面。

使用以下信息以开始。 请务必查看[常见问题解答](remote-desktop-client-faq.md)，如果你有任何问题。

> [!NOTE]
> - 好奇 Android 客户端的新版本？ 请查看[在 Android 上的远程桌面的新增功能？](android-whatsnew.md)
> 在 Android 4.1 和更高版本的设备上以及与安装的 ChromeOS 53 Chromebook 上，你可以运行客户端。 了解有关在 Chrome Android 应用程序[下面](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps)。

## 获取 RD 客户端和开始使用它

请按照以下步骤开始使用远程桌面 Android 设备上：

1. 从[Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)下载远程桌面客户端。 
2. [设置以接受远程连接的电脑](remote-desktop-allow-access.md)。
3. 添加远程桌面连接或远程资源。 使用连接直接连接到 Windows 电脑和远程资源使用 RemoteApp 程序、 基于会话的桌面或虚拟桌面发布在本地。 
4. 因此，你可以快速获得远程桌面，请创建一个构件。

> [!NOTE]
> 如果你想要更早版本进行外部测试新功能建议从 Google Play 应用商店下载我们[Microsoft 远程桌面 beta 版本](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta)的应用程序。 

### 添加远程桌面连接

若要创建远程桌面连接：

1. 在连接中心点击**+**，然后点击**桌面**。
2. 输入你想要连接到计算机的以下信息：
  - **电脑名称**– 计算机的名称。 这可以是 Windows 计算机名称、 Internet 域名或 IP 地址。 你也可以为电脑名称 （例如， **MyDesktop:3389**或**10.0.0.1:3389**） 附加端口信息。
  - **用户名**– 要用于访问远程电脑的用户名称。 你可以使用以下格式：*用户名*、*用户名*或*user_name@domain.com*。 你还可以指定是否提示输入用户名和密码。
3. 你还可以设置以下附加选项：
  - **友好名称**– 适用于要连接到电脑的轻松记住名称。 你可以使用任何字符串，但如果未指定一个友好名称，显示 PC 名称。
  - **网关**– 你想要使用连接到虚拟机、 RemoteApp 程序和基于会话的台式机公司内部网络上的远程桌面网关。 从系统管理员获取有关该网关的信息。
    需要配置远程桌面网关？
  - **声音**– 选择该设备要在远程会话期间用于音频。 你可以选择播放声音的本地设备，远程设备上或根本。
  - **自定义屏幕分辨率**-通过启用此设置设置连接的自定义解决方案。 当应用关闭分辨率已在应用的全局设置中定义。
  - **交换鼠标按钮**– 使用此选项以鼠标右键按钮交换鼠标左的按钮功能。 （如果这是特别有用远程电脑已配置为左手的用户，但使用右手鼠标。）
  - **连接到管理会话**-使用此选项以连接到控制台会话来管理 Windows server。
  - **将重定向到本地存储**– 可装载本地存储为远程文件系统上的远程电脑。
4. 点击**保存**。

需要编辑这些设置？ 点击桌面版名称旁边的溢出菜单 （**...**），然后点击**编辑**。

要删除连接？ 同样，点击溢出菜单 （**...**），然后点击**删除**。

>[!TIP]
> 如果你收到错误 0xf07 错误的密码 （"我们无法连接到远程电脑的用户帐户相关联的密码已过期"），更改密码，然后重试。

### 添加远程资源
远程资源是 RemoteApp 程序、 基于会话的台式机和发布 RemoteApp 和桌面连接使用的虚拟机。

若要添加的远程资源：

1. 在连接中心屏幕上，点击**+**，然后点击**远程资源源**。 
2. 输入远程资源的信息：
   - **电子邮件或 URL** -RD Web 访问服务器的 URL。 这将告知客户端搜索与你的电子邮件地址关联的 RD Web 访问服务器还可以在此字段 – 中输入你的公司电子邮件帐户。
   - **用户名**-要用于要连接到的 RD Web 访问服务器的用户名称。
   - **密码**-RD Web 访问服务器要连接到使用的密码。
3. 点击**保存**。

远程资源将显示在连接中心。


若要删除远程资源：

1. 在连接中心中，点击旁边远程资源溢出菜单 （**...**）。
2. 点击**删除**。
3. 确认删除。

### 小组件 – 固定到主页屏幕保存的桌面

远程桌面应用程序通过使用 Android 构件功能支持固定到主页屏幕的连接。 添加一个构件的方式取决于你使用的 Android 设备，并且其操作系统类型。 下面是添加构件的最常见方法： 

1. 点击**应用**启动应用菜单。
2. 点击**小组件**。
3. 轻扫通过小组件和查找远程桌面图标使用的说明，"Pin 远程桌面"。
4. 点击按住该远程桌面构件并将其移动到主屏幕。
5. 释放该图标时，你将看到已保存的远程桌面。 选择你想要将保存到主页屏幕的连接。

现在你可以通过点击直接从主页屏幕开始远程桌面连接。

> [!NOTE]
> 如果你重命名的远程桌面应用程序中的桌面连接，不会更新此固定的远程桌面的标签。

## 连接到 RD 网关访问内部资源

远程桌面网关 （RD 网关），可以从任何位置连接到公司网络上的远程计算机在 Internet 上。 你可以创建和管理使用远程桌面客户端在网关。

若要设置的新网关：

1. 在连接中心中，点击**设置 > 网关**。 点击**+** 若要添加的新网关。
2. 输入以下信息：
  - **服务器名称**– 你想要用作网关的计算机的名称。 这可以是 Windows 计算机名称、 Internet 域名或 IP 地址。 你还可以添加端口信息的服务器名称 (例如： **RDGateway:443**或**10.0.0.1:443**)。
  - **用户名**-用户名和密码以用于要连接到远程桌面网关。 你还可以选择**使用桌面的用户帐户**与用于远程桌面连接使用相同的凭据。

## 管理你的用户帐户

连接到桌面或远程资源时，你可以节省要从再次选择的用户帐户。 此外可以在客户端本身，而不是连接到桌面设备时保存用户数据中定义的用户帐户。

若要创建新的用户帐户：

1. 在连接中心中，点击**设置**，，然后点击**用户帐户**。
2. 点击**+** 添加新的用户帐户。
3. 输入以下信息：
   - **用户名**-用户保存与远程连接一起使用的名称。 你可以输入用户名称中的任何以下格式： 用户名，域 \ 用户名，或 user_name@domain.com。
   - **密码**-你指定的用户的密码。 你想要保存用于远程连接每个用户帐户需要有与之相关的密码。
4. 点击**保存**。

若要删除的用户帐户：

1. 在连接中心中，点击**设置 > 用户帐户**。
2. 点击并按住来选择它在列表中的用户帐户。 你可以选择多个用户。
3. 点击垃圾桶删除所选的用户。

## 导航远程桌面会话
启动远程桌面连接时，有工具可用，你可以使用导航会话。

### 开始菜单的远程桌面连接

1. 点击远程桌面连接来启动会话。 
2. 如果您需要验证的远程桌面的证书，请点击**连接**。 你还可以选择**不询问我再次进行到此计算机的连接**始终接受证书。

### 管理全局应用设置

你可以在 Android 客户端设置以下全局设置：

- **显示桌面预览**-允许你连接到它之前，请参阅连接中心中的桌面设备的预览。 默认情况下，这是设置为**上**。
- **收缩到缩放**-允许你使用捏合缩放收缩手势。 如果你使用的通过远程桌面应用支持多点触控 （在 Windows 8 中引入），打开此设置**关闭**。
- **有助于提高远程桌面**-向 Microsoft 发送匿名数据。 我们使用此数据以提高客户端。 你可以了解有关我们如何处理此匿名、 专用的数据的详细信息，请参阅[远程桌面客户端隐私声明](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx)。 默认情况下，此设置处于**打开**状态。
- **显示**-有两个全局显示设置：
   - **方向**的设置为你的会话的首选的方向 （横向或纵向布局）。 
   >[!NOTE]
   > 如果你连接到运行 Windows 8 或较早版本的 Windows 的电脑，会话将不会正确缩放。 最好的方法是从电脑，断开连接，然后重新连接以你想要使用的方向。 更好的选项是至少升级到电脑 Windows 8.1。

   - **分辨率**-设置你想要全局用于桌面连接的分辨率。 如果你已设置为单独的应用或连接的自定义分辨率，此设置不会更改该设置。
   >[!NOTE]
   >一个显示设置更改时，它们仅适用于新连接从该点上。 若要查看已连接断开连接，然后重新连接在会话中的更改。

### 连接栏

连接栏可以访问其他导航控件。 默认情况下，连接栏放置在屏幕顶部的中间。 双击并向左或向右以将其移动拖动栏。

- **平移控件**： 平移控件使屏幕放大，四处移动。 请注意，平移控件仅可使用直接触摸。
   - 启用 / 禁用平移控件： 点击连接栏中显示的平移控件和缩放屏幕的平移图标。 点击再次来隐藏该控件并返回到其原始分辨率的屏幕连接栏中的平移图标。
   - 使用平移控件： 点击并按住平移控件，然后在你想要移动屏幕的方向拖动。
   - 移动平移控件： 双击点击并按住移动在屏幕上的控件的平移控件。
- **其他选项**： 点击要显示会话选择栏和命令栏 （见下方） 的其他选项图标。
- **键盘**： 单击键盘图标，以显示或隐藏键盘。 显示键盘时，将自动显示平移控件。
- **移动连接栏**： 点击和按住连接栏，然后拖放和拖放到屏幕顶部的新位置。


### 命令栏

点击连接栏显示在屏幕的右侧的命令栏。 你可以在鼠标模式 （直接触摸和鼠标指针） 之间切换。 使用主页按钮来返回到连接中心从命令栏。 或者，你可以为相同的操作使用后退按钮。 活动会话将不会断开连接。 


### 使用直接在远程会话中触摸手势和鼠标模式

客户端使用标准触控笔势。 你还可以使用触控笔势复制在远程桌面的鼠标操作。 可用的鼠标模式下表中定义。

> [!NOTE]
> 交互与 Windows 8 或更高版本中直接触摸模式支持本机触控笔势。 

| 鼠标模式    | 鼠标操作      | 手势                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| 直接触摸  | 单击鼠标左键           | 1 个手指点击                                                          |
| 直接触摸  | 右键单击          | 1 个手指点击并按住                                                 |
| 鼠标指针 | 缩放                 | 使用 2 个手指并且收缩即可放大或移动手指分开以缩小内容。 |
| 鼠标指针 | 单击鼠标左键           | 1 个手指点击                                                          |
| 鼠标指针 | 左键单击并拖动  | 1 个手指双击和保留，然后拖动                               |
| 鼠标指针 | 右键单击          | 2 个手指点击                                                          |
| 鼠标指针 | 右键单击并拖动 | 2 个手指双击和暂停，然后拖动                               |
| 鼠标指针 | 鼠标滚轮          | 2 个手指点击和按住，然后拖动向上或向下                           |

> [!TIP]
> 问题和意见始终是欢迎。 但是，请不发布有关疑难解答帮助在本文末尾使用注释功能的请求。 相反，请转到[远程桌面客户端论坛](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc)和启动新线程。 有功能建议？ 在[客户端用户语音论坛](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android)告知我们。