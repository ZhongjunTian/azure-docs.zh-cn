---
title: "Azure Service Fabric 监视和诊断概述 | Microsoft 文档"
description: "了解如何监视和诊断 Azure、开发或本地环境中托管的 Microsoft Azure Service Fabric 应用程序。"
services: service-fabric
documentationcenter: .net
author: ms-toddabel
manager: mfussell
editor: 
ms.assetid: edcc0631-ed2d-45a3-851d-2c4fa0f4a326
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/3/2017
ms.author: toddabel
translationtype: Human Translation
ms.sourcegitcommit: fdb15c3af4980e284e1a9effe434b1cab959d24c
ms.openlocfilehash: 826ae11e4826b37379caa53d85d8ec834a5ed0fb


---
# <a name="monitor-and-diagnose-azure-service-fabric-applications"></a>监视和诊断 Azure Service Fabric 应用程序

监视、检测、诊断和故障排除功能允许服务继续运行，同时对用户体验造成最小的干扰。这些功能可提供业务见解、监视资源用量、检测硬件和软件故障或性能问题，以及诊断潜在的服务行为问题。 尽管监视和诊断在实际部署的生产环境中相当重要，但是效果取决于在服务开发过程中采用类似的模型，以便在转移到实际安装时确保其正常工作。 Service Fabric 使服务开发人员能够轻松实现可以跨单个计算机本地开发安装和实际生产群集安装无缝工作的诊断。 

“监视”是一个泛指的术语，具体包括检测代码、收集检测日志、分析日志、根据日志数据可视化见解、根据日志值和见解发出警报，监视基础结构，以及让工程师检测和诊断对其客户造成了影响的问题。 本文的目的是概述如何使用 .NET 来监视 Azure 中或本地托管的、在 Windows 或 Linux 上部署的 Service Fabric 群集。 首先，让我们将问题划分为三个部分
- 检测代码或基础结构
- 收集生成的事件
- 存储、聚合、可视化和分析

尽管有些产品能够解决所有三个方面的问题，但许多客户倾向于选择不同的技术来解决每种问题。 重要的是，必须将每个组成部分对接在一起才能为应用程序提供端到端的监视解决方案。 

## <a name="monitoring-infrastructure"></a>监视基础结构

尽管在基础结构发生故障期间 Service Fabric 可帮助保持应用程序运行，但应用程序操作员需要了解错误是在应用程序内部还是底层基础结构内部发生的。 规划容量时也需要监视基础结构，以了解何时添加或删除基础结构。 构成 Service Fabric 部署的基础结构和应用程序对于监视和故障排除过程非常重要。 只要将应用程序提供给客户使用，基础结构的某些组件就可能会出现问题。

### <a name="azure-monitoring"></a>Azure 监视

对于 Azure 部署的群集，可以使用 [Azure 监视](../monitoring-and-diagnostics/toc.md)来监视构建 Service Fabric 群集时所在的许多 Azure 资源。 对于[虚拟机规模集 (VMSS)](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)和每个 [VM](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines)，将会自动收集一组指标并将其显示在 Azure 门户中。 可在 Azure 门户中查看这些信息：选择 Service Fabric 群集所在的资源组，然后选择要查看的 VMSS 即可。 在“监视”导航部分中选择“指标”可查看值的图表

