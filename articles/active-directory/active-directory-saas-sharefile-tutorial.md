---
title: "教程：Azure Active Directory 与 Citrix ShareFile 的集成 | Microsoft Docs"
description: "了解如何在 Azure Active Directory 和 Citrix ShareFile 之间配置单一登录。"
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ddf6c69b-4a76-4b42-8619-6f2a22800a23
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 9dfbf4bcdcd580773a368b90c869bf9c4fefbd77
ms.lasthandoff: 03/25/2017


---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>教程：Azure Active Directory 与 Citrix ShareFile 的集成

本教程介绍如何将 Citrix ShareFile 与 Azure Active Directory (Azure AD) 集成。

将 Citrix ShareFile 与 Azure AD 集成具有以下优势：

- 可以在 Azure AD 中控制谁有权访问 Citrix ShareFile
- 可以让用户使用其 Azure AD 帐户自动登录到 Citrix ShareFile（单一登录）
- 可在一个中心位置（即 Azure 管理门户）管理帐户

如果要了解有关 SaaS 应用与 Azure AD 集成的更多详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](active-directory-appssoaccess-whatis.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Citrix ShareFile 的集成，需备齐以下项目：

- 一个 Azure AD 订阅
- 启用了 Citrix ShareFile 单一登录的订阅


> [!NOTE]
> 不建议使用生产环境测试本教程中的步骤。


测试本教程中的步骤应遵循以下建议：

- 不应使用生产环境，除非有此必要。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。


## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库添加 Citrix ShareFile
2. 配置和测试 Azure AD 单一登录


## <a name="adding-citrix-sharefile-from-the-gallery"></a>从库添加 Citrix ShareFile
若要配置 Citrix ShareFile 与 Azure AD 的集成，需要从库中将 Citrix ShareFile 添加到托管 SaaS 应用列表。

**若要从库中添加 Citrix ShareFile，请执行以下步骤：**

1. 在 **[Azure 管理门户](https://portal.azure.com)**的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
3. 单击对话框顶部的“添加”按钮。

    ![应用程序][3]

4. 在搜索框中，键入 **Citrix ShareFile**。

    ![创建 Azure AD 测试用户](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_001.png)

5. 在结果窗格中选择“Citrix ShareFile”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置并测试 Azure AD 单一登录
在本部分中，基于一个名为“Britta Simon”的测试用户使用 Citrix ShareFile 配置和测试 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Citrix ShareFile 用户。 换句话说，需要建立 Azure AD 用户与 Citrix ShareFile 中相关用户之间的链接关系。

可以通过将 Azure AD 中“用户名”的值分配为 Citrix ShareFile 中“用户名”的值来建立此链接关系。

若要配置并测试 Citrix ShareFile 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户能够使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 Citrix ShareFile 测试用户](#creating-a-citrix-sharefile-test-user)** - 在 Citrix ShareFile 中创建 Britta Simon 的对应者，链接到她的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 能够使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

本部分的步骤将在 Azure 管理门户中启用 Azure AD 单一登录并在 Citrix ShareFile 应用程序中配置单一登录。

**若要配置 Citrix ShareFile 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 管理门户的“Citrix ShareFile”应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

2. 在“单一登录”对话框中，选择“基于 SAML 的登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_01.png)

3. 在“Citrix ShareFile 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_02.png)

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 在“登录 URL”文本框中，使用以下模式键入 URL：`https://<tenant-name>.sharefile.com/saml/login`    

    > [!NOTE] 
    > 请注意，这些不是实际值。 必须使用实际登录 URL 和标识符更新这些值。 如果你不知道这些 URL，请键入使用示例模式的示例 URL，完成步骤 13，你将获取实际 URL。

4. 在“SAML 签名证书”部分中，单击“证书(base64)”，然后在计算机上保存证书文件。

    ![配置单一登录](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_05.png) 

5. 在“Citrix ShareFile 配置”部分中，单击“配置 Citrix ShareFile”打开“配置登录”窗口。

    ![配置单一登录](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_06.png) 

    ![配置单一登录](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_07.png)

6. 在另一个 Web 浏览器窗口中，以管理员身份登录到你的 Citrix ShareFile 公司站点。

7. 在顶部工具栏中，单击“管理”。

