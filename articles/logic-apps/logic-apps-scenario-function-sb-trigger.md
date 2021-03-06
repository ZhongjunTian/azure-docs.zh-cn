---
title: "方案 - 使用 Azure Functions 和 Azure 服务总线触发逻辑应用 | Microsoft Docs"
description: "使用 Azure Functions 和 Azure 服务总线创建触发逻辑应用的函数"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/23/2016
ms.author: LADocs; jehollan
ms.translationtype: Human Translation
ms.sourcegitcommit: 98c78d84f3a615fae7d6785994f0db20f7a53254
ms.openlocfilehash: 013e3d29694a8daf1481e513c9c4dfc6b5da3384
ms.contentlocale: zh-cn
ms.lasthandoff: 02/10/2017


---
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a>方案：使用 Azure Functions 和 Azure 服务总线触发逻辑应用

需要部署长时间运行的侦听器或任务时，可以使用 Azure Functions 为逻辑应用创建触发器。 例如，可以创建一个函数以侦听队列，然后立即以推送触发器的形式触发逻辑应用。

## <a name="build-the-logic-app"></a>构建逻辑应用
在此示例中，你有一个为需要触发的每个逻辑应用运行的函数。 首先，创建一个具有 HTTP 请求触发器的逻辑应用。 每当收到队列消息时，函数会调用该终结点。  

1. 创建逻辑应用。
2. 选择“手动 - 收到 HTTP 请求时”触发器。
   或者，可以使用诸如 [jsonschema.net](http://jsonschema.net) 此类的工具来指定要用于队列消息的 JSON 架构。 将该架构粘贴到触发器中。 架构有助于设计器理解数据的形状并使属性更易于流过工作流。
2. 添加要在收到队列消息之后进行的任何其他步骤。 例如，通过 Office 365 发送电子邮件。  
3. 保存逻辑应用以便为触发器生成指向此逻辑应用的回调 URL。 该 URL 会显示在触发器卡上。

![该回调 URL 会显示在触发器卡上][1]

## <a name="build-the-function"></a>构建函数
接下来，需要创建一个充当触发器并侦听队列的函数。

1. 在 [Azure Functions 门户](https://functions.azure.com/signin)中，选择“新建函数”，然后选择“ServiceBusQueueTrigger - C#”模板。
   
    ![Azure Functions 门户][2]
2. 配置与服务总线队列之间的连接，该连接会使用 Azure 服务总线 SDK `OnMessageReceive()` 侦听器。
3. 编写一个基本函数，以便通过将队列消息用作触发器来调用（之前创建的）逻辑应用终结点。 下面是函数的完整示例。 该示例使用 `application/json` 消息内容类型，不过可以在需要时更改此类型。
   
   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;
   
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";
   
   public static void Run(string myQueueItem, TraceWriter log)
   {
   
       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

若要进行测试，请通过诸如[服务总线资源管理器](https://github.com/paolosalvatori/ServiceBusExplorer)此类的工具添加队列消息。 可看到逻辑应用会在该函数收到消息之后立即触发。

<!-- Image References -->
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png

