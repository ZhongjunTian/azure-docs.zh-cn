---
title: "如何通过 Linux 使用 Azure 文件 | Microsoft Docs"
description: "按照此分步教程中的说明，在云中创建 Azure 文件共享。 管理文件共享内容，并从运行 Linux 的 Azure 虚拟机 (VM) 或支持 SMB 3.0 的本地应用程序安装文件共享。"
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6edc37ce-698f-4d50-8fc1-591ad456175d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/8/2017
ms.author: renash
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: 201ceaec874c2367c232076faba25bdae128e7e1
ms.lasthandoff: 04/06/2017


---
# <a name="how-to-use-azure-file-storage-with-linux"></a>如何通过 Linux 使用 Azure 文件存储
## <a name="overview"></a>概述
Azure 文件存储使用标准 SMB 协议在云中提供文件共享。 使用 Azure 文件，你可以将依赖于文件服务器的企业应用程序迁移到 Azure。 在 Azure 中运行的应用程序可以轻松地从运行 Linux 的 Azure 虚拟机装载文件共享。 并且使用最新版本的文件存储，你还可以从支持 SMB 3.0 的本地应用程序装载文件共享。

可以使用 [Azure 门户](https://portal.azure.com)、Azure 存储 PowerShell cmdlet、Azure 存储客户端库或 Azure 存储 REST API 来创建 Azure 文件共享。 此外，由于文件共享是 SMB 共享，因此你还可以通过标准的文件系统 API 来访问它们。

文件存储基于与 Blob、表和队列存储相同的技术构建，因此文件存储能够提供 Azure 存储平台内置的现有可用性、持续性、可伸缩性和异地冗余。 有关文件存储性能目标和限制的详细信息，请参阅[Azure 存储的可伸缩性和性能目标](storage-scalability-targets.md)。

文件存储现已正式推出并同时支持 SMB 2.1 和 SMB 3.0。 有关文件存储的更多详细信息，请参阅 [File Service REST API](https://msdn.microsoft.com/library/azure/dn167006.aspx)（文件服务 REST API）。

> [!NOTE]
> 由于 Linux SMB 客户端尚不支持加密，从 Linux 装载文件共享仍需要客户端与文件共享在同一 Azure 区域中。 但是，Linux 的加密支持已经在负责 SMB 功能的 Linux 开发人员的路线图上。 将来支持加密的 Linux 分发也将能够从任何位置装载 Azure 文件共享。
> 
> 

## <a name="video-using-azure-file-storage-with-linux"></a>视频：通过 Linux 使用 Azure 文件存储
下面是演示如何在 Linux 上创建和使用 Azure 文件共享的视频。

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-File-Storage-with-Linux/player]
> 
> 

## <a name="choose-a-linux-distribution-to-use"></a>选择要使用的 Linux 分发
在 Azure 中创建 Linux 虚拟机时，可以从 Azure 映像库指定支持 SMB 2.1 或更高版本的 Linux 映像。 下面是建议的 Linux 映像的列表：

* Ubuntu Server 14.04+
* RHEL 7+
* CentOS 7+
* Debian 8
* openSUSE 13.2+
* SUSE Linux Enterprise Server 12
* SUSE Linux Enterprise Server 12 (Premium Image)

## <a name="mount-the-file-share"></a>装载文件共享
若要从运行 Linux 的虚拟机装载文件共享，可能需要安装 SMB/CIFS 客户端（如果你使用的分发没有内置的客户端）。 这是 Ubuntu 中的命令，用于安装一个选择 cifs-utils：

```
sudo apt-get install cifs-utils
```

接下来，你需要设置装入点 (mkdir mymountpoint)，然后发出类似于下面所示的装载命令：

```
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename ./mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777,serverino
```

你还可以在 /etc/fstab 中添加设置以装载共享。

请注意，此处的 0777 代表目录/文件权限代码，用于向所有用户授予执行/读/写权限。 你可以将其替换为 Linux 文件权限文档后面的其他文件权限代码。

另外要使文件共享在重新启动后装载，可以在 /etc/fstab 中添加如下设置：

```
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777,serverino
```

例如，如果你是使用 Linux 映像 Ubuntu Server 15.04（可从 Azure 映像库中获得）创建的 Azure VM，则可以按如下所示装载文件：

```
azureuser@azureconubuntu:~$ sudo apt-get install cifs-utils
azureuser@azureconubuntu:~$ sudo mkdir /mnt/mountpoint
azureuser@azureconubuntu:~$ sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777,serverino
azureuser@azureconubuntu:~$ df -h /mnt/mountpoint
Filesystem  Size  Used Avail Use% Mounted on
//myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint
```

如果你使用 CentOS 7.1，则可以按如下所示装载文件：

```
[azureuser@AzureconCent ~]$ sudo yum install samba-client samba-common cifs-utils
[azureuser@AzureconCent ~]$ sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777,serverino
[azureuser@AzureconCent ~]$ df -h /mnt/mountpoint
Filesystem  Size  Used Avail Use% Mounted on
//myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint
```

如果你使用 Open SUSE 13.2，则可以按如下所示装载文件：

```
azureuser@AzureconSuse:~> sudo zypper install samba*  
azureuser@AzureconSuse:~> sudo mkdir /mnt/mountpoint
azureuser@AzureconSuse:~> sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777,serverino
azureuser@AzureconSuse:~> df -h /mnt/mountpoint
Filesystem  Size  Used Avail Use% Mounted on
//myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint
```

如果使用 RHEL 7.3，则可以按如下所示装载文件：

```
azureuser@AzureconRedhat:~> sudo yum install cifs-utils
azureuser@AzureconRedhat:~> sudo mkdir /mnt/mountpoint
azureuser@AzureconRedhat:~> sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mnt/mountpoint -o vers=3.0,user=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777,serverino
azureuser@AzureconRedhat:~> df -h /mnt/mountpoint
Filesystem  Size  Used Avail Use% Mounted on
//myaccountname.file.core.windows.net/mysharename  5.0T   64K  5.0T   1% /mnt/mountpoint
```

## <a name="manage-the-file-share"></a>管理文件共享
[Azure 门户](https://portal.azure.com)提供用于管理 Azure 文件存储的用户界面。 你可以从 Web 浏览器中执行以下操作：

* 将文件上载到文件共享和从文件共享下载文件。
* 监视每个文件共享的实际使用情况。
* 调整文件共享大小配额。
* 复制 `net use` 命令以用于从 Windows 客户端装载文件共享。

还可以从 Linux 使用 Azure 跨平台命令行界面 (Azure CLI) 来管理文件共享。 Azure CLI 提供了一组开放源代码跨平台命令，你可以使用这些命令来处理 Azure 存储（包括文件存储）。 它提供 Azure 门户所提供的很多相同功能，此外还有各种数据访问功能。 有关示例，请参阅[将 Azure CLI 用于 Azure 存储](storage-azure-cli.md)。

## <a name="develop-with-file-storage"></a>使用文件存储进行开发
作为开发人员，可以通过[适用于 Java 的 Azure 存储客户端库](https://github.com/azure/azure-storage-java)使用文件存储生成应用程序。 有关代码示例，请参阅[如何通过 Java 使用文件存储](storage-java-how-to-use-file-storage.md)。

还可以使用[适用于 Node.js 的 Azure 存储客户端库](https://github.com/Azure/azure-storage-node)对文件存储进行开发。

## <a name="feedback-and-more-information"></a>反馈和详细信息
Linux 用户，我们希望倾听你的意见！

针对 Linux 用户组的 Azure 文件存储提供了一个论坛，当你在 Linux 上评估和采用文件存储时，可以在该论坛上共享反馈。 向 [Azure 文件存储 Linux 用户](mailto:azurefileslinuxusers@microsoft.com)发送电子邮件可加入该用户组。

## <a name="next-steps"></a>后续步骤
请参阅以下链接以获取有关 Azure 文件存储的更多信息。

### <a name="conceptual-articles-and-videos"></a>概念性文章和视频
* [Azure 文件存储：适用于 Windows 和 Linux 的顺畅的云 SMB 文件系统](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [在 Windows 上开始使用 Azure 文件存储](storage-dotnet-how-to-use-files.md)

### <a name="tooling-support-for-file-storage"></a>文件存储的工具支持
* [使用 AzCopy 命令行实用程序传输数据](storage-use-azcopy.md)
* 通过 Azure CLI [创建和管理文件共享](storage-azure-cli.md#create-and-manage-file-shares)

### <a name="reference"></a>引用
* [文件服务 REST API 参考](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [Azure 文件疑难解答文章](storage-troubleshoot-file-connection-problems.md)

### <a name="blog-posts"></a>博客文章
* [Azure 文件存储现已正式发布](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Azure 文件存储内部](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Microsoft Azure 文件服务简介](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [将连接保存到 Microsoft Azure 文件中](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)

