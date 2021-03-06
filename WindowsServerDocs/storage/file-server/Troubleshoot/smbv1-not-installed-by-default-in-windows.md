---
title: 默认情况下，windows 10 版本1709、Windows Server 版本1709及更高版本中未安装 SMBv1
description: 讨论 Windows 10 秋季创意者更新和 Windows Server 版本1709及更高版本中的 SMBv1 协议的行为。
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 9a6e9778e0ce1e50a70e68832390321fb2d9f971
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654358"
---
# <a name="smbv1-is-not-installed-by-default-in-windows-10-version-1709-windows-server-version-1709-and-later-versions"></a>默认情况下，windows 10 版本1709、Windows Server 版本1709及更高版本中未安装 SMBv1

## <a name="summary"></a>摘要

在 Windows 10 秋季创意者更新和 Windows Server 版本1709（RS3）及更高版本中，默认情况下不再安装服务器消息块版本1（SMBv1）网络协议。 从2007年开始，SMBv2 和更高版本的协议取代了它。 Microsoft 在2014中公开弃用了 SMBv1 协议。 

SMBv1 在 Windows 10 秋季创意者更新和 Windows Server 版本1709（RS3）中具有以下行为： 
 
- SMBv1 现在具有可单独卸载的客户端和服务器子功能。    
- 默认情况下，windows 10 企业版和 Windows 10 教育版在全新安装之后不再包含 SMBv1 客户端或服务器。    
- Windows Server 2016 在全新安装之后，默认情况下不再包含 SMBv1 客户端或服务器。    
- 默认情况下，在全新安装后，windows 10 家庭版和 Windows 10 专业版不再包含 SMBv1 服务器。    
- 默认情况下，在全新安装后，windows 10 家庭版和 Windows 10 专业版仍包含 SMBv1 客户端。 如果 SMBv1 客户端的总时间不超过15天（不包括计算机的关闭状态），则它会自动将其卸载。    
- Windows 10 家庭版和 Windows 10 专业版的就地升级和内幕航班，最初不会自动删除 SMBv1。 如果 SMBv1 客户端或服务器的总时间不超过15天（不包括计算机关闭的时间），则它们会自动卸载。     
- Windows 10 企业版和 Windows 10 教育版的就地升级和内幕航班不会自动删除 SMBv1。 管理员必须决定卸载这些托管环境中的 SMBv1。 在 Windows 10 版本1809（RS5）及更高版本中，管理员可以通过打开 "SMB 1.0/CIFS 自动删除" 功能来激活 SMBv1 的自动删除。    
- 15天后自动删除 SMBv1 的操作是一次性的。 如果管理员重新安装 SMBv1，则不会再进一步尝试卸载它。
- SMB 版本2.02、2.1、3.0、3.02 和3.1.1 功能仍完全受支持，并且默认情况下作为 SMBv2 二进制文件的一部分包含在内。    
- 由于计算机浏览器服务依赖于 SMBv1，因此，如果卸载 SMBv1 客户端或服务器，则会卸载该服务。 这意味着，资源管理器 Networkcan 不再通过旧的 NetBIOS 数据报浏览方法显示 Windows 计算机。    
- 在所有版本的 Windows 10 和 Windows Server 2016 中，仍然可以重新安装 SMBv1。    
 
  > [!NOTE]
  > Windows 10 版本1803（RS4）专业版处理 SMBv1 的方式与 Windows 10、版本1703（RS2）和 Windows 10 版本1607（RS1）相同。 此问题已在 Windows 10 版本1809（RS5）中解决。 你仍可手动卸载 SMBv1。 但是，在以下情况下，Windows 将不会在15天后自动卸载 SMBv1： 
 
-  执行 Windows 10 版本1803的全新安装。     
-  将 Windows 10 版本1607或 Windows 10 版本1703升级到 Windows 10 1803 （版本），无需先升级到 Windows 10，版本1709。     
 
如果你尝试连接到仅支持 SMBv1 的设备，或者这些设备尝试连接到你，你可能会收到以下错误消息之一：     

```
You can't connect to the file share because it's not secure. This share requires the obsolete SMB1 protocol, which is unsafe and could expose your system to attack.
Your system requires SMB2 or higher. For more info on resolving this issue, see: https://go.microsoft.com/fwlink/?linkid=852747  
```

```
The specified network name is no longer available.
```

```
Unspecified error 0x80004005
```

```
System Error 64
```

```
The specified server cannot perform the requested operation.
```

```
Error 58
```    

当远程服务器需要来自此客户端的 SMBv1 连接，但在客户端上卸载或禁用了 SMBv1 时，将显示以下事件。

