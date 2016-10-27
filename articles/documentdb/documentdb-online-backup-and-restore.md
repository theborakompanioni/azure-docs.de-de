<properties
    pageTitle="Onlinesicherung und -wiederherstellung mit DocumentDB | Microsoft Azure"
    description="Erfahren Sie mehr über das automatische Sichern und Wiederherstellen von NoSQL-Datenbanken mit Azure DocumentDB."
    keywords="Sicherung und Wiederherstellung, Onlinesicherung"
    services="documentdb"
    documentationCenter=""
    authors="RahulPrasad16"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="09/23/2016"
    ms.author="raprasa"/>


# <a name="automatic-online-backup-and-restore-with-documentdb"></a>Automatische Onlinesicherung und -wiederherstellung mit DocumentDB 

Azure DocumentDB erstellt in regelmäßigen Abständen automatisch Sicherungen aller Daten. Die automatischen Sicherungen erfolgen ohne Beeinträchtigung der Leistung oder Verfügbarkeit des Betriebs Ihrer NoSQL-Datenbanken. Alle Sicherungskopien werden in einem anderen Speicherdienst getrennt gespeichert, und diese Sicherungen werden zum besseren Schutz vor regionalen Ausfällen global repliziert. Die automatischen Sicherungen sind für den Fall vorgesehen, dass Sie Ihre DocumentDB-Sammlung versehentlich löschen und später eine Daten- oder Notfallwiederherstellungslösung benötigen sollten.  

Dieser Artikel beginnt mit einer Kurzübersicht über die Redundanz und Verfügbarkeit von Daten in DocumentDB, an die sich eine Erörterung von Sicherungen anschließt. 

## <a name="high-availability-with-documentdb---a-recap"></a>Hohe Verfügbarkeit mit DocumentDB: Kurzübersicht

