---
title: "Scheduler – Einschränkungen und Standardwerte"
description: "Scheduler – Einschränkungen und Standardwerte"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: db6b1c196cb468f41c7a7ce34758de346b522abb


---
# <a name="scheduler-limits-and-defaults"></a>Scheduler – Einschränkungen und Standardwerte
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>Kontingente, Einschränkungen, Standardwerte und Drosselungen für Scheduler
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="the-x-ms-request-id-header"></a>Der Header „x-ms-request-id“
Jede Anforderung für den Scheduler-Dienst gibt einen Antwortheader namens**x-ms-request-id**zurück. Dieser Header enthält einen opaken Wert, der die Anforderung eindeutig identifiziert.

Wenn bei einer Anforderung kontinuierlich ein Fehler auftritt, obwohl die Anforderung ordnungsgemäß formuliert ist, können Sie den Fehler unter Angabe dieses Werts an Microsoft melden. Geben Sie in Ihrem Bericht den Wert von „x-ms-request-id“, die ungefähre Anforderungszeit, die ID des Abonnements, die Auftragssammlung und/oder den Auftrag sowie die Art des Vorgangs an, den die Anforderung ausgeführt hat.

## <a name="see-also"></a>Weitere Informationen
 [Was ist Azure Scheduler?](scheduler-intro.md)

 [Konzepte, Terminologie und Entitätshierarchie für Azure Scheduler](scheduler-concepts-terms.md)

 [Erste Schritte mit dem Scheduler im Azure-Portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler-REST-API – Referenz](https://msdn.microsoft.com/library/mt629143)

 [Azure Scheduler – PowerShell-Cmdlets-Referenz](scheduler-powershell-reference.md)

 [Hochverfügbarkeit und Zuverlässigkeit von Azure Scheduler](scheduler-high-availability-reliability.md)

 [Ausgehende Authentifizierung von Azure Scheduler](scheduler-outbound-authentication.md)




<!--HONumber=Nov16_HO3-->


