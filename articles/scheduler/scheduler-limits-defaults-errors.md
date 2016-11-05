---
title: Scheduler – Einschränkungen und Standardwerte
description: Scheduler – Einschränkungen und Standardwerte
services: scheduler
documentationcenter: .NET
author: krisragh
manager: dwrede
editor: ''

ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: krisragh

---
# Scheduler – Einschränkungen und Standardwerte
## Kontingente, Einschränkungen, Standardwerte und Drosselungen für Scheduler
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## Der Header „x-ms-request-id“
Jede Anforderung für den Scheduler-Dienst gibt einen Antwortheader namens **x-ms-request-id** zurück. Dieser Header enthält einen opaken Wert, der die Anforderung eindeutig identifiziert.

Wenn bei einer Anforderung kontinuierlich ein Fehler auftritt, obwohl die Anforderung ordnungsgemäß formuliert ist, können Sie den Fehler unter Angabe dieses Werts an Microsoft melden. Geben Sie in Ihrem Bericht den Wert von „x-ms-request-id“, die ungefähre Anforderungszeit, die ID des Abonnements, die Auftragssammlung und/oder den Auftrag sowie die Art des Vorgangs an, den die Anforderung ausgeführt hat.

## Weitere Informationen
 [Was ist Azure Scheduler?](scheduler-intro.md)

 [Konzepte, Terminologie und Entitätshierarchie für Azure Scheduler](scheduler-concepts-terms.md)

 [Erste Schritte mit dem Scheduler im Azure-Portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler-REST-API – Referenz](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler – PowerShell-Cmdlets-Referenz](scheduler-powershell-reference.md)

 [Hochverfügbarkeit und Zuverlässigkeit von Azure Scheduler](scheduler-high-availability-reliability.md)

 [Ausgehende Authentifizierung von Azure Scheduler](scheduler-outbound-authentication.md)

<!---HONumber=AcomDC_0824_2016-->