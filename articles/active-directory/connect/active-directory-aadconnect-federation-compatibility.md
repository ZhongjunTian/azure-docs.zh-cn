---
title: "Azure AD 联合身份验证兼容性列表"
description: "本页列出了可用于实现单一登录的非 Microsoft 标识提供者。"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 22c8693e-8915-446d-b383-27e9587988ec
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 6f91f732b7579c9f14458dab9be49c027debfac1
ms.contentlocale: zh-cn
ms.lasthandoff: 05/02/2017


---
# <a name="azure-ad-federation-compatibility-list"></a>Azure AD 联合身份验证兼容性列表
Azure Active Directory 为 Office 365 和其他 Microsoft Online 服务提供单一登录与增强的应用程序访问安全性，以便在不使用任何非 Microsoft 解决方案的情况下实施混合部署和仅限云的部署。 与大多数 Microsoft Online 服务一样，Office 365 可与 Azure Active Directory 集成，以利用目录服务、身份验证和授权。 Azure Active Directory 还为数千种 SaaS 应用程序与本地 Web 应用程序提供单一登录。 有关支持的 SaaS 应用程序，请参阅 Azure Active Directory 应用程序库。

对于投资了非 Microsoft 联合解决方案的组织，本主题包含有关通过以下“Azure Active Directory 联合兼容性列表”中所列的非 Microsoft 标识提供者，为使用 Microsoft 联机服务的 Windows Server Active Directory 用户配置单一登录的指导。 

