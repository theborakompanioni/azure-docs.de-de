---
title: Onlinesicherung und -wiederherstellung mit Azure Cosmos DB | Microsoft-Dokumentation
description: "Erfahren Sie mehr über das automatische Sichern und Wiederherstellen einer Azure Cosmos DB-Datenbank."
keywords: Sicherung und Wiederherstellung, Onlinesicherung
services: cosmos-db
documentationcenter: 
author: RahulPrasad16
manager: jhubbard
editor: monicar
ms.assetid: 98eade4a-7ef4-4667-b167-6603ecd80b79
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/23/2017
ms.author: raprasa
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: a438b5079ae48c82fb2dbd5ce4547302364e0ef5
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017


---
# <a name="automatic-online-backup-and-restore-with-azure-cosmos-db"></a>Automatische Onlinesicherung und -wiederherstellung mit Azure Cosmos DB
Azure Cosmos DB erstellt in regelmäßigen Abständen automatisch Sicherungen aller Daten. Die automatischen Sicherungen erfolgen ohne Beeinträchtigung der Leistung oder Verfügbarkeit des Betriebs Ihrer Datenbanken. Alle Sicherungskopien werden in einem anderen Speicherdienst getrennt gespeichert, und diese Sicherungen werden zum besseren Schutz vor regionalen Ausfällen global repliziert. Die automatischen Sicherungen sind für den Fall vorgesehen, dass Sie Ihren Cosmos DB-Container versehentlich löschen und später eine Daten- oder Notfallwiederherstellungslösung benötigen sollten.  

Dieser Artikel beginnt mit einer Kurzübersicht über die Redundanz und Verfügbarkeit von Daten in Cosmos DB, an die sich eine Erörterung von Sicherungen anschließt. 

