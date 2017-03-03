---
title: "PowerShell: Verwalten der Überwachung von Azure SQL-Datenbank | Microsoft Docs"
description: "Sie konfigurieren die Überwachung von Azure SQL-Datenbank mithilfe von PowerShell zum Nachverfolgen von Datenbankereignissen und zum Schreiben der Ereignisse in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto."
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: giladm
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: secure and protect
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2016
ms.author: ronitr; giladm
translationtype: Human Translation
ms.sourcegitcommit: 5d51a5ef3387b4c00079547b0f44ffe1f96bd77c
ms.openlocfilehash: 9839dfb02478593de9a6db59d2a462d2b64f957e
ms.lasthandoff: 02/17/2017


---
# <a name="configure-and-manage-sql-database-auditing-using-powershell"></a>Konfigurieren und Verwalten der Überwachung von SQL-Datenbank mithilfe von PowerShell

In diesem Abschnitt wird beschrieben, wie die Überwachung mithilfe von PowerShell konfiguriert und verwaltet wird. Informationen zum Konfigurieren und Verwalten der Überwachung über das Azure-Portal finden Sie unter [Konfigurieren der Überwachung im Azure-Portal](sql-database-auditing-portal.md). Informationen zum Konfigurieren und Verwalten der Überwachung mithilfe der REST-API finden Sie unter [Konfigurieren der Überwachung mithilfe der REST-API](sql-database-auditing-rest.md).

Eine Übersicht über die Überwachung finden Sie unter [Überwachung von SQL-Datenbank](sql-database-auditing.md).

## <a name="powershell-cmdlets"></a>PowerShell-Cmdlets

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Remove-AzureRMSqlDatabaseAuditing][103]
   * [Remove-AzureRMSqlServerAuditing][104]
   * [Set-AzureRMSqlDatabaseAuditingPolicy][105]
   * [Set-AzureRMSqlServerAuditingPolicy][106]
   * [Use-AzureRMSqlServerAuditingPolicy][107]

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Konfigurieren und Verwalten der Überwachung über das Azure-Portal finden Sie unter [Konfigurieren der Datenbanküberwachung im Azure-Portal](sql-database-auditing-portal.md). 
* Informationen zum Konfigurieren und Verwalten der Überwachung mithilfe von PowerShell finden Sie unter [Konfigurieren der Datenbanküberwachung mithilfe der REST-API](sql-database-auditing-rest.md).
* Eine Übersicht über die Überwachung finden Sie unter [Datenbanküberwachung](sql-database-auditing.md).


[101]: https://msdn.microsoft.com/en-us/library/azure/mt603731(v=azure.200).aspx
[102]: https://msdn.microsoft.com/en-us/library/azure/mt619329(v=azure.200).aspx
[103]: https://msdn.microsoft.com/en-us/library/azure/mt603796(v=azure.200).aspx
[104]: https://msdn.microsoft.com/en-us/library/azure/mt603574(v=azure.200).aspx
[105]: https://msdn.microsoft.com/en-us/library/azure/mt603531(v=azure.200).aspx
[106]: https://msdn.microsoft.com/en-us/library/azure/mt603794(v=azure.200).aspx
[107]: https://msdn.microsoft.com/en-us/library/azure/mt619353(v=azure.200).aspx