8. 在左侧导航窗格中，选择“配置单一登录”。

    ![在应用端配置单一登录](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_002.png)

9. 在“单一登录/ SAML 2.0 配置”对话框页上，在“基本设置”下执行以下步骤：

    ![在应用端配置单一登录](./media/active-directory-saas-sharefile-tutorial/sso_configuration_2.png)

    a. 单击“启用 SAML”。

    b. 在“IDP 颁发者/实体 ID”文本框中，输入 Azure AD 应用程序配置窗口中“SAML 实体 ID”的值。

    c. 单击“X.509 证书”字段旁边的“更改”，然后上载已下载的证书文件。 

    d.单击“下一步”。 在“登录 URL”文本框中，放置 Azure AD 应用程序配置窗口中“SAML 单一登录服务 URL”的值。

    e.在“新建 MySQL 数据库”边栏选项卡中，接受法律条款，然后单击“确定”。 在“注销 URL”文本框中，输入 Azure AD 应用程序配置窗口中“注销 URL”的值。

10. 在 Citrix ShareFile 管理门户中单击“保存”。
    
 

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 管理门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 管理门户的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/active-directory-saas-sharefile-tutorial/create_aaduser_01.png) 

2. 转到“用户和组”，单击“所有用户”显示用户列表。
    
    ![创建 Azure AD 测试用户](./media/active-directory-saas-sharefile-tutorial/create_aaduser_02.png) 

3. 在对话框顶部单击“添加”，打开“用户”对话框。
 
    ![创建 Azure AD 测试用户](./media/active-directory-saas-sharefile-tutorial/create_aaduser_03.png) 

4. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/active-directory-saas-sharefile-tutorial/create_aaduser_04.png) 

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 在“名称”文本框中，键入 **BrittaSimon**。

    b.保留“数据库类型”设置，即设置为“共享”。 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d.单击“下一步”。 单击“创建”。 



### <a name="creating-a-citrix-sharefile-test-user"></a>创建 Citrix ShareFile 测试用户

为了使 Azure AD 用户能够登录到 Citrix ShareFile，必须将其预配到 Citrix ShareFile 中。 对于 Citrix ShareFile，需要手动执行预配。

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>若要预配用户帐户，请执行以下步骤：

1. 以管理员身份登录到 Citrix ShareFile 公司站点。

2. 在顶部工具栏中，单击“管理用户”。 然后转到“管理用户主页”并单击“创建员工”。

    ![新建用户](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_010.png "New User")

3. 在“基本信息”部分中执行以下步骤：

    ![新建用户](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_011.png "New User")

    a.在“解决方案资源管理器”中，右键单击项目文件夹下的“引用”文件夹，然后单击“添加引用”。 在“电子邮件地址”文本框中，键入 Britta Simon 帐户的电子邮件地址。  

    b.保留“数据库类型”设置，即设置为“共享”。 在“名字”文本框中，键入“Britta”。  

    c. 在“姓氏”文本框中，键入“Simon”。

4. 单击“添加用户”。

    > [!NOTE]
    > AAD 帐户持有者将收到一封电子邮件，并打开用于在激活帐户前确认其帐户的链接。 可以使用 Citrix ShareFile 提供的任何其他 Citrix ShareFile 用户帐户创建工具或 API 来预配 AAD 用户帐户。



### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，将通过向 Britta Simon 授予对 Citrix ShareFile 的访问权限使她能够使用 Azure 单一登录。

![分配用户][200] 

**若要将 Britta Simon 分配到 Citrix ShareFile，请执行以下步骤：**

1. 在 Azure 管理门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，然后单击“所有应用程序”。

    ![分配用户][201] 

2. 在应用程序列表中，选择“Citrix ShareFile”。

    ![配置单一登录](./media/active-directory-saas-sharefile-tutorial/tutorial_sharefile_50.png) 

3. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

4. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

5. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

6. 在“用户和组”对话框中单击“选择”按钮。

7. 在“添加分配”对话框中单击“分配”按钮。
    


### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 Citrix ShareFile 磁贴时，你应自动登录到 Citrix ShareFile 应用程序。


## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](active-directory-saas-tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sharefile-tutorial/tutorial_general_203.png
