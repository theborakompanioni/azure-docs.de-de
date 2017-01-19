---
title: "Datenbank auf dem Server ist zurzeit nicht verfügbar. Herstellen einer Verbindung mit SQL-Datenbank | Microsoft Docs"
description: "Problembehandlung des Fehlers „Datenbank auf dem Server ist zurzeit nicht verfügbar.“, wenn eine Anwendung eine Verbindung mit SQL-Datenbank herstellt."
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
keywords: "Datenbank auf dem Server ist zurzeit nicht verfügbar, Herstellen einer Verbindung mit SQL-Datenbank"
ms.assetid: f61999ac-d46b-448a-8830-3b04978d84ec
ms.service: sql-database
ms.custom: troubleshoot
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/20/2017
ms.author: daleche
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ede7672196e71bc493cd9b758982445e991def1b


---
# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>Fehler „Datenbank auf dem Server ist zurzeit nicht verfügbar.“ beim Herstellen einer Verbindung mit SQL-Datenbank
[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Wenn eine Anwendung eine Verbindung zu einer Azure SQL-Datenbank herstellt, wird die folgende Fehlermeldung angezeigt:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [!NOTE]
> Diese ist eine in der Regel vorübergehende (kurzlebige) Fehlermeldung.
> 
> 

Dieser Fehler tritt auf, wenn die Azure-Datenbank verschoben (oder neu konfiguriert) wird und Ihre Anwendung die Verbindung zur SQL-Datenbank verliert. Ereignisse bei der Neukonfiguration von SQL-Datenbanken werden durch geplante Ereignisse (z.B. Softwareupgrades) oder ungeplante Ereignisse (z.B. einen Prozessabsturz oder einen Lastenausgleich) hervorgerufen. Die meisten bei einer Neukonfiguration auftretenden Ereignisse sind normalerweise kurzlebig und sollten innerhalb von höchstens 60 Sekunden beendet werden. Trotzdem kann das Beenden dieser Ereignisse beizeiten länger dauern, so z.B., wenn eine große Transaktion eine lang andauernde Wiederherstellung auslöst.

## <a name="steps-to-resolve-transient-connectivity-issues"></a>Schritte zum Beheben vorübergehender Verbindungsprobleme
1. Ziehen Sie das [Microsoft Azure Service-Dashboard](https://azure.microsoft.com/status) für alle bekannten Ausfälle zu Rat, die während der Fehlermeldung durch die Anwendung aufgetreten sind.
2. Für Anwendungen, die Verbindungen zu einem Clouddienst wie Azure SQL-Datenbank herstellen, sollten Sie regelmäßige Rekonfigurationsereignisse erwarten. Daher sollten Sie eine Wiederholungslogik zur Fehlerbehandlung implementieren, anstatt Benutzern diese Anwendungsfehler anzuzeigen. Weitere Informationen und allgemeine Wiederholungsstrategien finden Sie im Abschnitt [Vorübergehende Fehler](sql-database-connectivity-issues.md) sowie in den bewährten Methoden und Designrichtlinien der [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md). Tiefer gehende Informationen finden Sie anschließend in den Codebeispielen in den [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md).
3. Wenn sich eine Datenbank ihren Ressourcenbegrenzungen nähert, kann dies wie ein vorübergehendes Verbindungsproblem aussehen. Siehe [Problembehandlung bei Leistungsproblemen](sql-database-troubleshoot-performance.md).
4. Wenn Verbindungsprobleme weiterhin bestehen, die Fehlerdauer 60 Sekunden überschreitet oder der Fehler an einem Tag mehrfach auftritt, schicken Sie eine Azure-Supportanfrage, indem Sie auf der **Azure-Support** -Website [Support erhalten](https://azure.microsoft.com/support/options) auswählen.

## <a name="next-steps"></a>Nächste Schritte
* Wenn es sich um einen anderen Fehler handelt, überprüfen Sie die [Fehlermeldung](sql-database-develop-error-messages.md) bezüglich der Hinweise auf mögliche Ursachen.
* Wenn der Fehler dauerhaft ist, hilft Ihnen die Anleitung im Artikel [Behandeln von häufigen Verbindungsproblemen mit Azure SQL-Datenbank](sql-database-troubleshoot-common-connection-issues.md)weiter.




<!--HONumber=Nov16_HO3-->


