<properties
   pageTitle="Technischer Leitfaden zur Resilienz für die Wiederherstellung nach Datenbeschädigung oder versehentlichem Löschen | Microsoft Azure"
   description="In diesem Leitfaden erfahren Sie, wie Sie nach Datenbeschädigung oder versehentlichem Löschen eine Wiederherstellung ausführen, ausfallsichere, hochverfügbare und fehlertolerante Anwendungen erstellen und die Notfallwiederherstellung planen"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#Technischer Leitfaden zur Resilienz in Azure – Wiederherstellung nach Datenbeschädigung oder versehentlichem Löschen

Teil eines zuverlässigen Plans für Geschäftskontinuität sind Maßnahmen für den Fall, dass Daten beschädigt oder versehentlich gelöscht werden. Im Anschluss finden Sie Informationen zur Wiederherstellung von Daten, die durch Anwendungs- oder Bedienerfehler beschädigt oder versehentlich gelöscht wurden.

##Virtual Machines

Verwenden Sie [Azure Backup](https://azure.microsoft.com/services/backup/), um Ihre virtuellen Azure-Computer (manchmal auch als Infrastructure-as-a-Service-VMs bezeichnet) vor Anwendungsfehlern oder versehentlichem Löschen zu schützen. Mit Azure Backup können Sie über mehrere VM-Datenträger hinweg konsistente Sicherungen erstellen. Darüber hinaus kann der Sicherungstresor über Regionen hinweg repliziert werden, um bei Ausfall einer Region eine Wiederherstellung zu ermöglichen.

##Speicher

Beachten Sie, dass Azure Storage zwar Datenresilienz durch automatisierte Replikate sicherstellt, dies jedoch Ihren Anwendungscode (oder Entwickler/Benutzer) nicht vor Datenbeschädigung durch versehentliches oder unbeabsichtigtes Löschen, Aktualisieren usw. schützt. Um Datengenauigkeit bei Anwendungs- oder Benutzerfehlern zu gewährleisten, sind weitergehende Methoden erforderlich – beispielsweise das Kopieren der Daten an einen sekundären Speicherort mit einem Überwachungsprotokoll. Entwickler können von den Vorteilen der [Blob-Momentaufnahmefunktion](https://msdn.microsoft.com/library/azure/ee691971.aspx) profitieren, mit der sie schreibgeschützte Momentaufnahmen von Blobinhalten erstellen können. Diese kann als Grundlage für eine Datengenauigkeitslösung für Azure Storage-Blobs dienen.

###Blob- und Table Storage-Sicherung

Auch wenn Blobs und Tabellen äußerst beständig sind, bilden sie immer den aktuellen Zustand der Daten ab. Wenn Daten ungewollt verändert oder gelöscht wurden, müssen sie möglicherweise im ursprünglichen Zustand wiederhergestellt werden. Verwenden Sie dazu die Azure-Funktionen für das Speichern und Sichern von Zeitpunktkopien.

Für Azure-Blobs können Sie Zeitpunktsicherungen mithilfe der [Blob-Momentaufnahmefunktion](https://msdn.microsoft.com/library/ee691971.aspx) ausführen. Bei jeder Momentaufnahme wird Ihnen nur die erforderliche Speicherkapazität berechnet, die Sie benötigen, um die Veränderungen innerhalb des Blobs seit der letzten Momentaufnahme zu speichern. Die Momentaufnahmen sind von der Existenz des zugrunde liegenden Originalblobs abhängig, sodass ein Kopiervorgang in ein anderes Blob oder sogar in ein anderes Speicherkonto empfohlen wird. Dadurch sind die Sicherungsdaten dann ordnungsgemäß vor versehentlichem Löschen geschützt. Für Azure-Tabellen können Sie Zeitpunktkopien in eine andere Tabelle oder in Azure-Blobs erstellen. Ausführlichere Anweisungen und Beispiele für das Ausführen von Tabellen- und Blobsicherungen auf Anwendungsebene finden Sie hier:

  * [Schutz Ihrer Tabellen vor Anwendungsfehlern](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/05/03/protecting-your-tables-against-application-errors/)
  * [Schutz Ihrer Blobs vor Anwendungsfehlern](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/04/29/protecting-your-blobs-against-application-errors/)

##Datenbank

Für die Azure SQL-Datenbank stehen Ihnen mehrere Optionen für [Geschäftskontinuität](../sql-database/sql-database-business-continuity.md) (Sicherung, Wiederherstellung) zur Verfügung. Datenbanken können mithilfe der Funktion [Datenbankkopie](../sql-database/sql-database-copy.md) oder durch [Exportieren](../sql-database/sql-database-export.md) und [Importieren](https://msdn.microsoft.com/library/hh710052.aspx) einer SQL Server-BACPAC-Datei kopiert werden. Datenbankkopien liefern transaktionskonsistente Ergebnisse, eine BACPAC-Datei (über den Import-/Export-Dienst) dagegen nicht. Beide Optionen werden als warteschlangenbasierte Dienste innerhalb des Datencenters ausgeführt und bieten derzeit keine SLA für die Ausführungsdauer.

>[AZURE.NOTE]Das Kopieren der Datenbank und das Importieren/Exportieren stellen eine erhebliche Belastung für die Quelldatenbank dar. Dies kann zu Ressourcenkonflikten oder Leistungseinbußen führen.

###Sichern der SQL-Datenbank

Zeitpunktsicherungen für die Microsoft Azure SQL-Datenbank erfolgen durch das [Kopieren Ihrer Azure SQL-Datenbank](../sql-database/sql-database-copy.md). Mit diesem Befehl können Sie auf dem gleichen logischen Datenbankserver oder auf einem anderen Server eine transaktionskonsistente Kopie einer Datenbank erstellen. Die Datenbankkopie ist in beiden Fällen voll funktionsfähig und vollkommen unabhängig von der Quelldatenbank. Jede erstellte Kopie stellt eine Option für die Zeitpunktwiederherstellung dar. Sie können den Zustand der Datenbank vollständig wiederherstellen, indem Sie der neuen Datenbank den Namen der Quelldatenbank geben. Alternativ können Sie mithilfe von Transact-SQL-Abfragen eine bestimmte Teilmenge von Daten aus der neuen Datenbank wiederherstellen. Weitere Informationen zu SQL-Datenbank finden Sie in der [Übersicht über die Geschäftskontinuität mit Azure SQL-Datenbank](../sql-database/sql-database-business-continuity.md).

###Sichern von SQL Server auf Virtual Machines

Wenn SQL Server mit virtuellen Infrastructure-as-a-Service-Azure-Computern (oftmals auch IaaS oder IaaS-VMs genannt) verwendet wird, stehen zwei Optionen zur Verfügung: herkömmliche Sicherungen und Protokollversand. Die Nutzung herkömmlicher Sicherungen ermöglicht die Wiederherstellung auf einen bestimmten Zeitpunkt – allerdings ist der Wiederherstellungsprozess langsam. Um herkömmliche Sicherungen wiederherzustellen, müssen Sie mit einer vollständigen Erstsicherung beginnen und dann alle nachfolgend durchgeführten Sicherungen anwenden. Bei der zweiten Option wird eine Protokollversandsitzung konfiguriert, um die Wiederherstellung von Protokollsicherungen zu verzögern (beispielsweise um zwei Stunden). In dem entstandenen Zeitfenster können Fehler in der primären Datenbank behoben werden.

##Weitere Azure Platform-Dienste

Einige Azure Platform-Dienste speichern Informationen in einem benutzergesteuerten Speicherkonto oder in der Azure SQL-Datenbank. Wenn das Konto oder die Speicherressource gelöscht oder beschädigt wird, kann dies zu schwerwiegenden Problemen mit dem Dienst führen. In einem solchen Fall sollten Sie unbedingt über Sicherungen verfügen, mit denen Sie diese Ressourcen neu erstellen können, falls sie gelöscht oder beschädigt wurden.

Für Azure Websites und Azure Mobile Services müssen Sie die zugeordneten Datenbanken sichern und verwalten. Für Azure Media Service und Virtual Machines müssen Sie das zugeordneten Azure Storage-Konto und alle Ressourcen in diesem Konto verwalten. Für Virtual Machines müssen Sie beispielsweise die VM-Datenträger im Azure-Blobspeicher sichern und verwalten.

##Prüflisten für Datenbeschädigung oder versehentliches Löschen

##Prüfliste – Virtual Machines

  1. Lesen Sie den Abschnitt zu Virtual Machines in diesem Dokument.
  2. Sichern und verwalten Sie die VM-Datenträger mit Azure Backup (oder Ihrem eigenen Sicherungssystem unter Verwendung von Azure-Blobspeicher und VHD-Momentaufnahmen).

##Prüfliste – Speicher

  1. Lesen Sie den Abschnitt zu Storage in diesem Dokument.
  2. Erstellen Sie regelmäßig Sicherungen wichtiger Speicherressourcen.
  3. Verwenden Sie ggf. die Momentaufnahmefunktion für Blobs.

##Prüfliste – Datenbank

  1. Lesen Sie den Abschnitt zur Datenbank in diesem Dokument.
  2. Erstellen Sie mit dem Befehl zum Kopieren von Datenbanken Zeitpunktsicherungen.

##Prüfliste – Sicherung von SQL Server auf Virtual Machines

  1. Lesen Sie den Abschnitt zur Sicherung von SQL Server auf Virtual Machines in diesem Dokument.
  2. Verwenden Sie die üblichen Sicherungs- und Wiederherstellungsverfahren.
  3. Erstellen Sie eine verzögerte Protokollversandsitzung.

##Prüfliste – Web-Apps

  1. Sichern und verwalten Sie die zugeordnete Datenbank, falls vorhanden.

##Prüfliste – Media Services

  1. Sichern und verwalten Sie die zugeordneten Speicherressourcen.

##Weitere Informationen

Weitere Informationen zu den Sicherungs- und Wiederherstellungsfeatures in Azure finden Sie unter [Speicher-, Sicherungs- und Wiederherstellungsszenarien](https://azure.microsoft.com/documentation/scenarios/storage-backup-recovery/).

##Nächste Schritte

Dieser Artikel gehört zu einer Reihe von Artikeln, die als [Technischer Leitfaden zur Resilienz in Azure](./resiliency-technical-guidance.md) dienen. Weitere Ressourcen zu Resilienz, Notfallwiederherstellung und hoher Verfügbarkeit finden Sie im technischen Leitfaden zur Resilienz in Azure unter [Weitere Ressourcen](./resiliency-technical-guidance.md#additional-resources).

<!---HONumber=AcomDC_0824_2016-->