![](./media/active-directory-aadconnect-federation-compatibility/oxford2.jpg)   
[Oxford Computer Group](http://oxfordcomputergroup.com/) 作为代表 Microsoft 的第三方，利用非 Microsoft 标识提供者针对 Azure Active Directory 的一组常见用例测试了这些单一登录体验。

有关如何获取此处列出的第三方标识提供者的信息，请通过 [idp@oxfordcomputergroup.com](mailto:idp@oxfordcomputergroup.com) 与 Oxford Computer Group 联系。

> [!IMPORTANT]
> Oxford Computer Group 仅测试了这些单一登录方案的联合功能。 Oxford Computer Group 未对这些单一登录方案的同步、双重身份验证等组件执行测试。
> 
> 按备用 ID 和 UPN 使用登录也未在此计划中测试。
> 
> 

* [Azure Active Directory](#azure-active-directory)
* [AuthAnvil Single Sign On 4.5](#authanvil-single-sign-on-45)
* [BIG-IP with Access Policy Manager BIG-IP ver.11.3x – 11.6x](#big-ip-with-access-policy-manager-big-ip-ver-113x--116x) 
* [BitGlass](#bitglass)
* [CA Secure Cloud](#ca-secure-cloud) 
* [CA SiteMinder 12.52](#ca-siteminder-1252-sp1-cumulative-release-4) 
* [Centrify](#centrify) 
* [Dell One Identity Cloud Access Manager v7.1](#dell-one-identity-cloud-access-manager-v71) 
* [IBM Tivoli Federated Identity Manager 6.2.2](#ibm-tivoli-federated-identity-manager-622) 
* [IceWall Federation Version 3.0](#icewall-federation-version-30) 
* [Memority](#memority)
* [NetIQ Access Manager 4.x](#netiq-access-manager-4x) 
* [Okta](#okta) 
* [OneLogin](#onelogin) 
* [Optimal IDM Virtual Identity Server Federation Services](#optimal-idm-virtual-identity-server-federation-services) 
* [PingFederate 6.11, 7.2, 8.x](#pingfederate-611-72-8x)
* [RadiantOne CFS 3.0](#radiantone-cfs-30) 
* [Sailpoint IdentityNow](#sailpoint-identitynow)
* [SecureAuth IdP 7.2.0](#secureauth-idp-720) 
* [Sign&go 5.3](#signgo-53) 
* [SoftBank Technology Online Service Gate](#softbank)
* [VMware Workspace One](#vmware-workspace-one)
* [VMware  Workspace Portal version 2.1](#vmware--workspace-portal-version-21) 


> [!IMPORTANT]
> 由于这些是第三方产品，Microsoft 不会对与这些标识提供者相关的问题和疑问提供支持，例如部署、配置、故障排除、最佳实践等方面的问题和疑问。 如果需要获得支持或者存在有关这些标识提供者的疑问，请直接联系提供支持的第三方。
> 
> 仅使用 WS 联合和 WS 信任协议测试这些第三方标识提供者与 Microsoft 云服务的互操作性。 测试不包括使用 SAML 协议。
> 


## <a name="azure-active-directory"></a>Azure Active Directory

下面是此登录体验的方案支持对照表： 

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |无 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |无 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |
| 使用 ADAL 的现代应用程序，例如 Office 2016 |支持 |无 |

有关将 Azure Active Directory 与 AD FS 配合使用的详细信息，请参阅 [Active Directory 联合身份验证服务 (ADFS)](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)

有关将 Azure Active Directory 与密码同步配合使用的详细信息，请参阅 [Azure AD Connect](active-directory-aadconnect.md)。

## <a name="authanvil-single-sign-on-45"></a>AuthAnvil Single Sign On 4.5

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |不支持集成 Windows 身份验证 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |不支持集成 Windows 身份验证 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关详细信息，请参阅 [AuthAnvil 单一登录](https://help.scorpionsoft.com/entries/26538603-How-can-I-Configure-Single-Sign-On-for-Office-365-)。


## <a name="big-ip-with-access-policy-manager-big-ip-ver-113x--116x"></a>BIG-IP with Access Policy Manager BIG-IP ver. 11.3x – 11.6x

下面是此单一登录体验的方案支持对照表： 

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |无 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |不支持 |不支持 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 BIG-IP Access Policy Manager 的详细信息，请参阅 [BIG-IP Access Policy Manager](https://f5.com/products/modules/access-policy-manager)。 

有关如何将 STS 配置为向 Active Directory 用户提供单一登录体验的 BIG-IP Access Policy Manager 说明，请从[此处](http://www.f5.com/pdf/deployment-guides/microsoft-office-365-idp-dg.pdf)下载 PDF 文件。

## <a name="bitglass"></a>BitGlass

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |不支持集成 Windows 身份验证 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |不支持集成 Windows 身份验证 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 BitGlass 的详细信息，请查看[此处](http://www.bitglass.com )。

## <a name="ca-secure-cloud"></a>CA Secure Cloud

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |不支持集成 Windows 身份验证 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |不支持集成 Windows 身份验证 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 CA Secure Cloud 详细信息，请参阅 [CA Secure Cloud](http://www.ca.com/us/products/security-as-a-service.aspx)。

## <a name="ca-siteminder-1252-sp1-cumulative-release-4"></a>CA SiteMinder 12.52 SP1 Cumulative Release 4

下面是此单一登录体验的方案支持对照表： 

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |无 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |无 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 CA SiteMinder 详细信息，请参阅 [CA SiteMinder Federation。](http://www.ca.com/us/products/ca-single-sign-on.html) 

## <a name="centrify"></a>Centrify

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |无 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |无 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |不支持客户端访问控制 |

有关 Centrify 的详细信息，请查看[此处。](http://www.centrify.com/cloud/apps/single-sign-on-for-office-365.asp)|

## <a name="dell-one-identity-cloud-access-manager-v71"></a>Dell One Identity Cloud Access Manager v7.1

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |无 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |无 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 Dell One Identity Cloud Access Manager 的详细信息，请参阅 [Dell One Identity Cloud Access Manager。](http://software.dell.com/products/cloud-access-manager)

 有关如何将此 STS 配置为向 Office 365 用户提供单一登录体验的说明，请参阅[配置 Office 365 用户](http://documents.software.dell.com/dell-one-identity-cloud-access-manager/7.1/how-to-configure-microsoft-office-365)。 

## <a name="ibm-tivoli-federated-identity-manager-622"></a>IBM Tivoli Federated Identity Manager 6.2.2

下面是此单一登录体验的方案支持对照表： 

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |无 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |无 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 IBM Tivoli Federated Identity Manager 的详细信息，请参阅 [Microsoft 应用程序的 IBM 安全访问管理器](http://www-01.ibm.com/support/docview.wss?uid=swg24029517)。

## <a name="icewall-federation-version-30"></a>IceWall Federation Version 3.0

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |不支持集成 Windows 身份验证 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |不支持集成 Windows 身份验证 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 IceWall Federation 的详细信息，请参阅[此处](http://h50146.www5.hp.com/products/software/security/icewall/eng/federation/)和[此处。](http://h50146.www5.hp.com/products/software/security/icewall/federation/office365.html)

## <a name="memority"></a>Memority

下面是此登录体验的方案支持对照表： 

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |无 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |无 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关使用 Memority 的详细信息，请参阅 [Memority](http://www.memority.com)


## <a name="netiq-access-manager-4x"></a>NetIQ Access Manager 4.x

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |无|
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |无|
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关详细信息，请参阅 [NetIQ Access Manager](https://www.netiq.com/documentation/access-manager-43/admin/data/b65ogn0.html#b12iqp0m)

## <a name="okta"></a>Okta

下面是此单一登录体验的方案支持对照表： 

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |Windows 集成身份验证需要增设 Web 服务器和 Okta 应用程序。 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |Windows 集成身份验证 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 Okta 的详细信息，请参阅 [Okta](https://www.okta.com/)。

## <a name="onelogin"></a>OneLogin

下面是此单一登录体验的方案支持对照表： 

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |Windows 集成身份验证 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |Windows 集成身份验证 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 OneLogin 的详细信息，请参阅 [OneLogin。](https://www.onelogin.com/)

## <a name="optimal-idm-virtual-identity-server-federation-services"></a>Optimal IDM Virtual Identity Server Federation Services

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |无 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |Windows 集成身份验证 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |

有关客户端访问策略的详细信息，请参阅[基于客户端位置限制对 Office 365 服务的访问权限](https://technet.microsoft.com/library/hh526961.aspx) |。





## <a name="pingfederate-611-72-8x"></a>PingFederate 6.11, 7.2, 8.x

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |无 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |无 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关如何将此 STS 配置为向 Active Directory 用户提供单一登录体验的 PingFederate 说明，请参阅以下文章之一： 

- [PingFederate 6.11](http://go.microsoft.com/fwlink/?LinkID=266321)
- [PingFederate 7.2](http://documentation.pingidentity.com/display/PF72/PingFederate+7.2)
- [PingFederate 8.x](http://documentation.pingidentity.com/display/PFS/SSO+to+Office+365+Introduction)

## <a name="radiantone-cfs-30"></a>RadiantOne CFS 3.0

下面是此单一登录体验的方案支持对照表： 

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |无 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |Windows 集成身份验证 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 RadiantOne CFS 的详细信息，请参阅 [RadiantOne CFS。](http://www.radiantlogic.com/products/radiantone-cfs/)

## <a name="sailpoint-identitynow"></a>Sailpoint IdentityNow

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |不支持集成 Windows 身份验证 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |不支持集成 Windows 身份验证 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关详细信息，请参阅 [Sailpoint IdentityNow](https://www.sailpoint.com/idaas-identity-as-a-service-identitynow/)

## <a name="secureauth-idp-720"></a>SecureAuth IdP 7.2.0

下面是此单一登录体验的方案支持对照表： 

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |无 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |无 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 SecureAuth 的详细信息，请参阅 [SecureAuth IdP](http://go.microsoft.com/?linkid=9845293)。














## <a name="signgo-53"></a>Sign&go 5.3

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |支持 Kerberos 约定 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |无 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

Sign&go 5.3 支持通过配置 Kerberos 约定实现 Kerberos 身份验证。  如需此配置的帮助，请联系 Ilex 或查看[此处](http://www.ilex-international.com/docs/sign&go_wsfederation_en.pdf)的设置指南。

## <a name="softbank-technology-online-service-gate"></a>SoftBank Technology Online Service Gate

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |不支持集成 Windows 身份验证 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |不支持集成 Windows 身份验证 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 SoftBank Technology Online Service Gate 的详细信息，请参阅[此处](https://www.softbanktech.jp/service/list/osg-pro-ent/)。

## <a name="vmware-workspace-one"></a>VMware Workspace One

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |不支持集成 Windows 身份验证 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |不支持集成 Windows 身份验证 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关详细信息，请查看[此处](http://www.vmware.com/pdf/vidm-office365-saml.pdf)。

## <a name="vmware--workspace-portal-version-21"></a>VMware  Workspace Portal version 2.1

下面是此单一登录体验的方案支持对照表：

| 客户端 | 支持 | 异常 |
| --- | --- | --- |
| 基于 Web 的客户端（如 Exchange Web Access 和 SharePoint Online） |支持 |不支持集成 Windows 身份验证 |
| 富客户端应用程序（如 Lync、Office Subscription、CRM） |支持 |不支持集成 Windows 身份验证 |
| 多重格式电子邮件客户端（如 Outlook 和 ActiveSync） |支持 |无 |

有关 VMware  Workspace Portal version 2.1 的详细信息，请从[此处](http://pubs.vmware.com/workspace-portal-21/topic/com.vmware.ICbase/PDF/workspace-portal-21-resource.pdf)下载 PDF 文件。
