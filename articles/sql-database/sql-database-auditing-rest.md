---
title: "REST-API: Verwalten der Überwachung von Azure SQL-Datenbank | Microsoft Docs"
description: "Sie konfigurieren die Überwachung von Azure SQL-Datenbank mithilfe der REST-API zum Nachverfolgen von Datenbankereignissen und zum Schreiben der Ereignisse in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto."
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
ms.openlocfilehash: 9516dd1ae6a67fb9402591fe1906ee7554c1495c
ms.lasthandoff: 02/17/2017


---
# <a name="configure-and-manage-sql-database-auditing-using-the-rest-api"></a>Konfigurieren und Verwalten der Überwachung von SQL-Datenbank mithilfe der REST-API

In diesem Thema wird beschrieben, wie die Überwachung mithilfe der REST-API konfiguriert und verwaltet wird. Informationen zum Konfigurieren und Verwalten der Überwachung über das Azure-Portal finden Sie unter [Konfigurieren der Überwachung im Azure-Portal](sql-database-auditing-portal.md). Informationen zum Konfigurieren und Verwalten der Überwachung mithilfe von PowerShell finden Sie unter [Konfigurieren der Überwachung mithilfe von PowerShell](sql-database-auditing-powershell.md).

Eine Übersicht über die Überwachung finden Sie unter [Überwachung von SQL-Datenbank](sql-database-auditing.md).

## <a name="rest-api---blob-auditing"></a>REST-API – Blobüberwachung

   * [Erstellen oder Aktualisieren einer Richtlinie für die Überwachung von Datenbankblobs](https://msdn.microsoft.com/library/azure/mt695939.aspx)
   * [Erstellen oder Aktualisieren einer Richtlinie für die Überwachung von Serverblobs](https://msdn.microsoft.com/library/azure/mt771861.aspx)
   * [Abrufen einer Richtlinie für die Überwachung von Datenbankblobs](https://msdn.microsoft.com/library/azure/mt695938.aspx)
   * [Abrufen einer Richtlinie für die Überwachung von Serverblobs](https://msdn.microsoft.com/library/azure/mt771860.aspx)
   * [Abrufen des Ergebnisses eines Überwachungsvorgangs für Serverblobs](https://msdn.microsoft.com/library/azure/mt771862.aspx)


## <a name="rest-api---table-auditing"></a>REST-API – Tabellenüberwachung

   * [Erstellen oder Aktualisieren einer Richtlinie für die Überwachung von Datenbanken](https://msdn.microsoft.com/library/azure/mt604471.aspx)
   * [Erstellen oder Aktualisieren einer Richtlinie für die Überwachung von Servern](https://msdn.microsoft.com/library/azure/mt604383.aspx)
   * [Abrufen einer Richtlinie für die Überwachung von Datenbanken](https://msdn.microsoft.com/library/azure/mt604381.aspx)
   * [Abrufen einer Richtlinie für die Überwachung von Servern](https://msdn.microsoft.com/library/azure/mt604382.aspx)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Konfigurieren und Verwalten der Überwachung über das Azure-Portal finden Sie unter [Konfigurieren der Datenbanküberwachung im Azure-Portal](sql-database-auditing-portal.md). 
* Informationen zum Konfigurieren und Verwalten der Überwachung mithilfe von PowerShell finden Sie unter [Konfigurieren der Datenbanküberwachung mithilfe von PowerShell](sql-database-auditing-powershell.md).
* Eine Übersicht über die Überwachung finden Sie unter [Datenbanküberwachung](sql-database-auditing.md).
