---
title: 使用添加 DriverGroup 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a92fe8f-03f9-462a-b99e-f23275259807
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 322a9a671f90bf56f6357289f7727c142a0145cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825088"
---
# <a name="using-the-add-drivergroup-command"></a>使用添加 DriverGroup 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

将驱动程序组添加到服务器。
有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。
## <a name="syntax"></a>语法
```
wdsutil /add-DriverGroup /DriverGroup:<Group Name>\n\
[/Server:<Server name>] [/Enabled:{Yes | No}] [/Applicability:{Matched | All}] [/Filtertype:<Filter type> /Policy:{Include | Exclude} /Value:<Value> [/Value:<Value> ...]]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/DriverGroup:<Group Name>|指定新的驱动程序组的名称。|
|/ 服务器：<Server name>|指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果指定没有服务器名称，则使用本地服务器。|
|/ 已启用: {是&#124;无}|启用或禁用包。|
|/ 适用性: {匹配&#124;所有}|指定要满足的筛选器条件时安装的程序包。 **匹配**方法安装客户端的硬件匹配的驱动程序包。 **所有**表示将所有程序包都安装到客户端而不考虑其硬件。|
|/ 筛选器类型：<Filtertype>|指定要添加到组的筛选器的类型。 可以在单个命令中指定多个筛选器类型。 每个筛选器类型必须后跟 **/Policy**和至少一个 **/值**。 <Filtertype> 可以是以下值之一：<br /><br />**BiosVendor**<br /><br />**Biosversion**<br /><br />**Chassistype**<br /><br />**Manufacturer**<br /><br />**uuid**<br /><br />**Osversion**<br /><br />**Osedition**<br /><br />**OsLanguage**<br /><br />有关获取值对于所有其他筛选器类型的信息，请参阅[驱动程序组筛选器](https://go.microsoft.com/fwlink/?LinkID=155158)(https://go.microsoft.com/fwlink/?LinkID=155158)。|
|[/ 策略: {包括&#124;排除}]|指定要在筛选器上设置的策略。 如果 **/Policy**设置为**Include**，允许客户端计算机的筛选器匹配此组中安装的驱动程序。 如果 **/Policy**设置为**排除**，则不允许的筛选器匹配的客户端计算机在此组中安装的驱动程序。|
|[/ 值：<Value>]|指定客户端值对应于 **/Filtertype**。 可以指定多个单精度类型值。 请参阅以下列表了解某些筛选器类型的有效值。 以下属性适用于**Chassistype**。 有关获取的值对于所有其他筛选器类型的信息，请参阅[驱动程序组筛选器](https://go.microsoft.com/fwlink/?LinkID=155158)(https://go.microsoft.com/fwlink/?LinkID=155158)。<br /><br />**其他**<br /><br />**UnknownChassis**<br /><br />**桌面**<br /><br />**LowProfileDesktop**<br /><br />**PizzaBox**<br /><br />**MiniTower**<br /><br />**塔**<br /><br />**可移植**<br /><br />**Laptop**<br /><br />**笔记本**<br /><br />**手持设备**<br /><br />**DockingStation**<br /><br />**AllInOne**<br /><br />**SubNotebook**<br /><br />**SpaceSaving**<br /><br />**LunchBox**<br /><br />**MainSystemChassis**<br /><br />**ExpansionChassis**<br /><br />**SubChassis**<br /><br />**BusExpansionChassis**<br /><br />**PeripheralChassis**<br /><br />**StoraeChassis**<br /><br />**RackmountChassis**<br /><br />**SealedCasecomputer**<br /><br />**MultiSystemChassis**<br /><br />**compactPci**<br /><br />**AdvancedTca**|
## <a name="BKMK_examples"></a>示例
若要添加驱动程序组，请键入以下项之一：
```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Enabled:Yes
```
```
wdsutil /add-DriverGroup /DriverGroup:printerdrivers /Applicability:All /Filtertype:Manufacturer /Policy:Include /Value:Name1 /Filtertype:Chassistype /Policy:Exclude /Value:Tower /Value:MiniTower
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加 DriverGroupPackage 命令](using-the-add-drivergrouppackage-command.md)
[使用添加 DriverGroupPackages 命令](using-the-add-drivergrouppackages-command.md)
 [使用添加 DriverGroupFilter 命令](using-the-add-drivergroupfilter-command.md)