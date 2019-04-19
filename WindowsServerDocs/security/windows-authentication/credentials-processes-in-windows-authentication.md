---
title: "在 Windows 身份验证的凭据进程"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48c60816-fb8b-447c-9c8e-800c2e05b14f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 52d50a1bb6bfbe9d35146f362a2a5184df0a6504
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="credentials-processes-in-windows-authentication"></a>在 Windows 身份验证的凭据进程

>适用于：Windows Server（半年通道），Windows Server 2016

IT 专业人员的此参考主题介绍 Windows 身份验证如何处理的凭据。

Windows 凭据管理是操作系统凭据将接收来自服务或用户和保护该信息，以便将来为身份验证目标的过程。 在已加入域的计算机的情况下进行身份验证的目标是域控制器。 身份验证中使用凭据数字将用户的身份验证真品证书，如证书、密码或 PIN 的一些形式向相关联的文档。

默认情况下，Windows 凭据进行验证与安全帐户管理器（三千）数据库在本地计算机上或在已加入域的计算机中，通过 Winlogon 服务 Active Directory。 登录的用户界面或通过应用程序编程接口 (API) 呈现到身份验证的目标编程方式用户输入通过收集的凭据。

下的注册表中存储本地安全信息**HKEY_LOCAL_MACHINE\SECURITY**。 存储的信息包括策略设置、安全的默认值和帐户信息，如缓存的登录凭据。 三千数据库一份存储还在这里，，不过很写保护。

以下图表显示所需的组件和路径，凭据会通过系统进行身份验证的用户或成功登录的进程。

