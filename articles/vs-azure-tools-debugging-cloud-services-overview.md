<properties 
   pageTitle="调试云服务"
   description="调试云服务"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="patshea123"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="visual-studio-online"
   ms.date="08/12/2015"
   wacn.date="" />

# 调试云服务

您可以使用 Azure Tools for Microsoft Visual Studio 和 Azure SDK，以不同的方法调试 Azure 应用程序：

- 在开发 Azure 应用程序时，可以从 Visual Studio 对其进行调试，就像调试任何 Visual C# 或 Visual Basic 应用程序一样。有关详细信息，请参阅[在 Visual Studio 中调试云服务或虚拟机](http://go.microsoft.com/fwlink/p/?LinkID=623018)。

- 可使用 Azure 诊断以记录在角色内运行的代码的详细信息，角色是否在开发环境或 Azure 中运行。有关详细信息，请参阅[使用 Azure 诊断收集日志记录数据](http://go.microsoft.com/fwlink/p/?LinkId=400450)。

- 如果使用 Visual Studio Enterprise 来编写以 .NET Framework 4 或.NET Framework 4.5 为目标的角色，则从 Visual Studio 部署 Azure 云服务时，可以启用 IntelliTrace。IntelliTrace 提供一个日志，您可将该日志与 Visual Studio 一起使用以调试应用程序，就如同应用程序在 Azure 中运行一样。有关详细信息，请参阅[使用 IntelliTrace 和 Visual Studio 调试已发布的云服务](http://go.microsoft.com/fwlink/p/?LinkId=623016)。

- 从 Visual Studio 部署云服务时，可以在云服务上启用远程调试。如果您选择为部署的项目启用远程调试，将在运行每个角色实例的虚拟机上安装远程调试服务。这些服务（如 msvsmon.exe）不影响性能，也不额外收费。有关详细信息，请参阅[在 Visual Studio 中调试云服务或虚拟机](http://go.microsoft.com/fwlink/p/?LinkID=623018)。

<!---HONumber=71-->