## <a name="high-availability-with-cosmos-db---a-recap"></a>Hohe Verfügbarkeit mit Cosmos DB: Kurzübersicht
Cosmos DB ist auf eine [globale Verteilung](distribute-data-globally.md) ausgelegt und ermöglicht eine Skalierung des Durchsatzes in mehreren Azure-Regionen sowie ein durch Richtlinien gesteuertes Failover und transparente Multihosting-APIs. In einem Datenbanksystem, das [SLAs für eine Verfügbarkeit von 99,99 Prozent](https://azure.microsoft.com/support/legal/sla/cosmos-db) bietet, werden alle Schreibvorgänge in Cosmos DB mithilfe eines Quorums von Replikaten in einem lokalen Rechenzentrum beständig auf lokalen Datenträgern gespeichert, bevor der Client eine Bestätigung erhält. Beachten Sie, dass die hohe Verfügbarkeit von Cosmos DB von lokalem Speicher und nicht von externen Speichertechnologien abhängig ist. Wenn Ihr Konto darüber hinaus mehreren Azure-Regionen zugeordnet ist, werden Ihre Schreibvorgänge auch in andere Regionen repliziert. Um Ihren Durchsatz zu skalieren und mit kurzer Latenz auf Daten zuzugreifen, können Sie in Ihrem Datenbankkonto über so viele Leseregionen wie gewünscht verfügen. In jeder Leseregion werden die (replizierten) Daten dauerhaft in einer Replikatgruppe gespeichert.  

Wie im folgenden Diagramm dargestellt, wird ein einzelner Cosmos DB-Container [horizontal partitioniert](partition-data.md). Eine „Partition“ ist im folgenden Diagramm durch einen Kreis dargestellt, und jede Partition bietet mithilfe einer Replikatgruppe hohe Verfügbarkeit. Dies ist die lokale Verteilung innerhalb einer einzelnen Azure-Region (siehe die x-Achse). Zusätzlich wird jede Partition (samt zugehöriger Replikatgruppe) anschließend auf mehrere Regionen verteilt, die Ihrem Datenbankkonto zugeordnet sind (in dieser Abbildung z.B. auf die drei Regionen USA, Osten, USA, Westen und Indien, Mitte). Die „Partitionsgruppe“ ist eine global verteilte Entität, die aus mehreren Kopien Ihrer Daten in jeder Region besteht (siehe die Y-Achse). Sie können den Ihrem Datenbankkonto zugeordneten Regionen eine Priorität zuweisen. Bei einem Notfall führt Cosmos DB ein transparentes Failover in die nächste Region durch. Sie können ein Failover auch manuell simulieren, um zu testen, ob Ihre Anwendung lückenlos verfügbar ist.  

Die folgende Abbildung veranschaulicht das hohe Maß an Redundanz dank Cosmos DB.

![Hohes Maß an Redundanz mit Cosmos DB](./media/online-backup-and-restore/redundancy.png)

![Hohes Maß an Redundanz mit Cosmos DB](./media/online-backup-and-restore/global-distribution.png)

## <a name="full-automatic-online-backups"></a>Vollständige, automatische Onlinesicherungen
Huch, ich habe leider meinen Container bzw. meine Datenbank gelöscht! Cosmos DB sorgt nicht nur dafür, dass Ihre Daten, sondern auch die Sicherungen Ihrer Daten überaus redundant und gegen regionale Katastrophen geschützt sind. Diese automatisierten Sicherungen werden derzeit ungefähr alle vier Stunden ausgeführt, und es werden immer die beiden neuesten Sicherungen gespeichert. Wenn die Daten aus Versehen gelöscht oder beschädigt werden, wenden Sie sich innerhalb von acht Stunden an den [Azure-Support](https://azure.microsoft.com/support/options/). 

Die Sicherungen erfolgen ohne Beeinträchtigung der Leistung oder Verfügbarkeit Ihrer Datenbanken. Cosmos DB erstellt die Sicherung im Hintergrund, ohne Ihre bereitgestellten Anforderungseinheiten zu beanspruchen bzw. die Leistung oder die Verfügbarkeit Ihrer Datenbank zu beeinträchtigen. 

Im Gegensatz zu Ihren Daten, die in Cosmos DB gespeichert sind, werden automatische Sicherungen im Azure Blob Storage-Dienst gespeichert. Um niedrige Latenz und ein effizientes Hochladen zu gewährleisten, wird die Momentaufnahme Ihrer Sicherung in eine Instanz von Azure Blob Storage in der Region hochgeladen, in der sich auch die aktuelle Schreibregion Ihres Cosmos DB-Datenbankkontos befindet. Zum besseren Schutz vor regionalen Katastrophen wird jede Momentaufnahme Ihrer Sicherungsdaten in Azure Blob Storage nochmals mithilfe von georedundantem Speicher (GRS) in eine andere Region repliziert. Das folgende Diagramm zeigt, dass der gesamte Cosmos DB-Container (mit allen drei primären Partitionen in USA, Westen in diesem Beispiel) in einem von Azure Blob Storage-Remotekonto in USA, Westen gesichert wird und anschließend mithilfe von GRS nach USA, Osten repliziert wird. 

Das folgende Bild veranschaulicht regelmäßige vollständige Sicherungen aller Cosmos DB-Entitäten in georedundantem Azure Storage.

![Regelmäßige vollständige Sicherungen aller Cosmos DB-Entitäten in georedundantem Azure Storage](./media/online-backup-and-restore/automatic-backup.png)

## <a name="retention-period-for-a-given-snapshot"></a>Beibehaltungsdauer einer bestimmten Momentaufnahme
Wie oben beschrieben erstellen wir alle 4 Stunden Momentaufnahmen Ihrer Daten und behalten die letzten zwei Momentaufnahmen 30 Tage lang bei. Gemäß unseren Konformitätsbestimmungen werden Momentaufnahmen nach 90 Tagen gelöscht.

Wenn Sie eigene Momentaufnahmen beibehalten möchten, können Sie die Option zum Export in eine JSON-Datei im [Datenmigrationstool](import-data.md#export-to-json-file) Azure Cosmos DB verwenden, um zusätzliche Sicherungen zu planen. 

## <a name="restore-database-from-the-online-backup"></a>Wiederherstellen einer Datenbank aus der Onlinesicherung
Für den Fall, dass Sie Ihre Daten versehentlich löschen, können Sie [ein Supportticket anfordern](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) oder den [Azure Support bitten](https://azure.microsoft.com/support/options/), die Daten aus der letzten automatischen Sicherung wiederherzustellen. Damit eine bestimmte Momentaufnahme Ihrer Sicherung wiederhergestellt wird, fordert Cosmos DB an, dass die Daten mindestens für die Dauer des Sicherungszyklus dieser Momentaufnahme bei uns verfügbar waren.

## <a name="next-steps"></a>Nächste Schritte

Wie Sie die Datenbank in mehrere Rechenzentren replizieren, erfahren Sie unter [Globale Verteilung von Daten mit Cosmos DB](distribute-data-globally.md). 

Zum Kontaktieren des Azure-Supports [fordern Sie im Azure-Portal ein Ticket an](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


