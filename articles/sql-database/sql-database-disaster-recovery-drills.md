---
title: "SQL 数据库灾难恢复演练 | Microsoft 文档"
description: "了解有关使用 Azure SQL 数据库执行灾难恢复演练，帮助任务关键型业务应用程序弹性应对故障和中断的指导和最佳做法。"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: b44a269c-fe2a-404f-b013-290030860bd1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/31/2016
ms.author: sashan
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 1b1d65a41a794a566287dcffe3581ac58e2a965f
ms.contentlocale: zh-cn
ms.lasthandoff: 05/18/2017


---
# <a name="performing-disaster-recovery-drill"></a>执行灾难恢复演练
建议定期对恢复工作流执行应用程序就绪性验证。 验证应用程序的行为以及数据丢失和/或涉及到故障转移的中断所造成的影响，是一种良好的工程实践。 许多行业标准在涉及到业务连续性认证方面也会提出此要求。

执行灾难恢复演练的操作包括：

* 模拟数据层中断
* 恢复
* 验证恢复后的应用程序完整性

根据[针对业务连续性设计应用程序](sql-database-business-continuity.md)的方式，用于执行演练的工作流会有所不同。 下面将会讨论在 Azure SQL 数据库上下文中执行灾难恢复演练的最佳实践。

## <a name="geo-restore"></a>异地还原
若要防止执行灾难恢复演练时发生潜在的数据丢失，我们建议通过创建生产环境的副本在测试环境中执行演练，并使用测试环境来验证应用程序的故障转移工作流。

#### <a name="outage-simulation"></a>中断模拟
若要模拟中断，可以删除或重命名源数据库。 这会导致应用程序连接失败。

#### <a name="recovery"></a>恢复
* 根据[此处](sql-database-disaster-recovery.md)所述，在另一台服务器中执行数据库异地还原。
* 更改应用程序配置以连接到已恢复的数据库，并按照[在恢复后配置数据库](sql-database-disaster-recovery.md)指南完成恢复。

#### <a name="validation"></a>验证
* 通过验证恢复后的应用程序完整性（包括连接字符串、登录名、基本功能测试，或标准应用程序验收过程的其他验证部分）来完成演练。

## <a name="geo-replication"></a>异地复制
对于使用异地复制保护的数据库，演练过程包括按计划故障转移到辅助数据库。 计划的故障转移可确保在切换角色后主数据库和辅助数据库保持同步。 与非计划的故障转移不同，此操作不会导致数据丢失，因此可以在生产环境中执行演练。

#### <a name="outage-simulation"></a>中断模拟
若要模拟中断，可以禁用已连接到数据库的 Web 应用程序或虚拟机。 这会导致 Web 客户端连接失败。

#### <a name="recovery"></a>恢复
* 确保 DR 区域中的应用程序配置指向以前的辅助数据库，故障转移后，该数据库将成为完全可访问的新主数据库。
* 执行[计划的故障转移](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)，使辅助数据库成为新的主数据库
* 根据在[恢复后配置数据库](sql-database-disaster-recovery.md)指南完成恢复。

#### <a name="validation"></a>验证
* 通过验证恢复后的应用程序完整性（包括连接字符串、登录名、基本功能测试，或标准应用程序验收过程的其他验证部分）来完成演练。

## <a name="next-steps"></a>后续步骤
* 若要了解业务连续性方案，请参阅[连续性方案](sql-database-business-continuity.md)
* 若要了解 Azure SQL 数据库的自动备份，请参阅 [SQL 数据库自动备份](sql-database-automated-backups.md)
* 若要了解如何使用自动备份进行恢复，请参阅[从服务启动的备份中还原数据库](sql-database-recovery-using-backups.md)
* 若要了解更快的恢复选项，请参阅[活动异地复制](sql-database-geo-replication-overview.md)  

