---
title: bitsadmin getdisplayname
description: 适用于**bitsadmin getdisplayname**的 Windows 命令主题-检索指定作业的显示名称。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 229bd245f9e810fc6aeb856bbfba253b9ab8a9f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381626"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname



检索指定作业的显示名称。

## <a name="syntax"></a>语法

```
bitsadmin /GetDisplayName <Job>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|

## <a name="BKMK_examples"></a>示例

下面的示例检索名为*myDownloadJob*的作业的显示名称。
```
C:\>bitsadmin /GetDisplayName myDownloadJob
```
其他参考

[命令行语法项](command-line-syntax-key.md)