```
Log Name:      Microsoft-Windows-SmbClient/Security
 Source:        Microsoft-Windows-SMBClient
 Date:          Date/Time
 Event ID:      32002
 Task Category: None
 Level:         Info
 Keywords:      (128)
 User:          NETWORK SERVICE
 Computer:      junkle.contoso.com
 Description:
 The local computer received an SMB1 negotiate response. 

Dialect: 
SecurityMode 
Server name: 

Guidance: 
SMB1 is deprecated and should not be installed nor enabled. For more information, see https://go.microsoft.com/fwlink/?linkid=852747.
```

```
Log Name:      Microsoft-Windows-SmbClient/Security
 Source:        Microsoft-Windows-SMBClient
 Date:          Date/Time
 Event ID:      32000
 Task Category: None
 Level:         Info
 Keywords:      (128)
 User:          NETWORK SERVICE
 Computer:      junkle.contoso.com
 Description: 
SMB1 negotiate response received from remote device when SMB1 cannot be negotiated by the local computer. 
Dialect: 
Server name: 

Guidance: 
The client has SMB1 disabled or uninstalled. For more information: https://go.microsoft.com/fwlink/?linkid=852747.     
```

这些设备不可能运行 Windows。 它们更有可能运行早期版本的 Linux、Samba 或其他类型的第三方软件来提供 SMB 服务。 通常，这些版本的 Linux 和 Samba 将不再受支持。 

> [!NOTE]
> Windows 10 版本1709也称为 "秋季创意者更新"。   

## <a name="more-information"></a>详细信息

若要解决此问题，请与仅支持 SMBv1 的产品制造商联系，并请求支持 SMBv 2.02 或更高版本的软件或固件更新。 有关已知供应商的当前列表及其 SMBv1 要求，请参阅以下 Windows 和 Windows Server 存储工程团队博客文章： 

[SMBv1 产品交换所](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/SMB1-Product-Clearinghouse/ba-p/426008) 
#### <a name="leasing-mode"></a>租赁模式

如果需要 SMBv1 来提供旧软件行为的应用程序兼容性，例如禁用 oplock 的要求，Windows 将提供一个新的 SMB 共享标志，称为租赁模式。此标志指定共享是否禁用新式 SMB 语义，如租约和 oplock。

可以不使用 oplock 或租约指定共享，以允许旧版应用程序使用 SMBv2 或更高版本。 为此，请将**new-smbshare**或**new-smbshare** PowerShell Cmdlet 与 **-LeasingMode None** 参数一起使用。

> [!NOTE]
> 仅应在第三方应用程序所需的共享上使用此选项，以便在供应商声明需要时使用。 不要在横向扩展文件服务器使用的用户数据共享或 CA 共享上指定租用模式。 这是因为删除 oplock 和租约会导致大多数应用程序的不稳定和数据损坏。 租赁模式仅适用于共享模式。 它可供任何客户端操作系统使用。

#### <a name="explorer-network-browsing"></a>资源管理器网络浏览

计算机浏览器服务依赖于 SMBv1 协议来填充 Windows 资源管理器网络节点（也称为 "网络邻居"）。 此旧版协议已过时，不会进行路由，并且安全性有限。 由于在没有 SMBv1 的情况下服务无法正常工作，因此它将同时被删除。

但是，如果仍需要使用资源管理器网络流入量 home 和 small business 工作组环境来查找基于 Windows 的计算机，则可以在不使用 SMBv1 的基于 Windows 的计算机上执行以下步骤： 
 
1. 启动 "功能发现提供程序主机" 和 "函数发现资源发布" 服务，然后将它们设置为 "**自动（延迟启动）** "。

2. 打开资源管理器网络时，在出现提示时启用网络发现。    
 
该子网中具有这些设置的所有 Windows 设备现在将显示在 "网络中进行浏览"。 这将使用 WS 发现协议。 如果在出现 Windows 设备后，其设备仍不显示在此浏览列表中，请联系你的其他供应商和制造商。 可能禁用了此协议，或者它们只支持 SMBv1。

> [!NOTE]
> 我们建议你映射驱动器和打印机，而不是启用此功能，这仍需要搜索和浏览其设备。 映射的资源更易于定位，需要较少的培训，并且更安全。 如果通过组策略自动提供这些资源，则更是如此。 管理员可以通过使用 IP 地址、Active Directory 域服务（AD DS）、Bonjour、Mdn、uPnP 等方法，使用旧计算机浏览器服务以外的方法配置打印机的位置。

如果你不能使用这些解决方法中的任何一种，或者应用程序制造商无法提供支持的 SMB 版本，你可以按照[如何在 Windows 中检测、启用和禁用 SMBv1、SMBv2 和 SMBv3](detect-enable-and-disable-smbv1-v2-v3.md)中的步骤手动重新启用 SMBv1。

> [!IMPORTANT]
> 强烈建议您不要重新安装 SMBv1。 这是因为此较旧的协议具有有关勒索软件和其他恶意软件的已知安全问题。   

## <a name="references"></a>引用

[停止使用 SMB1](https://aka.ms/stopusingsmb1)