![Azure 门户视图：收集的指标信息](./media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

可根据 [Microsoft Azure 中的指标概述](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)一文中的说明自定义图表。 还可以根据[使用 Azure 门户为 Azure 服务创建警报](../monitoring-and-diagnostics/insights-alerts-portal.md)一文中所述，基于这些指标创建警报。 可以根据 [针对 Azure 指标警报配置 Webhook](.../monitoring-and-diagnostics/insights-webhooks-alerts.md) 一文中所述，使用 Webhook 将警报发送到通知服务。 Azure 监视支持单个订阅。 如果需要支持多个订阅或者需要其他功能，[Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) 中的 [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) 组件为本地基础结构和基于云的基础结构提供了全方位的 IT 管理解决方案。 可将来自 Azure 监视的数据直接路由到 Log Analytics，以便在一个位置查看整个环境的指标和日志。

建议使用 [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) 来监视本地基础结构，但是，也可以使用贵公司用来监视基础结构的任何现有解决方案。

### <a name="service-fabric-support-logs"></a>Service Fabric 支持日志

如需联系 Microsoft 支持部门来获取 Azure Service Fabric 群集方面的帮助，几乎始终需要提供支持日志。 如果群集托管在 Azure 中，则会自动配置这些日志，并在创建群集的过程中收集这些日志。 日志存储在可在群集资源组中看到的专用存储帐户中。 该存储帐户没有固定的名称，但在其中可以看到以“fabric”开头的 Blob 容器和表。 如果群集是独立群集，应该遵循[创建和管理独立 Azure Service Fabric 群集](service-fabric-cluster-creation-for-windows-server.md)和 [Windows 独立群集的配置设置](service-fabric-cluster-manifest.md)中的指导，配置这些日志的收集。 对于独立 Service Fabric，应该将日志发送到本地文件共享。 **必须**提供这些日志才能获得支持，但是，这些日志只能由 Microsoft 客户支持团队使用。 我们拥有欧比旺·肯诺比绝技，外人对这些日志只能望洋兴叹...

## <a name="instrument-your-code"></a>检测代码

对于服务监视的其他大多数环节，检测代码是一个基本要求。 人们经常对检测的依赖程度感到吃惊，但考虑到只能通过检测来判断是否出错以及诊断需要解决哪些问题，因此也就能够理解这种依赖性。 尽管在技术上是可行的，但将调试器连接到生产服务的做法并不常见，因此，提供详细的检测数据非常重要。 生成这种量级的信息时，从本地节点传送所有事件可能会消耗大量资源。 许多服务使用由两个部分构成的策略来处理检测数据量：
* 将所有事件保存在本地滚动更新的日志文件中短短几天，只在需要调试时才收集。 通常，详细诊断所需的事件保存在节点上，以降低成本和资源利用率
* 指示服务运行状况的所有事件（例如错误事件、检测信号事件或性能事件）将发送到中心存储库，可在其中用来针对不正常的服务发出警报。

某些解决方案可自动检测代码。 尽管这些产品能够正常运行，但几乎始终都要执行手动检测。 最后，必须提供足够的信息来对应用程序进行取证式的调试。 后续部分将介绍检测代码的不同方法，以及如何在不同的方法之间做出选择。

### <a name="eventsource"></a>EventSource

在 Visual Studio 中通过模板创建 Azure Service Fabric 解决方案时，将生成 EventSource 派生类（*ServiceEventSource* 或 *ActorEventSource*）。 这样就会提供一个模板，可将适用于应用程序或服务的其他事件添加到其中。 EventSource 名称**必须**是唯一的，并且应该将它重命名，而不能使用最初的字符串“MyCompany-&lt;solution&gt;-&lt;project&gt;”。 使用多个同名的 EventSource 定义会导致运行时出现问题。 每个定义的事件必须具有唯一标识符。 如果标识符不唯一，将发生运行时失败。 通常，系统将为标识符预先分配值范围，避免不同的开发团队之间发生冲突。 有关 EventSource 的其他信息，请参阅 [Vance 的博客](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)或 [MSDN 文档](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx)。

#### <a name="using-structured-eventsource-events"></a>使用结构化 EventSource 事件

我们针对特定的用例定义了以下每个事件，例如，注册了某个服务类型。 以这种方式定义消息可以连同错误文本一起打包数据。 这样，就可以根据指定属性的名称或值更方便地执行搜索和筛选。 将检测输出结构化可使其变得更易于使用，但需要花费更多的精力和时间来为每种用例定义新的事件。 某些事件定义可在整个应用程序中共享，例如，可在应用程序中的多个服务之间重复使用方法启动或停止事件。 特定于域的服务（例如订单系统）可以包含 CreateOrder 事件，该事件包含自身的唯一事件。 通常，这种方法会生成大量的事件，可能需要在项目团队之间协调标识符。 有关 Service Fabric 中 EventSources 结构的完整示例，请参阅“合作群集”示例中的 PartyCluster.ApplicationDeployService。

```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // Instance constructor is private to enforce singleton semantics
        private ServiceEventSource() : base() { }

        ...

        // ServiceTypeRegistered event contains a unique identifier, an event attribute that defined the event and the code implementation of the event.
        private const int ServiceTypeRegisteredEventId = 3;
        [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
        public void ServiceTypeRegistered(int hostProcessId, string serviceType)
        {
            WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
        }

        // ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined the event and the code implementation of the event.
        private const int ServiceHostInitializationFailedEventId = 4;
        [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
        public void ServiceHostInitializationFailed(string exception)
        {
            WriteEvent(ServiceHostInitializationFailedEventId, exception);
        }
```
#### <a name="using-eventsource-generically"></a>一般情况下使用 EventSource

由于特定的事件可能难以定义，许多用户都会使用一组常用的参数来定义少量的事件，这些参数通常以字符串的形式输出其信息。 因此，大部分结构化信息已丢失，使结果搜索和筛选变得更加困难。 使用此方法时，将会定义一些通常对应于日志记录级别的事件。 以下代码片段定义调试和错误消息。
```csharp
    [EventSource(Name = "MyCompany-VotingState-VotingStateService")]
    internal sealed class ServiceEventSource : EventSource
    {
        public static readonly ServiceEventSource Current = new ServiceEventSource();

        // Instance constructor is private to enforce singleton semantics
        private ServiceEventSource() : base() { }

        ...

        private const int DebugEventId = 10;
        [Event(DebugEventId, Level = EventLevel.Verbose, Message = "{0}")]
        public void Debug(string msg)
        {
            WriteEvent(DebugEventId, msg);
        }

        private const int ErrorEventId = 11;
        [Event(ErrorEventId, Level = EventLevel.Error, Message = "Error: {0} - {1}")]
        public void Error(string error, string msg)
        {
            WriteEvent(ErrorEventId, error, msg);
        }
```
也可以混合使用结构化检测和常规检测方法。 在这种情况下，结构化检测用于报告错误和指标，而常规事件可用于详细日志记录，工程师在排除故障时可以使用这些日志信息。

### <a name="aspnet-core-logging"></a>ASP.NET Core 日志记录

有时很难选择要如何检测代码。如果选择不当，然后必须重新检测，则需要重新访问代码基，从而可能使其变得不稳定。 为了降低风险，开发人员可以选择一个检测库，例如，ASP.NET Core 提供的 [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/)。 此库提供一个 [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) 接口来允许使用所选的提供程序，同时尽量减轻对现有代码的影响。 这种做法的另一个优势是，代码不仅可以在 Windows 和 Linux 上的 .NET Core 中使用，而且可以在整个框架中使用，因此可跨 .NET 和 .NET Core 标准化检测代码。

#### <a name="how-to-use-microsoftextensionslogging-within-service-fabric"></a>如何在 Service Fabric 中使用 Microsoft.Extensions.Logging

1. 将 **Microsoft.Extensions.Logging** NuGet 包添加到要检测的项目。 此外，还需要添加所有提供程序包。下面，我们将针对某个第三方包执行此操作。 有关详细信息，请参阅 [Logging in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging)（ASP.NET Core 中的日志记录）
2. 在服务文件中添加一条针对“Microsoft.Extensions.Logging”的 using 指令
3. 在服务类中定义一个专用变量
```csharp
        private ILogger _logger = null;
```
4. 在服务类的构造函数中，添加
```csharp
        _logger = new LoggerFactory().CreateLogger<Stateless>();
```
5. 开始检测方法中的代码。 下面提供了几个示例
```csharp
        _logger.LogDebug("Debug level event from Microsoft.Logging");
        _logger.LogInformation("Informational level event from Microsoft.Logging");

        // In this variant, we're adding structured properties RequestName and Duration that has values MyRequest and the duration of the request.
        // More on why you'll want to do this later.
        _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);
```

#### <a name="using-other-logging-providers"></a>使用其他日志记录提供程序

可以配合某些第三方提供程序来使用此方法，例如 [SeriLog](https://serilog.net/)、[NLog](http://nlog-project.org/) 和 [loggr](https://github.com/imobile3/Loggr.Extensions.Logging)。 其中的每个提供程序可插入 ASP.Net Core 日志记录，也可以单独使用。 SeriLog 中的某个功能可以扩充记录器发出的所有消息，对服务名称、类型和分区信息的输出可能很有作用。 若要在 ASP.NET Core 基础结构中使用此功能，请执行以下操作

1. 将 **Serilog**、**Serilog.Extensions.Logging** 和 **Serilog.Sinks.Observable** NuGet 包添加到项目。 另外，请为本示例添加 **SeriLog.Sinks.Literate**。本文稍后将介绍一种更好的方法
2. 在 SeriLog 中创建 LoggerConfiguration 和记录器实例
```csharp
    Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
```
3. 将 SeriLog.ILogger 参数添加到服务构造函数并传递新建的记录器
```csharp
    ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
```
4. 在服务构造函数中添加以下内容，为服务的 ServiceTypeName、ServiceName、PartitionId 和 InstanceId 属性创建属性扩充器。 此功能还会将该属性添加到 ASP.NET Core 日志记录工厂，以便能够在代码中使用 Microsoft.Extensions.Logging.ILogger。
```csharp
        public Stateless(StatelessServiceContext context, Serilog.ILogger serilog)
            : base(context)
        {
            PropertyEnricher[] properties = new PropertyEnricher[]
            {
                new PropertyEnricher("ServiceTypeName", context.ServiceTypeName),
                new PropertyEnricher("ServiceName", context.ServiceName),
                new PropertyEnricher("PartitionId", context.PartitionId),
                new PropertyEnricher("InstanceId", context.ReplicaOrInstanceId),
            };

            serilog.ForContext(properties);

            _logger = new LoggerFactory().AddSerilog(serilog.ForContext(properties)).CreateLogger<Stateless>();
        }
```
5. 就像在不使用 SeriLog 的情况下运行 ASP.NET Core 一样检测代码。

> [!NOTE] 
> 不建议在这种方法中使用静态 Log.Logger，因为 Service Fabric 可在单个进程中托管同一服务类型的多个实例。 这意味着，属性扩充器的最后一个写入者会向所有正在运行的实例显示扩充器的值。 这是 _logger 变量为何是服务类的专用成员变量的原因之一。 这也意味着，必须将 _logger 提供给可跨服务使用的通用代码使用。 

### <a name="which-one-should-i-use"></a>应使用哪种方法？

如果应用程序极其关注性能，则最好使用 EventSource，因为与 ASP.NET Core 日志记录或任何第三方解决方案相比，它使用的资源**通常**更少，并且表现更佳。  对于许多服务来说这并不是一个问题，但如果服务以性能为中心，则 EventSource 可能是更好的选择。 若要获得与结构化日志记录相同的优势，EventSource 要求工程团队做出大笔投资。 若要确定应为项目选择哪种方法，最合理的做法是根据每种方法快速制作一个原型，然后选择最符合需要的方法。

## <a name="event-and-log-collection"></a>事件和日志收集

### <a name="azure-diagnostics"></a>Azure 诊断

除了 Azure 监视的现有功能以外，Azure 还提供了将事件从每个服务收集到中心位置的方式。 有两篇文章介绍了如何为 [Windows](service-fabric-diagnostics-how-to-setup-wad.md) 和 [Linux](service-fabric-diagnostics-how-to-setup-lad.md) 配置事件收集。 其中说明了如何收集事件数据并将其发送到 Azure 存储。 在门户或 Resource Manager 模板中启用诊断，就能轻松实现此目的。 启用此功能可收集 Service Fabric 自动生成的一些事件源：

- 使用 Reliable Actor 编程模型时生成的 EventSource 事件和性能计数器。 有关 [Reliable Actors 的诊断和性能监视](service-fabric-reliable-actors-diagnostics.md)的文章中列举了这些事件
- 使用 Reliable Services 编程模型时生成的 EventSource 事件。 有关[有状态 Reliable Services 的诊断功能](service-fabric-reliable-services-diagnostics.md)的文章中列举了这些事件
- 系统事件以 ETW 事件的形式发出。 从 Service Fabric 发出的许多事件都属于此类别，包括服务位置、启动/停止事件。 查看发出的事件的最佳方式是在本地计算机上运行时，使用 [Visual Studio 诊断查看器](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)。 由于这些事件属于本机 ETW 事件，因此其收集方式存在一些限制
- 从 Service Fabric 版本 5.4 开始，将会公开运行状况和负载指标事件。 这样，便可以收集这些事件用于历史报告和警报。 这些事件也属于本机 ETW 事件，因此其收集方式存在一些限制

经过配置后，这些事件将显示在创建群集时创建的某个 Azure 存储帐户中（假设已启用诊断）。 表命名为 *WADServiceFabricReliableActorEventTable*、*WADServiceFabricReliableServiceEventTable* 和 *WADServiceFabricSystemEventTable*。 默认不会添加运行状况事件，这些事件要求修改 Resource Manager 模板。 有关详细信息，请参阅[使用 Azure 诊断收集日志](service-fabric-diagnostics-how-to-setup-wad.md)。

这些文章还介绍了如何将自定义事件捕获到 Azure 存储中。 有关如何配置性能计数器或者将其他监视信息从 VM 捕获到 Azure 诊断的任何现有 Azure 诊断文章同样适用于 Service Fabric 群集。 例如，如果不想要将 Azure 表存储用作目标，可参阅有关如何[使用事件中心流式传输热路径中的 Azure 诊断数据](../event-hubs/event-hubs-streaming-azure-diags-data.md)的文章。 事件进入中心事件后，可以读取这些事件并将其发送到所选位置。 此外，还有一篇文章介绍了如何[使用 Application Insights 集成 Azure 诊断信息](https://azure.microsoft.com/blog/azure-diagnostics-integration-with-application-insights/)

使用 Azure 诊断的缺点之一是，配置是使用 Resource Manager 模板完成的，因此只能在 VMSS 级别进行。 VMSS 对应于 Service Fabric 中的节点类型。 这意味着，必须针对可在每个类型的节点上运行的所有应用程序和服务配置每个节点类型。 根据配置的应用程序和服务数目，这可能会生成大量的 EventSource。 此外，每当任何应用程序的配置发生更改时，都必须执行 Resource Manager 部署。 理想的情况下，监视配置会连同服务配置一起传播。

Azure 诊断仅适用于部署到 Azure 的 Service Fabric 群集，但适用于 Windows 和 Linux 群集。

### <a name="eventflow"></a>EventFlow

[EventFlow 由 Visual Studio 团队发布](service-fabric-diagnostic-collect-logs-without-an-agent.md)，它提供一种机制用于将事件从某个节点路由到一个或多个监视目标。 由于它以 NuGet 包的形式包含在服务项目中，EventFlow 的代码和配置可以连同服务一起传播，消除了上面所述的需要在 Azure 诊断中为每个节点完成配置的问题。 EventFlow 在服务进程中运行，直接连接到配置的输出。 由于这种直接连接，EventFlow 适用于 Azure、容器或本地服务部署。 在容器等高密度方案中运行时必须保持谨慎，由于每个 EventFlow 管道会建立外部连接，如果要托管大量进程，最终会建立大量的出站连接！ 这对于 Service Fabric 应用程序而言并不是一个大问题，因为 ServiceType 的所有副本在同一个进程中运行，限制了出站连接数。 EventFlow 还提供事件筛选，以便只发送与指定的筛选器匹配的事件。 有关在 Service Fabric 中使用 EventFlow 的详细信息，请参阅[直接从 Azure Service Fabric 服务进程收集日志](service-fabric-diagnostic-collect-logs-without-an-agent.md)

> [!NOTE]
> 在将来的 Service Fabric 版本中，将提供一个 EventSource 主机应用程序，用于根据输入、节点级指标集合和滚动更新日志文件支持侦听 ETW。

EventFlow 的用法相当简单
1. 将 NuGet 包添加到服务项目
2. 在服务的 **Main** 函数中创建 EventFlow 管道并配置输出。 本例说明如何将 SeriLog 用作输出
```csharp
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>
        private static void Main()
        {
            try
            {
                using (var pipeline = ServiceFabricDiagnosticPipelineFactory.CreatePipeline("MonitoringE2E-Stateless-Pipeline"))
                {
                    IObserver<LogEvent> serilogInput = pipeline.Inputs.OfType<SerilogInput>().First();
                    Log.Logger = new LoggerConfiguration().WriteTo.Observers(events => events.Subscribe(serilogInput)).CreateLogger();

                    // The ServiceManifest.XML file defines one or more service type names.
                    // Registering a service maps a service type name to a .NET type.
                    // When Service Fabric creates an instance of this service type,
                    // an instance of the class is created in this host process.

                    ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
                    ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless).Name);
                    
                    // Prevents this host process from terminating so services keep running.
                    Thread.Sleep(Timeout.Infinite);
                }
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e.ToString());
                throw;
            }
        }
    }
```
3. 在服务的 PackageRoot | Config 文件夹中创建名为 *eventFlowConfig.json* 的文件。 该文件中的配置如下所示
```json
    {
    "inputs": [
        {
        "type": "EventSource",
        "sources": [
            { "providerName": "Microsoft-ServiceFabric-Services" },
            { "providerName": "Microsoft-ServiceFabric-Actors" },
            { "providerName": "MyCompany-MonitoringE2E-Stateless" }
        ]
        },
        {
        "type": "Serilog"
        }
    ],
    "filters": [
        {
        "type": "drop",
        "include": "Level == Verbose"
        },
        {
        "type": "metadata",
        "metadata": "request",
        "requestNameProperty": "RequestName",
        "include":  "RequestName==MyRequest",
        "durationProperty": "Duration",
        "durationUnit": "milliseconds"
        }
    ],
    "outputs": [
        {
        "type": "StdOutput"
        },
        {
        "type": "ApplicationInsights",
        "instrumentationKey": "== instrumentation key here =="
        }
    ],
    "schemaVersion": "2016-08-11",
    "extensions": []
    }
```
配置中定义了两个输入：Service Fabric 创建的两个基于 EventSource 的源，以及服务的 EventSource。 请注意，使用 ETW 的系统级别和运行状况事件不可用于 EventFlow。 这是因为，需要使用高级特权来侦听 ETW 源，并且永远不应使用任何高特权运行服务。 另一个输入为 SeriLog，该项配置出现在 **Main** 方法中。  此外应用了一些筛选器，第一个筛选器告知 EventFlow 要删除属于详细事件级别的所有事件。 稍后我们再讲述另一个筛选器定义。 另外，还配置了两个输出：标准输出，将数据写入 Visual Studio 中的输出窗口。 另一个输出是 ApplicationInsights，请务必添加你的检测密钥。

4. 最后一步是检测代码。 在本示例中，我们将以几种不同的方式检测 RunAsync。 以下代码仍使用 SeriLog，使用的某些语法特定于 SeriLog。 请注意所选日志记录解决方案的具体功能。 生成了三个事件：一个调试级事件和两个信息性事件，后者跟踪请求持续时间。 如果使用上述 EventFlow 配置，调试级事件不应流向输出。

```csharp
    Stopwatch sw = Stopwatch.StartNew();

    while (true)
    {
        cancellationToken.ThrowIfCancellationRequested();

        sw.Restart();

        // Delay a random interval to provide a more interesting request duration.
        await Task.Delay(TimeSpan.FromMilliseconds(DateTime.Now.Millisecond), cancellationToken);

        ServiceEventSource.Current.ServiceMessage(this.Context, "Working-{0}", ++iterations);
        _logger.LogDebug("Debug level event from Microsoft.Logging");
        _logger.LogInformation("Informational level event from Microsoft.Logging");
        _logger.LogInformation("{RequestName} {Duration}", "MyRequest", sw.ElapsedMilliseconds);
    }
```

若要在 Application Insights 中查看事件，请打开 Azure 门户并导航到你的 ApplicationInsights 资源。 在左上角单击“搜索”，随后应显示所有事件。

![Application Insights 中的事件搜索视图](./media/service-fabric-diagnostics-overview/ai-search-events.png)

跟踪位于图片底部。 可以看到，这里只有两个事件，因为调试级事件已被 EventFlow 删除。 跟踪上面的请求条目是什么？ 它是第三个“_logger”检测行，显示该事件已转换成 Application Insights 中的请求指标。 让我们回到筛选器定义，其类型为“metadata”。 此定义声明某个事件具有“RequestName”属性且其值为“MyRequest”，另一个属性“Duration”包含请求持续时间，以毫秒为单位。 这就是在 Application Insights 中的请求事件内看到的内容。 同样的方法适用于任何受支持的 EventFlow 输入，包括 EventSource。

如果群集是独立的群集，出于策略原因无法连接到基于云的解决方案，EventFlow 支持使用弹性搜索作为输出，但可以写入其他输出，并且我们建议使用提取请求。 ASP.NET Core 日志记录的某些第三方提供程序提供支持本地安装的解决方案。

## <a name="azure-service-fabric-health-and-load-reporting"></a>Azure Service Fabric 运行状况和负载报告

Service Fabric 具有自身的运行状况模型，有多篇文章对此做了详细介绍
- [Service Fabric 运行状况监视简介](service-fabric-health-introduction.md)
- [报告并检查服务运行状况](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [添加自定义 Service Fabric 运行状况报告](service-fabric-report-health.md)
- [查看 Service Fabric 运行状况报告](service-fabric-view-entities-aggregated-health.md)

运行状况监视对于运行服务的各个方面至关重要。 最重要的是，当 Service Fabric 执行命名应用程序升级时，服务的每个升级域都已升级并且提供给客户使用后，只有通过了运行状况检查，才能转到下一个升级域。 如果无法实现良好的运行状况，部署将会回滚，使应用程序保持一种已知正常的状态。 尽管在回滚服务之前某些客户受到了影响，但大多数客户未遇到任何问题。 此外，问题的解决速度相对较快，无需等待操作员的人工操作。 在代码中合并的运行状况检查越多，服务应对部署问题的弹性就越高。

服务运行状况的另一个方面是从服务报告指标。 指标在 Service Fabric 中非常重要，因为它们用于均衡资源使用量，并用作系统运行状况的指示器。 假设应用程序包含许多服务，每个实例报告每秒请求数 (RPS) 指标。 如果某个服务使用的资源比另一个服务要多，Service Fabric 会围绕群集移动服务实例，尽量使资源利用率保持均衡。 [在 Service Fabric 中使用指标管理资源消耗和负载](service-fabric-cluster-resource-manager-metrics.md)一文提供了此项技术的工作原理的详细说明。

使用指标还能洞察服务的执行情况，从长远来看，可用于检查服务是否根据所需的参数运行。 例如，如果根据趋势，星期一上午 9 点的平均 RPS 为 1000，则可以设置一份运行状况报告，以便在 RPS 低于 500 或高于 1500 时发出警报。 尽管一切看上去正常，但还是值得执行一番检查，确保客户获得优越的体验。 服务可能会定义一组用于运行状况报告的指标，但不会影响群集的资源均衡，在这种情况下，可将指标权重设置为零。 建议一开始为所有指标使用零权重，只有在确定这会对群集的资源均衡产生何种影响时，才增大权重。

> [!TIP]
> 请注意不要使用过多的加权指标，否则可能难以了解服务实例为何被移动，并且某些指标可能会持续很长时间！

指标和运行状况报告的候选项就是可以指明应用程序的运行状况和性能的任何对象。 CPU 性能计数器可以告知节点的利用情况，但不会真正指明特定的服务是否正常，因为单个节点上可能运行了多个服务。 另一方面，RPS、已处理的项数或请求延迟等指标都可以指明特定服务的运行状况。

若要报告运行状况，请添加如下所示的代码

```csharp
 if (!result.HasValue)
 {
     HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
     this.Partition.ReportInstanceHealth(healthInformation);
 }
```

若要报告指标，请将类似于下面的代码添加到服务

```csharp
this.ServicePartition.ReportLoad(new List<LoadMetric> { new LoadMetric("MemoryInMb", 1234), new LoadMetric("metric1", 42) });
```

## <a name="watchdogs"></a>监视器

监视器是一个独立的服务，可以监视各个服务的运行状况和负载，并报告运行状况模型层次结构中任何组件的运行状况。 这有助于防止出现基于单个服务的视图所检测不到的错误。 此外，监视器也是一个不错的代码托管位置，它可以针对已知状态执行补救措施，而无需任何人工交互。

## <a name="visualization-analysis-and-alerting"></a>可视化、分析和警报

监视的最后一个部分是可视化事件流、针对服务性能提供报告，以及在检测到问题时发出警报。 有大量的解决方案可用于实现监视的这个层面。使用 Application Insights 和 OMS 可以基于事件流发出警报。 可以使用 PowerBI 或第三方解决方案（如 [Kibana](https://www.elastic.co/products/kibana) 或 [Splunk](https://www.splunk.com/)）来可视化数据。

## <a name="next-steps"></a>后续步骤

* [如何使用 Azure 诊断收集日志](service-fabric-diagnostics-how-to-setup-wad.md)
* [直接从 Azure Service Fabric 服务进程收集日志](service-fabric-diagnostic-collect-logs-without-an-agent.md)
*  [在 Service Fabric 中使用指标管理资源消耗和负载](service-fabric-cluster-resource-manager-metrics.md)




<!--HONumber=Feb17_HO1-->

