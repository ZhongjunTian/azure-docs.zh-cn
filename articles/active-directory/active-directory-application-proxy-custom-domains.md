---
title: "Azure AD 应用程序代理中的自定义域 | Microsoft 文档"
description: "管理 Azure AD 应用程序代理中的自定义域，使得无论用户在哪里访问应用，应用的 URL 都相同。"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/13/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 3004e694a8ea8cadf622ffeacf00f2b1ff9e550a
ms.contentlocale: zh-cn
ms.lasthandoff: 05/17/2017


---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>使用 Azure AD 应用程序代理中的自定义域
通过使用默认域，你能够将相同的 URL 设置为用于访问应用程序的内部和外部 URL，以便不管从哪里访问，你的用户只有一个需要记住来访问应用的 URL。 并且还可让你在应用程序的访问面板中创建一个快捷方式。 如果使用 Azure AD 应用程序代理提供的默认域，则无需额外配置即可启用你的域。 如果使用自定义域，则需要执行几个操作，以确保应用程序代理能够识别你的域并验证其证书。

## <a name="select-your-custom-domain"></a>选择自定义域
1. 根据[使用应用程序代理发布应用程序](active-directory-application-proxy-publish.md)中的说明发布应用程序。
2. 应用程序显示在应用程序列表中之后，选择该应用程序并单击“配置”。
3. 在“外部 URL”下，输入自定义域。
4. 如果外部 URL 是 https，系统将提示上传证书，以便 Azure 可以验证应用程序的 URL。 还可以上载与应用程序的外部 URL 匹配的通配符证书。 此域必须位于 [Azure 已验证域](https://msdn.microsoft.com/library/azure/jj151788.aspx)列表中。 Azure 必须具有应用程序的域 URL 或与应用程序的外部 URL 匹配的通配符证书。
5. 添加将内部 URL 路由到应用程序的 DNS 记录。 使用此记录，可以将同一 URL 用于对该应用的内部访问和外部访问，并且用户的应用程序列表中只会存在一个快捷方式。

## <a name="frequently-asked-questions"></a>常见问题
**问：无需再次上传的情况下，是否可以选择已上传的证书？**  
答：以前上载的证书已自动绑定到应用程序，并且只有一个证书匹配应用程序的主机名称。  

**问：如何添加证书，以及导出的证书应以何种格式上传？**  
答：应从应用程序配置页中上载证书。 证书应为 PFX 文件。  

**问：是否可以使用 ECC 证书？**  
答：对于签名方法没有明确的限制。  

**问：是否可以使用 SAN 证书？**  
答：是的。  

**问：是否可以使用通配符证书？**  
答：是的。  

**问：是否可以在每个应用程序上使用不同的证书？**  
答：可以，除非这两个应用程序共享相同的外部主机。  

**问：如果注册新的域，是否可以使用该域？**  
答：可以。域列表来源于租户的已验证域列表。  

**问：当证书过期时会发生什么情况？**  
答：会在应用程序配置页的证书部分中收到一条警告。 当用户尝试访问该应用程序时，将弹出一条安全警告。  

**问：应该如何替换指定应用的证书？**  
答：从应用程序配置页上载新证书。  

**问：是否可以删除并替换证书？**  
答：上传新证书时，如果旧证书未被其他应用程序使用，那么将自动删除该证书。  

**问：当证书被吊销时会发生什么情况？**  
答：不会对证书执行吊销检查。 当用户尝试访问应用程序时，根据所使用的浏览器，可能会出现安全警告。  

**问：是否可以使用自签名证书？**  
答：可以。允许使用自签名的证书。 如果使用的是私有证书颁发机构，那么证书的 CDP（证书吊销点分发点）应该是公共的。  

**问：是否有地方可以查看我的租户的所有证书？**  
答：当前版本中不支持。  

## <a name="next-steps"></a>后续步骤
* 使用 Azure AD 身份验证对已发布应用[启用单一登录](active-directory-application-proxy-sso-using-kcd.md)。
* 对已发布应用[启用条件访问](active-directory-application-proxy-conditional-access.md)。
* [将自定义域名添加到 Azure AD](active-directory-add-domain.md)



