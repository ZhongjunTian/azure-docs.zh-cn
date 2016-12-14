---
title: "流分析的上的 Power BI 仪表板 | Microsoft 文档"
description: "使用实时流式处理 Power BI 仪表板来采集商业智能信息，并分析流分析作业中的大量数据。"
keywords: "分析仪表板, 实时仪表板"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: fe8db732-4397-4e58-9313-fec9537aa2ad
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 09/26/2016
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: c954e85aa61803b4afe82c2e9d30729035ac094f


---
# <a name="stream-analytics-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>流分析和 Power BI：针对流式数据的实时分析仪表板
Azure 流分析允许你利用领先的商业智能工具 Microsoft Power BI。 了解如何使用 Azure 流分析来分析大量流式数据，以便在实时 Power BI 分析仪表板中获得相关见解。

使用 [Microsoft Power BI](https://powerbi.com/) 快速构建实时仪表板。 [观看演示本方案的视频](https://www.youtube.com/watch?v=SGUpT-a99MA)。

在本文中，你将了解如何使用 Power BI 作为 Azure 流分析作业的输出，以便创建你自己的自定义商业智能工具，同时充分利用实时仪表板。

## <a name="prerequisites"></a>先决条件
* Microsoft Azure 帐户
* 流分析作业的输入，提供可用的流式数据。 流分析接受 Azure 事件中心或 Azure Blob 存储提供的输入。  
* 用于 Power BI 的工作或学校帐户

## <a name="create-azure-stream-analytics-job"></a>创建 Azure 流分析作业
在 [Azure 经典门户](https://manage.windowsazure.com)中，单击“新建”、“数据服务”、“流分析”、“快速创建”。

指定以下值，然后单击“创建流分析作业”：

* **作业名称** - 输入作业名称。 例如 **DeviceTemperatures**。
* **区域** - 选择希望放置作业的区域。 考虑将作业和事件中心放在同一区域，以确保获得理想的性能，避免在不同区域之间传输数据引发的费用。
* **存储帐户** - 选择用于存储监视在此区域中运行的所有流分析作业数据的存储帐户。 可以选择现有存储帐户，也可以创建新存储帐户。

单击左窗格中的“流分析”，列出流分析作业。

![graphic1][graphic1]

> [!TIP]
> 新作业在列出时的状态为“未启动”。 请注意，页面底部的“启动”按钮已禁用。 这是正常现象，因为你必须先配置作业的输入、输出、查询等，然后才能启动作业。
> 
> 

## <a name="specify-job-input"></a>指定作业输入
就本教程来说，我们假定你使用事件中心作为输入，并使用 JSON 序列化和 UTF-8 编码。

* 单击作业名称。
* 单击页面顶部的“输入”，然后单击“添加输入”。 此时会打开一个对话框，引导你完成设置输入所需的一系列步骤。
* 选择“数据流”，然后单击右侧按钮。
* 选择“事件中心”，然后单击右侧按钮。
* 在第三页中键入或选择以下值：
  * **输入别名** - 输入此作业输入的友好名称。 请注意，你需要在后面的查询中使用此名称。
  * **事件中心** - 如果创建的事件中心与流分析作业属于同一订阅，请选择事件中心所在的命名空间。
* 如果你的事件中心属于其他订阅，请选择“使用其他订阅的事件中心”，然后手动输入以下项目的相关信息：**服务总线命名空间**、**事件中心名称**、**事件中心策略名称**、**事件中心策略密钥**以及**事件中心分区计数**。

> [!NOTE]
> 此示例使用默认数量的分区，即 16 个分区。
> 
> 

* **事件中心名称** - 选择 Azure 事件中心的名称。
* **事件中心策略名称** - 选择所使用的事件中心的事件中心策略。 确保此策略具有管理权限。
* **事件中心使用者组** – 可以将此字段留空，也可以指定事件中心的使用者组。 请注意，每个事件中心的使用者组一次只能有 5 个读取器。 因此，请根据你的作业来相应地确定适当的使用者组。 如果将该字段留空，它将使用默认使用者组。
* 单击右侧按钮。
* 指定以下值：
  * **事件序列化程序格式** - JSON
  * **编码** - UTF8
* 单击相应勾选按钮以添加此源，并确保流分析可以成功连接到事件中心。

## <a name="add-power-bi-output"></a>添加 Power BI 输出
1. 单击页面顶部的“输出”，然后单击“添加输出”。 你将看到 Power BI 被列为输出选项。
   
   ![graphic2][graphic2]  
2. 选择“Power BI”，然后单击右侧的按钮。
3. 你会看到如下所示的屏幕：
   
   ![graphic3][graphic3]  
4. 在此步骤中，请提供流分析作业输出的工作或学校帐户。 如果已经拥有 Power BI 帐户，请选择“立即授权”。 如果没有，选择“立即注册”。 [以下是一个优质博客，介绍了 Power BI 注册的详细信息](http://blogs.technet.com/b/powerbisupport/archive/2015/02/06/power-bi-sign-up-walkthrough.aspx)。
   
   ![graphic11][graphic11]  
5. 接下来，你会看到如下所示的屏幕：
   
   ![graphic4][graphic4]  

提供如下所示的值：

* **输出别名** – 可以使用任何便于引用的输出别名。 如果决定为你的作业设置多个输出，则此输出别名特别有用。 在这种情况下，你必须在查询中引用此输出。 例如，可以使用输出别名值“OutPbi”。
* **数据集名称** - 提供希望 Power BI 输出使用的数据集名称。 例如，可以使用“pbidemo”。
* **表名** - 在 Power BI 输出数据集下提供表名。 该表名可以是“pbidemo”。 目前，流分析作业的 Power BI 输出只能在数据集中设置一个表。
* **工作区** – 在 Power BI 租户中选择一个可在其下创建数据集的工作区。

> [!NOTE]
> 不应在 Power BI 帐户中显式创建此数据集和表。 在启动流分析作业，并且该作业开始向 Power BI 发送输出时，这些文件将自动创建。 如果作业查询没有返回任何结果，数据集和表将无法创建。
> 
> 

* 单击“确定”、“测试连接”，此时输出配置已完成。

> [!WARNING]
> 另请注意，如果 Power BI 已有与你在此流分析作业中提供的数据集和表同名的数据集和表，现有数据将被覆盖。
> 
> 

## <a name="write-query"></a>编写查询
转到作业的“查询”选项卡。 编写查询，你希望其输出位于 Power BI 中。 例如，该查询可以类似于以下 SQL 查询：

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,1),
        dspl



启动你的作业。 验证事件中心是否正在接收事件，以及你的查询是否生成预期的结果。 如果你的查询输出的行数为 0，则不会自动创建 Power BI 数据集和表。

## <a name="create-the-dashboard-in-power-bi"></a>在 Power BI 中创建仪表板
转到 [Powerbi.com](https://powerbi.com)，使用工作或学校帐户登录。 如果流分析作业查询输出了结果，你会看到你的数据集已经创建：

![graphic5][graphic5]

若要创建仪表板，请转到“仪表板”选项，然后创建新的仪表板。

![graphic6][graphic6]

在此示例中，我们将它标记为“演示仪表板”。

现在，请单击流分析作业创建的数据集（当前示例中的 pbidemo）。 你会转到一个页面，然后便可根据此数据集创建图表。 下面仅仅是你可以创建的报告的一个示例：

选择 Σ temp 和时间字段。 然后会自动转到图表的“值”和“轴”：

![graphic7][graphic7]

然后，你会自动获得如下所示的图表：

![graphic8][graphic8]

在值部分，单击 temp 的下拉列表，针对温度选择“平均”，然后在图表中单击“可视化”并选择“折线图”：

![graphic9][graphic9]

现在，你将获得平均值随时间变化的折线图。  使用如下所示的固定选项，你可以将此项固定到此前创建的仪表板：

![graphic10][graphic10]

现在，当你查看包含这个已固定报告的仪表板时，你会看到报告的实时更新。 尝试更改事件中的数据，例如添加一个峰值温度或类似的变化，你就会看到反映在仪表板中的实时效果。

请注意，本教程演示了如何为数据集创建一种类型的图表。 Power BI 可帮助你为组织创建其他客户商业智能工具。 如需 Power BI 仪表板的其他示例，请观看 [Power BI 入门](https://youtu.be/L-Z_6P56aas?t=1m58s)视频。

如需进一步了解如何配置 Power BI 输出以及如何利用 Power BI 组，请参阅[了解流分析输出](stream-analytics-define-outputs.md "了解流分析输出")的 [Power BI部分](stream-analytics-define-outputs.md#power-bi)。 若要详细了解如何通过 Power BI 来创建仪表板，请参阅另一有用的资源：[Power BI 中的仪表板](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/)。

## <a name="limitations-and-best-practices"></a>限制和最佳实践
Power BI 同时部署并行性和吞吐量约束，如下所述：[https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing "Power BI 定价")

由于这些因素，Power BI 最适合可通过 Azure 流分析来大幅减少数据加载的案例。
我们建议你使用 TumblingWindow 或 HoppingWindow 来确保数据推送速率最多为 1 次推送/秒，并且你的查询满足吞吐量要求 – 你可以使用以下公式来计算提供你的窗口所需的值（以秒为单位）：

![equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

例如，如果你每秒有 1,000 个设备在发送数据，并且你使用的 Power BI Pro SKU 支持 1,000,000 行/小时，而你需要获取每个设备在 Power BI 上的平均数据，则每个设备最多可以每 4 秒进行一次推送（如下所示）：  

![equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

这意味着，我们需要将原始查询更改为：

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl

### <a name="powerbi-view-refresh"></a>PowerBI 视图刷新
一个常见的问题是，为何仪表板在 PowerBI 中不自动更新？

为此，可在 PowerBI 中利用“问题与解答”来提问“当时间戳为当天时 temp 的最大值”之类的问题，然后将该磁贴固定到仪表板。

### <a name="renew-authorization"></a>续订授权
如果自作业创建后或上次身份验证后更改了密码，你需要重新对 Power BI 帐户进行身份验证。 如果在 Azure Active Directory (AAD) 租户上配置了 Multi-Factor Authentication (MFA)，还需要每 2 周续订一次 Power BI 授权。 此问题的症状是没有作业输出，并且操作日志存在“验证用户错误”：

![graphic12][graphic12]

同样，如果作业尝试在令牌已过期时启动，将发生错误并且作业启动将失败。 此错误将看起来如下所示：

![PowerBI 验证错误](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-expire.png) 

要解决此问题，请停止正在运行的作业并转到你的 Power BI 输出。  单击“续订授权”链接，并在“上次停止时间”重新启动你的工作以避免数据丢失。

![PowerBI 验证续订](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renew.png) 

使用 Power BI 刷新授权后，你将在授权区域中看到绿色警报：

![PowerBI 验证续订](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renewed.png) 

## <a name="get-help"></a>获取帮助
如需进一步的帮助，请尝试我们的 [Azure 流分析论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>后续步骤
* [Azure 流分析简介](stream-analytics-introduction.md)
* [Azure 流分析入门](stream-analytics-get-started.md)
* [缩放 Azure 流分析作业](stream-analytics-scale-jobs.md)
* [Azure 流分析查询语言参考](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure 流分析管理 REST API 参考](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[graphic1]: ./media/stream-analytics-power-bi-dashboard/1-stream-analytics-power-bi-dashboard.png
[graphic2]: ./media/stream-analytics-power-bi-dashboard/2-stream-analytics-power-bi-dashboard.png
[graphic3]: ./media/stream-analytics-power-bi-dashboard/3-stream-analytics-power-bi-dashboard.png
[graphic4]: ./media/stream-analytics-power-bi-dashboard/4-stream-analytics-power-bi-dashboard.png
[graphic5]: ./media/stream-analytics-power-bi-dashboard/5-stream-analytics-power-bi-dashboard.png
[graphic6]: ./media/stream-analytics-power-bi-dashboard/6-stream-analytics-power-bi-dashboard.png
[graphic7]: ./media/stream-analytics-power-bi-dashboard/7-stream-analytics-power-bi-dashboard.png
[graphic8]: ./media/stream-analytics-power-bi-dashboard/8-stream-analytics-power-bi-dashboard.png
[graphic9]: ./media/stream-analytics-power-bi-dashboard/9-stream-analytics-power-bi-dashboard.png
[graphic10]: ./media/stream-analytics-power-bi-dashboard/10-stream-analytics-power-bi-dashboard.png
[graphic11]: ./media/stream-analytics-power-bi-dashboard/11-stream-analytics-power-bi-dashboard.png
[graphic12]: ./media/stream-analytics-power-bi-dashboard/12-stream-analytics-power-bi-dashboard.png
[graphic13]: ./media/stream-analytics-power-bi-dashboard/13-stream-analytics-power-bi-dashboard.png



<!--HONumber=Nov16_HO3-->