DocumentDB ist auf eine [globale Verteilung](documentdb-distribute-data-globally.md) ausgelegt und ermöglicht eine Skalierung des Durchsatzes in mehreren Azure-Regionen sowie ein durch Richtlinien gesteuertes Failover und transparente Multihosting-APIs. In einem Datenbanksystem, das [SLAs für eine Verfügbarkeit von 99,99 Prozent](https://azure.microsoft.com/support/legal/sla/documentdb/v1_0/) bietet, werden alle Schreibvorgänge in DocumentDB mithilfe eines Quorums von Replikaten in einem lokalen Rechenzentrum beständig auf lokalen Datenträgern gespeichert, bevor der Client eine Bestätigung erhält. Beachten Sie, dass die hohe Verfügbarkeit von DocumentDB von lokalem Speicher und nicht von externen Speichertechnologien abhängig ist. Wenn Ihr Konto darüber hinaus mehreren Azure-Regionen zugeordnet ist, werden Ihre Schreibvorgänge auch in andere Regionen repliziert. Um Ihren Durchsatz zu skalieren und mit kurzer Latenz auf Daten zuzugreifen, können Sie in Ihrem Datenbankkonto über so viele Leseregionen wie gewünscht verfügen. In jeder Leseregion werden die (replizierten) Daten dauerhaft in einer Replikatgruppe gespeichert.  

Wie im folgenden Diagramm dargestellt, wird eine einzelne DocumentDB-Sammlung [horizontal partitioniert](documentdb-partition-data.md). Eine „Partition“ ist im folgenden Diagramm durch einen Kreis dargestellt, und jede Partition bietet mithilfe einer Replikatgruppe hohe Verfügbarkeit. Dies ist die lokale Verteilung innerhalb einer einzelnen Azure-Region (siehe die x-Achse). Zusätzlich wird jede Partition (samt zugehöriger Replikatgruppe) anschließend auf mehrere Regionen verteilt, die Ihrem Datenbankkonto zugeordnet sind (in dieser Abbildung z.B. auf die drei Regionen USA, Osten, USA, Westen und Indien, Mitte). Die „Partitionsgruppe“ ist eine global verteilte Entität, die aus mehreren Kopien Ihrer Daten in jeder Region besteht (siehe die Y-Achse). Sie können den Ihrem Datenbankkonto zugeordneten Regionen eine Priorität zuweisen. Bei einem Notfall führt DocumentDB ein transparentes Failover in die nächste Region durch. Sie können ein Failover auch manuell simulieren, um zu testen, ob Ihre Anwendung lückenlos verfügbar ist.  

Die folgende Abbildung veranschaulicht das hohe Maß an Redundanz dank DocumentDB.

![Hohes Maß an Redundanz mit DocumentDB](./media/documentdb-online-backup-and-restore/azure-documentdb-nosql-database-redundancy.png)


![Hohes Maß an Redundanz mit DocumentDB](./media/documentdb-online-backup-and-restore/azure-documentdb-nosql-database-global-distribution.png)

## <a name="full,-automatic,-online-backups"></a>Vollständige, automatische Onlinesicherungen

Huch, ich habe leider meine Sammlung bzw. Datenbank gelöscht! DocumentDB sorgt nicht nur dafür, dass Ihre Daten, sondern auch die Sicherungen Ihrer Daten überaus redundant und gegen regionale Katastrophen geschützt sind. Diese automatisierte Sicherungen werden derzeit ungefähr alle vier Stunden ausgeführt. 

Die Sicherungen erfolgen ohne Beeinträchtigung der Leistung oder Verfügbarkeit Ihrer Datenbanken. DocumentDB erstellt die Sicherung im Hintergrund, ohne Ihre bereitgestellten Anforderungseinheiten zu beanspruchen bzw. die Leistung oder die Verfügbarkeit Ihrer NoSQL-Datenbank zu beeinträchtigen. 

Im Gegensatz zu Ihren Daten, die in DocumentDB gespeichert sind, werden automatische Sicherungen im Azure Blob Storage-Dienst gespeichert. Um niedrige Latenz und ein effizientes Hochladen zu gewährleisten, wird die Momentaufnahme Ihrer Sicherung in eine Instanz von Azure Blob Storage in der Region hochgeladen, in der sich auch die aktuelle Schreibregion Ihres DocumentDB-Datenbankkontos befindet. Zum besseren Schutz vor regionalen Katastrophen wird jede Momentaufnahme Ihrer Sicherungsdaten in Azure Blob Storage nochmals mithilfe von georedundantem Speicher (GRS) in eine andere Region repliziert. Das folgende Diagramm zeigt, dass die gesamte DocumentDB-Sammlung (mit allen drei primären Partitionen in USA, Westen in diesem Beispiel) in einem von Azure Blob Storage-Remotekonto in USA, Westen gesichert wird und anschließend mithilfe von GRS nach USA, Osten repliziert wird. 

Das folgende Bild veranschaulicht regelmäßige vollständige Sicherungen aller DocumentDB-Entitäten in georedundantem Azure-Speicher.

![Regelmäßige vollständige Sicherungen aller DocumentDB-Entitäten in georedundantem Azure-Speicher](./media/documentdb-online-backup-and-restore/azure-documentdb-nosql-database-automatic-backup.png)


## <a name="retention-period-for-a-given-snapshot"></a>Beibehaltungsdauer einer bestimmten Momentaufnahme

Wie oben beschrieben, erstellen wir in regelmäßigen Abständen Momentaufnahmen Ihrer Daten. Gemäß unseren Vorschriften wird die letzte Momentaufnahme bis zu 90 Tage aufbewahrt, ehe sie endgültig gelöscht wird. Wenn eine Sammlung oder ein Konto gelöscht wird, bleibt die letzte Sicherung von DocumentDB 90 Tage gespeichert.

## <a name="restore-database-from-the-online-backup"></a>Wiederherstellen einer Datenbank aus der Onlinesicherung

Für den Fall, dass Sie Ihre Daten versehentlich löschen, können Sie [ein Supportticket anfordern](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) oder den [Azure Support bitten](https://azure.microsoft.com/support/options/), die Daten aus der letzten automatischen Sicherung wiederherzustellen. Damit eine bestimmte Momentaufnahme Ihrer Sicherung wiederhergestellt wird, fordert DocumentDB an, dass die Daten mindestens für die Dauer des Sicherungszyklus dieser Momentaufnahme bei uns verfügbar waren.

## <a name="next-steps"></a>Nächste Schritte

Wie Sie die NoSQL-Datenbank in mehrere Rechenzentren replizieren, erfahren Sie unter [Globale Verteilung von Daten mit DocumentDB](documentdb-distribute-data-globally.md). 

Zum Kontaktieren des Azure-Supports [fordern Sie im Azure-Portal ein Ticket an](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).


<!--HONumber=Oct16_HO2-->


