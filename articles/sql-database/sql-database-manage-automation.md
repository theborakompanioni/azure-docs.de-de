---
title: Verwalten von Azure SQL-Datenbanken mit Azure Automation | Microsoft Docs
description: Erfahren Sie, wie der Azure Automation-Dienst zur angemessenen Verwaltung von Azure SQL-Datenbanken verwendet werden kann.
services: sql-database, automation
documentationcenter: 
author: jodoglevy
manager: jhubbard
editor: monicar
ms.assetid: 77c262a1-9b93-456d-b3c7-b2f23bdfcd61
ms.service: sql-database
ms.custom: monitor & manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: jhubbard
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: f2f2b2ab37d84865c049253be7c30da0d98fd992
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="managing-azure-sql-databases-using-azure-automation"></a>Verwalten von Azure SQL-Datenbanken mit Azure Automation
Dieser Leitfaden bietet eine Einführung in den Azure Automation-Dienst und zeigt, wie Azure Automation zur Vereinfachung der Verwaltung Ihrer Azure SQL-Datenbanken genutzt werden kann.

## <a name="what-is-azure-automation"></a>Was ist Azure Automation?
[Azure Automation](https://azure.microsoft.com/services/automation/) ist ein Azure-Dienst für die Vereinfachung der Cloudverwaltung durch eine  Prozessautomatisierung. Mit Azure Automation können zeitintensive, manuelle, fehleranfällige und häufig ausgeführte Aufgaben automatisiert werden, um Zuverlässigkeit und Effizienz zu steigern und eine schnellere Amortisierung für Ihre Organisation zu erzielen.

Azure Automation bietet ein äußerst zuverlässiges und hochverfügbares Workflow-Ausführungsmodul, das nach Ihren Anforderungen skaliert werden kann, wenn Ihre Organisation wächst. In Azure Automation können Prozesse manuell, durch Drittanbietersysteme oder in geplanten Intervallen gestartet werden, sodass Aufgaben genau nach Bedarf ausgeführt werden.

Indem Sie die Aufgaben in Zusammenhang mit der Cloudverwaltung mit Azure Automation automatisieren, verringern Sie den Verwaltungsaufwand und ermöglichen es Ihren IT- und DevOps-Mitarbeitern, sich Aufgaben zu widmen, die einen Mehrwert für Ihr Unternehmen liefern.

## <a name="how-can-azure-automation-help-manage-azure-sql-databases"></a>Inwiefern unterstützt Azure Automation die Verwaltung von Azure SQL-Datenbanken?
Eine Azure SQL-Datenbank kann in Azure Automation mit den [Azure SQL Database PowerShell-Cmdlets](https://docs.microsoft.com/powershell/servicemanagement/azure.sqldatabase/v1.6.1/azure.sqldatabase/) verwaltet werden, die in den [Azure PowerShell-Tools](/powershell/azure/overview) zur Verfügung stehen. In Azure Automation sind diese PowerShell-Cmdlets für Azure SQL-Datenbanken sofort einsatzfähig, sodass Sie alle Aufgaben in Zusammenhang mit der Verwaltung Ihrer SQL-Datenbanken innerhalb des Diensts ausführen können. Sie können diese Cmdlets in Azure Automation auch mit den Cmdlets für andere Azure-Dienste koppeln, um komplexe Aufgaben in Azure-Diensten und Systemen anderer Hersteller zu automatisieren.

Azure Automation bietet außerdem die Möglichkeit, über die Ausgabe von SQL-Befehlen in PowerShell direkt mit SQL-Servern zu kommunizieren.

Der [Azure Automation-Runbookkatalog](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) enthält eine Vielzahl von Runbooks vom Produktteam und der Community für den Einstieg in die Automatisierung der Verwaltung von Azure SQL-Datenbank, anderen Azure-Diensten und Drittanbietersystemen. Zu den Katalogrunbooks zählen:

* [How to use a SQL Command in an Azure Automation Runbook (in englischer Sprache)](https://gallery.technet.microsoft.com/scriptcenter/How-to-use-a-SQL-Command-be77f9d2)
* [Azure SQL Database: Vertically Scale (in englischer Sprache)](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-Database-e957354f)
* [Truncate table in SQL DB if DB is approaching max size (in englischer Sprache)](https://gallery.technet.microsoft.com/scriptcenter/Azure-Automation-Your-SQL-30f8736b)
* [Indexes tables in an Azure database if they have a high fragmentation (in englischer Sprache)](https://gallery.technet.microsoft.com/scriptcenter/Indexes-tables-in-an-Azure-73a2a8ea)

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun mit den Grundlagen von Azure Automation vertraut gemacht haben, folgen Sie diesen Links, um weitere Informationen zu Azure Automation zu erhalten.

* [Übersicht über Azure Automation](../automation/automation-intro.md)
* [Mein erstes Runbook](../automation/automation-first-runbook-graphical.md)
* [Azure Automation – Lernkurve](https://azure.microsoft.com/documentation/learning-paths/automation/)
* [Azure Automation: Ihr SQL-Agent in der Cloud](https://azure.microsoft.com/blog/2014/06/26/azure-automation-your-sql-agent-in-the-cloud/) 


