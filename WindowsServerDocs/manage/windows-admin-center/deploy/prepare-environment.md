---
title: 为 Windows Admin Center 准备环境
description: 为 Windows Admin Center (Project Honolulu) 准备环境
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 72e71ce2d1427f392aa02d32597f92d031f9a5c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407008"
---
# <a name="prepare-your-environment-for-windows-admin-center"></a>为 Windows Admin Center 准备环境

> 适用于：Windows Admin Center、Windows Admin Center 预览版

有些服务器版本在可以使用 Windows Admin Center 进行管理之前需要进行额外的准备：

- [Windows Server 2012 和 2012 R2](#prepare-windows-server-2012-and-2012-r2)
- [Windows Server 2008 R2](#prepare-windows-server-2008-r2)
- [Microsoft Hyper-V Server 2016](#prepare-microsoft-hyper-v-server-2016)
- [Microsoft Hyper-V Server 2012 R2](#prepare-microsoft-hyper-v-server-2012-r2)

在管理 Windows 管理中心之前，可能还需要修改[目标服务器上的端口配置](#port-configuration-on-the-target-server)。

## <a name="prepare-windows-server-2012-and-2012-r2"></a>准备 Windows Server 2012 和 2012 R2

### <a name="install-wmf-version-51-or-higher"></a>安装 WMF 5.1 或更高版本

默认情况下，Windows Server 2012 和 2012 R2 中未包含 Windows Admin Center 需要的 PowerShell 功能。 要使用 Windows Admin Center 管理 Windows Server 2012 或 2012 R2，你将需要在这些服务器上安装 WMF 5.1 或更高版本。

在 PowerShell 中键入 `$PSVersiontable`，以验证是否安装了 WMF 并且版本是否为 5.1 或更高版本。

如果未安装，则可以[下载并安装 WMF 5.1](https://docs.microsoft.com/powershell/wmf/setup/install-configure)。

## <a name="prepare-windows-server-2008-r2"></a>准备 Windows Server 2008 R2

### <a name="install-wmf-version-51-or-higher"></a>安装 WMF 5.1 或更高版本

默认情况下，Windows Server 2008 R2 中未包含 Windows Admin Center 需要的 PowerShell 功能。 要使用 Windows Admin Center 管理 Windows Server 2008 R2，你将需要在这些服务器上安装 WMF 5.1 或更高版本。 

确保计算机上已安装[.NET Framework 4.5.2 或更高版本](https://docs.microsoft.com/dotnet/framework/install/on-windows-7)。

在 PowerShell 中键入 `$PSVersiontable`，以验证是否安装了 WMF 并且版本是否为 5.1 或更高版本。

如果未安装，则可以[下载并安装 WMF 5.1](https://docs.microsoft.com/powershell/wmf/setup/install-configure)。

在 PowerShell 控制台中运行 `Enable-PSRemoting –force` 以启用 Powershell 远程连接。 

### <a name="enable-remote-desktop"></a>启用远程桌面

要在 Windows Admin Center 内使用远程桌面，你将需要在 Windows Server 2008 R2 服务器上启用远程桌面。

从**服务器管理器**中，转到**配置远程桌面**。 启用远程桌面以“允许运行任意版本远程桌面的计算机连接”。

## <a name="prepare-microsoft-hyper-v-server-2016"></a>准备 Microsoft Hyper-V Server 2016

要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2016，你将需要在执行此操作之前启用一些服务器角色。

### <a name="to-manage-microsoft-hyper-v-server-2016-with-windows-admin-center"></a>要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2016，请执行以下操作：

1. 启用远程管理。
2. 启用文件服务器角色。
3. 启用 PowerShell 的 Hyper-V 模块。

### <a name="step-1-enable-remote-management"></a>**步骤 1:** 启用远程管理

要在 Hyper-V Server 中启用远程管理，请执行以下操作：

1. 登录到 Hyper-V Server。
2. 在**服务器配置** (SCONFIG) 工具中，键入 **4** 以配置远程管理。
3. 键入 **1** 以启用远程管理。
4. 键入 **4** 以返回到主菜单。

### <a name="step-2-enable-file-server-role"></a>**步骤 2:** 启用文件服务器角色

要启用文件服务器角色以进行基本的文件共享和远程管理，请执行以下操作：

1. 在**工具**菜单中，单击**角色和功能**。
2. 在**角色和功能**中，查找**文件和存储服务**，并选择**文件和 iSCSI 服务**和**文件服务器**：

![显示选定的 "文件和 iSCSI 服务" 角色的角色和功能的屏幕截图](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-3-enable-hyper-v-module-for-powershell"></a>**步骤 3：** 启用 PowerShell 的 Hyper-V 模块

要启用 PowerShell 功能的 Hyper-V 模块，请执行以下操作：

1. 在**工具**菜单中，单击**角色和功能**。
2. 在**角色和功能**中，查找**远程服务器管理工具**，并选择**角色管理工具**和 **PowerShell 的 Hyper-V 模块**：

![显示选定的 Hyper-v 角色的角色和功能的屏幕截图](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2016 现在可以使用 Windows Admin Center 进行管理。

## <a name="prepare-microsoft-hyper-v-server-2012-r2"></a>准备 Microsoft Hyper-V Server 2012 R2

要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2012 R2，你将需要在执行此操作之前启用一些服务器角色。  此外，你将需要安装 WMF 5.1 或更高版本。

### <a name="to-manage-microsoft-hyper-v-server-2012-r2-with-windows-admin-center"></a>要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2012 R2，请执行以下操作：

1. 安装 Windows Management Framework (WMF) 5.1 或更高版本
2. 启用远程管理
3. 启用文件服务器角色
4. 启用 PowerShell 的 Hyper-V 模块

### <a name="step-1-install-windows-management-framework-51"></a>第 1 步：安装 Windows Management Framework 5。1

默认情况下，Microsoft Hyper-V Server 2012 R2 中未包含 Windows Admin Center 需要的 PowerShell 功能。 要使用 Windows Admin Center 管理 Microsoft Hyper-V Server 2012 R2，你将需要安装 WMF 5.1 或更高版本。

在 PowerShell 中键入 `$PSVersiontable`，以验证是否安装了 WMF 并且版本是否为 5.1 或更高版本。 

如果未安装，你可以[下载 WMF 5.1](https://docs.microsoft.com/powershell/wmf/setup/install-configure)。

### <a name="step-2-enable-remote-management"></a>步骤 2：启用远程管理

要启用 Hyper-V Server 远程管理，请执行以下操作：

1. 登录到 Hyper-V Server。
2. 在**服务器配置** (SCONFIG) 工具中，键入 **4** 以配置远程管理。
3. 键入 **1** 以启用远程管理。
4. 键入 **4** 以返回到主菜单。

### <a name="step-3-enable-file-server-role"></a>步骤 3:启用文件服务器角色

要启用文件服务器角色以进行基本的文件共享和远程管理，请执行以下操作：

1. 在**工具**菜单中，单击**角色和功能**。
2. 在**角色和功能**中，查找**文件和存储服务**，并选择**文件和 iSCSI 服务**和**文件服务器**：

![显示选定的 "文件和 iSCSI 服务" 角色的角色和功能的屏幕截图](../media/prepare-environment/c6c30b812d96afcc1edcdb6f52f0e13c.png)

### <a name="step-4-enable-hyper-v-module-for-powershell"></a>步骤 4：启用 PowerShell 的 Hyper-V 模块

要启用 PowerShell 功能的 Hyper-V 模块，请执行以下操作：

1. 在**工具**菜单中，单击**角色和功能**。
2. 在**角色和功能**中，查找**远程服务器管理工具**，并选择**角色管理工具**和 **PowerShell 的 Hyper-V 模块**：

![显示已选择 Hyper-v 远程服务器管理工具的角色和功能的屏幕截图](../media/prepare-environment/7ab0999602b7083733525bd0c1ba2747.png)

Microsoft Hyper-V Server 2012 R2 现在可以使用 Windows Admin Center 进行管理。

## <a name="port-configuration-on-the-target-server"></a>目标服务器上的端口配置

Windows 管理中心对某些文件复制任务（例如在远程服务器上导入证书时）使用 SMB 文件共享协议。 要使这些文件复制操作成功，远程服务器上的防火墙必须允许端口445上的入站连接。  你可以使用 Windows 管理中心中的防火墙工具来验证 "文件服务器远程管理（SMB 输入）" 的传入规则是否设置为 "允许在此端口上访问"。

> [!Tip]
> 已准备好安装 Windows Admin Center？ [立即下载](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center#download-now)