![显示所需的组件和路径凭据的图示通过系统需要进行身份验证的用户或成功登录的进程。](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

下表介绍了管理凭据在登录时验证过程中的每个组件。

**身份验证组件的所有系统**

|组件|描述|
|-------|--------|
|用户登录|Winlogon.exe 是可执行文件负责管理安全用户交互。 通过收集的用户操作安全于桌面版 (登录 UI) 上到本地安全颁发机构 (LSA) 通过 Secur32.dll 的凭据，Winlogon 服务启动 Windows 操作系统的登录过程。|
|登录应用程序|应用程序或不需要交互式登录登录服务。 大多数启动的用户使用 Secur32.dll，而在启动时，如服务、启动进程使用 Ksecdd.sys 以内核模式运行以用户模式运行的进程。<br /><br />有关用户模式和内核模式的详细信息，请参阅本主题中的应用程序和用户模式或服务和内核模式。|
|Secur32.dll|多个的身份验证提供程序从身份验证的基础处理。|
|Lsasrv.dll|LSA 服务器服务，也不能同时强制安全策略充当 LSA 安全包管理器。 LSA 包含协商函数中，选中确定哪个协议已成功后 NTLM 或 Kerberos 协议。|
|安全支持的提供商|一套可以单独调用了一个或多个身份验证协议的提供商。 提供商的默认设置可以更改的每个版本的 Windows 操作系统、，并且是可写自定义的提供商。|
|Netlogon.dll|网络登录服务执行服务如下所示：<br /><br />-维护的计算机安全通道（无法与 Schannel 混淆）到某个域控制器。<br />-将通过安全通道用户的凭据传递域控制器，然后返回域安全标识符 (Sid)，为用户的用户权利。<br />-在域名系统 (DNS) 发布服务资源记录并使用 DNS 域控制器的 Internet 协议 (IP) 地址解决名称。<br />-实现复制协议根据远程过程调用 (RPC) 有关同步主域控制器 (Pdc) 和备份域控制器 (Bdc)。|
|Samsrv.dll|存储安全本地帐户、安全帐户管理器（三千）执行本地存储的策略，并支持 Api。|
|注册表|注册表包含一份三千数据库、本地安全策略设置、安全的默认值和才可以访问系统的帐户信息。|

本主题包含以下部分：

-   [用户登录的的凭据输入](#BKMK_CrentialInputForUserLogon)

-   [应用和服务的登录凭据输入](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [本地安全颁发机构](#BKMK_LSA)

-   [缓存的凭据和验证](#BKMK_CachedCredentialsAndValidation)

-   [凭据存储和验证](#BKMK_CredentialStorageAndValidation)

-   [安全帐户管理器的数据库](#BKMK_SAM)

-   [本地域和受信任的域](#BKMK_LocalDomainsAndTrustedDomains)

-   [在 Windows 身份验证的证书](#BKMK_CertificatesInWindowsAuthentication)

## <a name="BKMK_CrentialInputForUserLogon"></a>用户登录的的凭据输入
在 Windows Server 2008 和 Windows Vista 的图形识别，身份验证 (GINA) 体系结构已替换为凭据提供商型号，这使你可以枚举登录磁贴使用不同的登录类型。 下面介绍了这两种型号。

**图形识别，身份验证的体系结构**

图形识别，身份验证 (GINA) 体系结构可适用于 Windows Server 2003，Microsoft Windows 2000 Server、Windows XP 和 Windows 2000 专业版操作系统。 这些系统，在每次登录交互式会话创建 Winlogon 服务的单独实例。 GINA 体系结构加载到使用 Winlogon 进程空间，接收和处理凭据，并且调用通过 LSALogonUser 身份验证接口。

运行会话 0 交互式登录 Winlogon 实例。 会话 0 主机系统服务和其他重要的过程，其中包括本地安全颁发机构 (LSA) 的过程。

以下图表显示为 Windows Server 2003、Microsoft Windows 2000 Server、Windows XP 和 Microsoft Windows 2000 专业版的凭据过程。

![Windows Server 2003、Microsoft Windows 2000 Server、Windows XP 和 Microsoft Windows 2000 专业版中显示的凭据进程的图示](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**凭据提供商建筑**

适用于这些版本中指定的凭据提供程序体系结构**适用于**开头的本主题列表。 在这些系统中，输入凭据的体系结构更改为可扩展设计通过使用凭据提供商。 这些提供商表示不同登录上的磁贴安全桌面允许任意数量的登录方案的不同相同的用户和不同身份验证方法，例如密码、智能卡和生物识别的帐户。

使用凭据提供商的体系结构，Winlogon 始终登录 UI 之后开始收到一个安全关注序列事件。 登录 UI 查询提供商配置为枚举其他凭据类型数每个凭据提供的商。 凭据提供商已这些磁贴之一指定为默认值的选项。 所有的提供商有枚举其磁贴后，登录 UI 向用户显示它们。 用户与磁贴提供自己的凭据交互。 登录 UI 提交这些进行身份验证的凭据。

凭据提供商不是执法机制。 它们用于收集序列的凭据。 本地安全颁发机构，身份验证的程序包强制安全性。

凭据提供商的计算机上注册，负责如下：

-   描述所需的身份验证的凭据信息。

-   处理通信并使用外部认证颁发机构的逻辑。

-   对于包装凭据交互式和网络登录。

对于包装凭据交互式和网络登录包括序列的过程。 通过序列凭据登录的多个磁贴可以显示在登录 UI。 因此，你的组织可以控制例如用户、目标系统上的登录到的网络和工作站锁定/解锁策略-使用自定义的凭据提供商预登录访问登录屏幕。 同一台计算机上，可以同时存在多个凭据提供商。

作为标准凭据提供商或前登录访问提供商可以开发单一登录 (SSO) 提供商。

每个版本的 Windows 包含一个默认凭据提供商和一个默认前-登录访问提供商 (PLAP)，也称为 SSO 提供商。 SSO 提供商允许用户连接到网络之前登录到本地计算机上。 实现该提供商时, 提供商不枚举登录 ui 的磁贴。

在以下情况中使用均无意 SSO 提供程序：

-   **网络身份验证和计算机的登录凭据不同提供商由处理。** 为此项 scenario 变化包括：

    -   用户在登录到计算机之前已连接到网络，如连接到虚拟专用网络 (VPN) 的选项，但无需进行此连接。

    -   网络身份验证需要检索交互式本地计算机上的身份验证过程中使用的信息。

    -   多个网络身份验证跟其他方案之一。 例如，用户对 Internet 服务提供商 (ISP) 进行身份验证、到 VPN，验证，然后使用其用户帐户凭据登录本地。

    -   禁用缓存的凭据，并远程访问服务连接到 VPN 之前本地登录要求进行身份验证的用户。

    -   用户域没有在已加入域的计算机上设置了本地帐户，必须远程访问服务通过建立连接之前完成交互式登录的 VPN 连接。

-   **网络身份验证和计算机登录处理相同的凭据提供商。** 在此情况下，用户都必须连接到之前登录到计算机的网络。

**登录磁贴枚举**

凭据提供商枚举在以下情况下登录磁贴：

-   对于那些中指定的操作系统**适用于**开头的本主题列表。

-   凭据提供商枚举工作站登录磁贴。 凭据提供商通常序列到本地安全颁发机构验证的凭据。 此过程显示到每个用户目标系统的特定的每位用户的和特定的磁贴。

-   登录和身份验证的体系结构允许用户用于解锁工作站枚举凭据提供商的磁贴。 通常，当前登录的用户是默认磁贴，但多用户登录后，如果显示了大量的磁贴。

-   凭据提供商枚举磁贴，以更改其密码或其他私密的信息，如 PIN 用户请求的响应。 通常，当前登录的用户是默认磁贴;但是，如果多用户登录时，会显示了大量的磁贴。

-   凭据提供商枚举基于序列化凭据，用于进行身份验证的远程计算机上的磁贴。 作为登录 UI，即解除锁定工作站或更改密码，凭据 UI 不使用的相同的提供商处。 因此，不能之间的凭据 UI 实例提供程序中保留状态的信息。 此结构将导致每个远程计算机登录，则凭据已正确序列一个磁贴。 此项 scenario 还可在用户帐户控制 (UAC)，可帮助防止未经授权的更改计算机的通过之前允许的操作，可能会影响的计算机的操作时，或者，无法更改设置会影响该计算机的其他用户提示用户提供权限或管理员密码。

下图显示中指定的操作系统的凭据过程**适用于**开头的本主题列表。

![图中显示中指定的操作系统的凭据过程 ** 适用于 ** 列表本主题中的开头](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>应用和服务的登录凭据输入
Windows 身份验证旨在管理应用程序或不需要用户交互的服务的凭据。 以用户模式应用程序不能超过在哪些系统资源有权时服务可以可以自由访问系统内存和外部设备。

系统服务和传输级别应用程序访问安全支持提供商 (SSP) 通过安全支持提供程序接口 (SSPI) 在 Windows 中，该法案枚举可用的系统上的安全程序包、选择包，以及使用该包获取身份验证的连接的功能。

身份验证的客户端月服务器连接时：

-   在客户端应用程序的连接到服务器使用 SSPI 函数的发送凭据`InitializeSecurityContext (General)`。

-   连接的应用上的服务器端响应与 SSPI 函数`AcceptSecurityContext (General)`。

-   SSPI 功能`InitializeSecurityContext (General)`和`AcceptSecurityContext (General)`重复直到交换必要的身份验证的所有邮件成功或失败身份验证。

-   已身份验证的连接后，服务器上的 LSA 使用信息来自客户端版本安全上下文，其中包含访问标记。

-   服务器可能再说 SSPI 函数`ImpersonateSecurityContext`将访问令牌附加到该服务模拟线程。

**应用程序和用户模式**

在 Windows 中的用户模式组成的两个可将 I/O 请求传递到相应的内核模式驱动程序的系统：环境系统，运行的目标读者为许多不同类型的操作系统的应用程序，并且不可或缺的系统，表示代表环境系统系统特定功能。

整体系统管理代表环境系统的操作 system'specific 功能，并包含的安全系统进程 (LSA)、工作站一项服务，并服务器服务。 安全系统进程涉及安全标记，授予或拒绝基于资源的权限的用户帐户的访问权限、处理登录请求和启动登录身份验证，确定哪些操作系统需要审核的系统资源。

应用程序可以在用户模式运行该应用程序可以运行为包括的本地系统（系统）中的安全上下文中的任何主体的位置。 应用程序也可以运行以内核模式应用程序可以运行的本地系统（系统）中的安全上下文中的位置。

SSPI 版可通过 Secur32.dll 模块，这是用于获取安全集成的服务身份验证、消息完整性和消息隐私 API。 它提供一个应用程序级别协议和安全协议之间的抽象层。 不同的应用程序需要不同的方法找出或验证用户和加密数据，因为它通过网络传输不同的方法，因为 SSPI 使您能够访问动态链接包含不同的身份验证和加密函数的库 (Dll)。 这些 Dll 称为安全支持的提供商 (Ssp)。

托管服务帐户和虚拟帐户引入了 Windows Server 2008 R2 和 Windows 7 提供关键应用程序，如 Microsoft SQL Server 和 Internet 信息服务 (IIS)，使用他们自己的域帐户，隔离时手动不再需要的管理员管理服务主要名称 (SPN) 和这些帐户有关的凭据。 有关这些功能和其至关重要身份验证的详细信息，请参阅[Windows 7 和 Windows Server 2008 R2 托管服务帐户文档](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)和[组托管服务帐户概述](../group-managed-service-accounts/group-managed-service-accounts-overview.md)。

**服务和内核模式**

尽管大多数 Windows 应用程序启动它们的安全环境运行，这不是用户的正确的服务。 服务控制器被启动许多 Windows 服务，如网络和打印的服务，用户启动计算机。 这些服务作为本地服务或当地系统可能会运行，并运行最后一个人的用户注销后可能会继续。

> [!NOTE]
> 服务中的安全环境称为本地系统（系统）、网络服务或当地服务正常运行。  Windows Server 2008 R2 引入的托管的服务帐户、运行的是域主体的服务。

在开始前一项服务，该服务控制器登录使用帐户，指定的服务，然后展示了通过 LSA 的身份验证服务的凭据。 Windows service 实现服务控制器管理器可用于控制服务编程界面。 Windows service 可以系统启动或时手动与服务控制程序自动启动。 例如，当 Windows 客户端计算机加入域，计算机上的 messenger 服务连接到域控制器，然后打开安全通道到它。 若要获取身份验证的连接，该服务必须远程计算机的本地安全颁发机构 (LSA) 信任的凭据。 当网络中的其他计算机与通信，LSA 使用本地计算机域帐户，凭据，一样安全本地系统和网络的服务的环境中运行的所有其他服务。 作为系统运行，以便无需会提供给 LSA 凭据本地计算机上的服务。

文件 Ksecdd.sys 管理和加密这些凭据使用本地过程调用 LSA 插入。 该文件类型 DRV（驱动程序），以内核模式安全支持提供商 (SSP)，这些中指定的版本中称为**适用于**列表中的本主题开头，fips 1 兼容 140 2 级别。

内核模式已完全访问计算机的硬件和系统资源。 内核模式停止用户模式服务和应用程序访问不应有访问的操作系统的关键领域。

## <a name="BKMK_LSA"></a>本地安全颁发机构
本地安全颁发机构 (LSA) 是受保护的系统进程进行身份验证，并登录到本地计算机用户。 此外，LSA 保留的所有方面的信息本地安全于一台电脑（统称为与本地安全策略已知这些方面），并提供各种名称和安全标识符 (Sid) 之间翻译服务。 跟踪的安全系统进程，本地安全颁发机构服务器服务 (LSASS)，安全策略，实际上是在计算机的系统的帐户。

LSA 验证根据其以下两个实体发出的用户帐户的用户的身份：

-   **本地安全颁发机构。** LSA 可以通过检查位于同一台计算机安全帐户管理器（三千）数据库验证用户信息。 任何工作站或成员服务器可以存储本地用户帐户和本地组有关的信息。 但是，这些帐户可用于访问该工作站或计算机。

-   **安全颁发机构地域或某个受信任的域。** LSA 联系人实体颁发该帐户并请求验证该帐户有效，并且该帐户持有人发出请求。

本地安全颁发机构子系统服务 (LSASS) 代表使用活动 Windows 会话用户在内存中存储的凭据。 存储的凭据让用户无缝访问网络资源，如文件共享、Exchange Server 邮箱和 SharePoint 网站，而无需重新输入它们为每个远程服务的凭据。

LSASS 可以存储凭据多个形式，其中包括：

-   可逆加密纯文本

-   Kerberos 门票（票证许可门票 (Tgt)，服务门票）

-   NT 希

-   LAN 管理器 (LM) 希

如果用户登录到 Windows 通过一张智能卡，LSASS 将不会储存纯文本密码，但它存储的帐户和智能卡的 pin 码将纯文本相应 NT 希值。 如果为一张智能卡所需的交互式登录启用了帐户属性，而不是原始密码哈希帐户自动生成随机 NT 希值。 设置了属性时自动生成密码希不变。

如果用户登录到基于 Windows 的计算机用 LAN 管理器 (LM) 哈希与兼容的密码，该 authenticator 随后将显示在内存中。

无法禁用纯文本凭据在内存中存储，即使要求他们的凭据提供被禁用。

存储的凭据直接与已启动后最后在本地安全颁发机构子系统服务 (LSASS) 登录会话重新启动并不已关闭。 例如，用户可以执行以下任一时创建存储 LSA 凭据 LSA 会议：

-   登录到会话本地或远程桌面协议 (RDP) 的计算机上的会话

-   通过运行任务**RunAs**选项

-   在计算机上运行活动的 Windows 服务

-   运行计划的任务或批量作业

-   使用远程管理工具本地计算机上运行一项任务

在某些情况下，LSA 机密，即机密条只有访问系统帐户进程的数据存储在硬盘驱动器上。 一些这些机密信息是必须重新引导后，将保留的凭据，它们在硬盘上的加密的形式存储。 作为 LSA 机密存储凭据情况可能包括：

-   计算机的 Active Directory 域服务 (广告 DS) 帐户帐户密码

-   帐户密码，为配置电脑的 Windows 服务的

-   配置计划任务的帐户密码

-   帐户 IIS 应用程序池和网站的密码

-   Microsoft 帐户的密码

引入 Windows 8.1 中，客户端操作系统提供更多保护以防止阅读内存和代码由不受保护的进程插入 LSA。 此保护会增加安全 LSA 存储，并管理的凭据。

有关这些其他保护的详细信息，请参阅[配置其他 LSA 防护](../credentials-protection-and-management/configuring-additional-lsa-protection.md)。

## <a name="BKMK_CachedCredentialsAndValidation"></a>缓存的凭据和验证
验证机制依赖的凭据登录次演示文稿上。 但是，当计算机断开域控制器，并且用户呈现域凭据时，Windows 将验证机制使用缓存的凭据的过程。

每次用户登录到某个域，Windows 缓存提供的凭据，并将其存储在注册表中的安全配置单元的操作系统。

使用缓存的凭据，用户可以域成员不登录未连接到域控制器该域内。


## <a name="BKMK_CredentialStorageAndValidation"></a>凭据存储和验证
它并不总是希望使用一组凭据以访问不同的资源。 例如，管理员可能会想要使用而不是用户凭据管理访问远程服务器时。 同样，如果用户访问外部资源，例如银行帐户，他或她可以仅使用其域凭据不同的凭据。 以下部分介绍凭据管理之间当前版本的 Windows 操作系统和 Windows Vista 和 Windows XP 操作系统的差异。

### <a name="remote-logon-credential-processes"></a>远程的登录凭据进程
远程桌面协议 (RDP) 管理的用户使用远程桌面客户端，这在 Windows 8 中引入来连接到远程计算机的凭据。 凭据纯文本的形式发送给目标主机其中主机尝试执行身份验证过程，并且，如果成功，将用户连接到允许资源。 RDP 不会存储在客户端的凭据，但存储在 LSASS 用户的域的凭据。

引入 Windows Server 2012 R2 和 Windows 8.1 中，受限管理员模式提供额外的安全于远程登录的情况。 这种远程桌面模式会导致执行 NT 单向函数 (NTOWF) 与网络登录质询响应或进行身份验证的远程主机时，使用 Kerberos 服务票证客户端应用程序。 管理员身份验证后，管理员各自的帐户凭据中没有 LSASS 因为他们未提供远程主机。 而是管理员可以在为会话计算机帐户凭据。 不到远程主机上，提供管理员凭据，以便为计算机的帐户执行的操作。 资源也是仅限于计算机帐户，并且管理员无法访问具有自己的帐户的资源。

### <a name="automatic-restart-sign-on-credential-process"></a>自动重启的登录凭据进程
当用户在 Windows 8.1 设备上登录时，LSA 将用户凭据加密内存中的只能由 LSASS.exe 访问。 Windows 更新启动时自动重新启动不用户状态，这些凭据用于为用户配置自动登录。

重新启动时，用户自动登录通过自动登录机制，并计算机此外锁定保护用户会话。 锁定启动通过 Winlogon 而凭据管理通过 LSA。 通过自动登录，并在主机上锁定用户会话，该用户的锁屏界面应用程序是重新启动和可用。

有关 ARSO 的详细信息，请参阅[Winlogon 自动重启登录 & #40;ARSO & #41;](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>存储的用户名和密码，在 Windows Vista 和 Windows XP
在 Windows Server 2008、Windows Server 2003、Windows Vista 和 Windows XP**存储用户名和密码**控制面板中简化了的管理和使用的登录凭据，包括用于智能卡 X.509 证书和 Windows Live 的凭据（现在更名为 Microsoft 帐户）的多个设置。 在需要时才存储的一部分的用户的个人资料-的凭据。 此操作会增加安全基于每个资源确保，如果一个密码受到威胁后，它不会危及所有安全。

用户登录并尝试访问其他的受密码保护的资源，如在服务器上，共享后，并且没有足够获取访问权限，该用户的默认的登录凭据**存储用户名和密码**查询。 如果显示正确的登录信息的凭据备用保存在**存储用户名和密码**，使用这些凭据以访问。 否则，提示用户提供新的凭据，然后均可保存供以后在登录会话中或后续会话期间重复使用。

以下限制：

-   如果**存储用户名和密码**有关特定资源，访问资源被拒绝，包含无效或不正确的凭据和**存储用户名和密码**未显示对话框。

-   **用户名和密码存储**仅用于 NTLM、存储凭据 Kerberos 协议、Microsoft 帐户 (原名为 Windows Live ID) 和安全套接字层 (SSL) 身份验证。 某些版本的 Internet Explorer 维护基本身份验证自己缓存。

这些凭据成为 \Documents 和 Settings\Username\Application Data\ Microsoft \Credentials 目录中的用户的本地配置文件的加密的组成部分。 因此，这些凭据可以将随用户漫游如果用户的网络策略的支持漫游用户配置文件。 但是，如果用户副本**存储用户名和密码**在两种不同的计算机和更改在其中一台计算机，资源与相关联的凭据更改不会传播到**存储用户名和密码**第二台计算机上。

### <a name="windows-vault-and-credential-manager"></a>Windows 圆顶和凭据管理器
作为应用商店和管理用户名和密码一控制面板项功能在 Windows Server 2008 R2 和 Windows 7 引入了凭据管理器。 凭据管理器可让用户在安全的 Windows 圆顶存储相关的其他系统和网站的凭据。 某些版本的 Internet Explorer 的访问网站的身份验证使用此功能。

通过使用凭据管理器的凭据管理受用户在本地计算机上。 用户可以存储和存储凭据的受支持的浏览器和 Windows 应用程序，以使其方便需要时，他们登录到这些资源。 凭据下的用户配置文件的计算机上保存的特殊加密的文件夹中。 应用程序支持（通过使用凭据经理 Api)，如浏览 web 和应用，该功能可以登录过程出示正确到其他计算机和网站的凭据。

当某个网站、应用程序或通过 NTLM 或 Kerberos 协议的其他计算机请求身份验证，对话框中显示在你选择的**更新默认凭据**或**保存密码**复选框。 此对话框中，可让用户保存凭据本地生成的应用程序支持凭据管理器 Api。 如果用户选择**保存密码**复选框，用户用户名、密码、正在使用的身份验证服务用于和相关信息，则会持续跟踪的凭据管理器。

下一次使用该服务，则凭据管理器自动提供存储在 Windows 圆顶凭据。 如果它未被接受，提示用户正确的访问权限的信息。 使用新的凭据授予访问权限时，如果凭据管理器的新覆盖以前凭据并将存储新凭据 Windows 圆顶中。

## <a name="BKMK_SAM"></a>安全帐户管理器的数据库
安全帐户管理器（三千）将存储在本地用户帐户和组的数据库。 它是存在于 Windows 的每个操作系统;但是，计算机已加入域，当 Active Directory 管理的域的 Active Directory 中域帐户。

例如，客户端计算机运行的是 Windows 操作系统加入了网络域通过与域控制器进行通信，即使没有人工用户登录到此计算机。 若要启动通信，的计算机必须具有域中的活动的帐户。 之前接受从计算机的通信，域控制器上的 LSA 验证身份的计算机，然后像它人工安全主体构造计算机安全环境。 此安全上下文定义特定计算机或用户、服务或在网络上的计算机上身份和功能的用户或服务。 例如，包含安全环境中访问令牌定义可以访问的资源（如共享文件或打印机）和由该用户的用户、的电脑或服务上的操作（如阅读、写入或修改）资源。

用户或计算机的安全上下文可以从一台计算机有所不同，对其他用户在登录时服务器或之外的用户的主要工作站工作站到如。 它还可以为另一，例如当管理员修改用户的权利和权限从一个会话变化。 此外，安全上下文时通常不同的用户或计算机运行单独的方式，在网络中，或 Active Directory 域的一部分。

## <a name="BKMK_LocalDomainsAndTrustedDomains"></a>本地域和受信任的域
在两个域之间存在信任，为每个域身份验证机制依赖于来自其他域身份验证的有效性。 信任帮助通过验证该传入的身份验证请求提供资源域（信任）中共享资源的控制的访问来自受信任的授权（受信任的域）。 这种方式，只允许的桥验证身份验证请求旅行域之间充当信任。

如何特定信任传递身份验证请求取决于配置的方式。 信任关系可，通过提供来自受信任的域中访问信任的域中，在资源双向，通过提供的每个域访问其他域中的资源。 信任也是非传递，这种情况下，仅之间两个信任合作伙伴域，存在信任或传递，这种情况下信任自动延伸到任何其他合作伙伴任一信任的域。

有关域和森林信任与您的关系有关身份验证信息，请参阅[委派身份验证和信任关系](https://technet.microsoft.com/library/dn169022.aspx)。

## <a name="BKMK_CertificatesInWindowsAuthentication"></a>在 Windows 身份验证的证书
公共基础结构 (PKI) 都是软件、加密技术、进程，并启用组织安全其通信和企业交易的服务的组合。 PKI 能够安全通信和企业交易基于交换身份验证的用户和受信任的资源之间的数字证书。

数字证书是一种电子的文档，其中包含有关实体它属于、它由颁发实体、唯一的序列号或某些其他唯一标识、颁发和到期日期和数字的指纹的信息。

验证是确定远程主机可以信任的过程。 若要建立其可靠性，远程主机必须提供接受身份验证证书。

远程主机建立它们可以向您获得从认证颁发机构证书。 CA 反过来，可以创建信任一系列的更高版本颁发机构颁发。 若要确定是否证书值得的应用必须确定根加拿大的身份，并且然后确定是否可靠。

同样，远程主机或本地计算机必须确定证书呈现用户或应用程序是否为正版。 通过 LSA 和 SSPI 用户提供的证书计算通过 Active Directory 中的证书应用商店的本地登录在本地计算机上，该网络，或域的真实性。

来繁衍证书、身份验证数据将通过哈希算法，例如安全哈希算法 1 (SHA1)，以生成消息摘要。 消息摘要进行数字签名通过发件人专用密钥证明消息摘要产生发件人。

> [!NOTE]
> SHA1 是 Windows 7 和 Windows Vista 中的默认值，但已更改为 SHA2 Windows 8 中。

**智能卡身份验证**

智能卡技术是证书基于身份验证的示例。 在登录到的一张智能卡网络提供了很强的身份验证因为它使用基于加密识别和所有权证明时验证到某个域中的用户。 Active Directory 证书服务（广告客户服务）提供了加密基于标识通过发行的每个智能卡登录的证书。

智能卡身份验证信息，请参阅[Windows 智能卡技术参考](https://technet.microsoft.com/library/ff404297.aspx)。

Windows 8 中引入了虚拟智能卡技术。 它将智能卡证书存储在电脑上，并且然后将其保护通过使用设备的篡改受信任的平台模块 (TPM) 安全芯片。 在这样一来，电脑实际上成为智能卡必须以进行身份验证来接收该用户的 PIN。

**远程和无线身份验证**

远程和无线网络身份验证是另一种用于身份验证证书的技术。 Internet 身份验证服务 (IAS) 和虚拟专用网络服务器使用扩展验证协议-传输层安全性 (EAP-TLS)，保护可扩展身份验证协议 (PEAP)，或者 Internet 协议安全执行证书基于身份验证多种类型的网络访问权限，包括虚拟专用网络 (VPN) 和无线连接。

在网络中的证书基于身份验证信息，请参阅[网络的访问权限身份验证和证书](https://technet.microsoft.com/library/cc759575(WS.10).aspx)。

## <a name="BKMK_SeeAlso"></a>请参阅
[Windows 身份验证的概念](https://technet.microsoft.com/library/d169018.aspx)

