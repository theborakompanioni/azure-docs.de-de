<properties
   pageTitle="Technischer Leitfaden zur Resilienz für die Wiederherstellung nach Datenbeschädigung oder versehentlichem Löschen | Microsoft Azure"
   description="In diesem Leitfaden erfahren Sie, wie Sie nach Datenbeschädigung oder versehentlichem Löschen eine Wiederherstellung ausführen, ausfallsichere, hochverfügbare und fehlertolerante Anwendungen erstellen und die Notfallwiederherstellung planen"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="hongfeig"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/13/2016"
   ms.author="patw;jroth;aglick"/>

#Technischer Leitfaden zur Resilienz in Azure – Wiederherstellung nach Datenbeschädigung oder versehentlichem Löschen

Teil eines zuverlässigen Plans für Geschäftskontinuität sind Maßnahmen für den Fall, dass Daten beschädigt oder versehentlich gelöscht werden. Im Folgenden finden Sie Informationen zur Wiederherstellung von Daten, die durch Anwendungs- oder Operatorfehler beschädigt oder versehentlich gelöscht wurden.

##Virtual Machines
Verwenden Sie [Azure Backup](https://azure.microsoft.com/services/backup/), um Azure Virtual Machines (manchmal auch als Infrastructure-as-a-Service-VMs [IaaS] bezeichnet) vor Anwendungsfehlern oder versehentlichem Löschen zu schützen. Mit Azure Backup können Sie über mehrere VM-Datenträger hinweg konsistente Sicherungen erstellen. Darüber hinaus kann der Sicherungstresor über Regionen hinweg repliziert werden, um bei Ausfall einer Region eine Wiederherstellung zu ermöglichen.

##Storage
Beachten Sie, dass Azure Storage zwar Datenresilienz durch automatisierte Replikate sicherstellt, dies jedoch Ihren Anwendungscode (oder Entwickler/Benutzer) nicht vor Datenbeschädigung durch versehentliches oder unbeabsichtigtes Löschen, Aktualisieren usw. schützt. Um Datengenauigkeit bei Anwendungs- oder Benutzerfehlern zu gewährleisten, sind weitergehende Methoden erforderlich, beispielsweise das Kopieren der Daten an einen sekundären Speicherort mit einem Überwachungsprotokoll. Entwickler können von den Vorteilen der Blob-[Momentaufnahmefunktion](https://msdn.microsoft.com/library/azure/ee691971.aspx) profitieren, mit der sie schreibgeschützte Zeitpunkt-Momentaufnahmen von Blobinhalten erstellen können. Diese kann als Grundlage für eine Datengenauigkeitslösung für Azure Storage-Blobs dienen.

###Blob- und Table Storage-Sicherung
Auch wenn Blobs und Tabellen äußerst beständig sind, bilden sie immer den aktuellen Zustand der Daten ab. Wenn Daten ungewollt verändert oder gelöscht wurden, müssen sie möglicherweise im ursprünglichen Zustand wiederhergestellt werden. Verwenden Sie dazu die Azure-Funktionen für das Speichern und Sichern von Zeitpunktkopien.

Für Azure-Blobs können Sie Zeitpunktsicherungen mithilfe der [Blob-Momentaufnahmefunktion](https://msdn.microsoft.com/library/ee691971.aspx) ausführen. Bei jeder Momentaufnahme wird Ihnen nur die erforderliche Speicherkapazität berechnet, die Sie benötigen, um die Veränderungen innerhalb des Blobs seit der letzten Momentaufnahme zu speichern. Die Momentaufnahmen sind davon abhängig, dass das Originalblob vorhanden ist, auf dem sie basieren. Daher wird ein Kopiervorgang auf ein anderes Blob oder sogar ein anderes Speicherkonto empfohlen, damit die Sicherungsdaten vor versehentlichem Löschen geschützt sind. Für Azure-Tabellen können Sie Zeitpunktkopien in eine andere Tabelle oder in Azure-Blobs erstellen. Ausführlichere Anweisungen und Beispiele für das Ausführen von Tabellen- und Blobsicherungen auf Anwendungsebene finden Sie hier:

  * [Schutz Ihrer Tabellen vor Anwendungsfehlern](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/05/03/protecting-your-tables-against-application-errors/)
  * [Schutz Ihrer Blobs vor Anwendungsfehlern](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/04/29/protecting-your-blobs-against-application-errors/)

##Datenbank
Für die Azure SQL-Datenbank stehen Ihnen mehrere Optionen für [Geschäftskontinuität](../sql-database/sql-database-business-continuity.md) (Sicherung, Wiederherstellung) zur Verfügung. Datenbanken können mithilfe der [Datenbankkopierfunktion](../sql-database/sql-database-copy.md) kopiert werden, oder Sie [exportieren](../sql-database/sql-database-export.md) und [importieren](https://msdn.microsoft.com/library/hh710052.aspx) eine SQL Server-BACPAC-Datei. Datenbankkopien liefern transaktionskonsistente Ergebnisse, eine BACPAC-Datei (über den Import-/Export-Dienst) dagegen nicht. Beide Optionen werden als warteschlangenbasierte Dienste innerhalb des Rechenzentrums ausgeführt und bieten derzeit keine SLA für die Abschlusszeit.

>[AZURE.NOTE]Die Datenbankkopier- und Import-/Exportoptionen belasten die Quelldatenbank erheblich und können Ressourcenkonflikte oder Drosselungen auslösen.

###Sichern der SQL-Datenbank
Zeitpunktsicherungen für die Microsoft Azure SQL-Datenbank erfolgen durch das [Kopieren Ihrer Azure SQL-Datenbank](../sql-database/sql-database-copy.md). Sie können diesen Befehl nutzen, um eine transaktionskonsistente Kopie einer Datenbank auf dem gleichen logischen Datenbankserver oder einem anderen Server zu erstellen. Die Datenbankkopie ist in beiden Fällen voll funktionsfähig und vollkommen unabhängig von der Quelldatenbank. Jede erstellte Kopie stellt eine Option für die Zeitpunktwiederherstellung dar. Sie können den Zustand der Datenbank vollständig wiederherstellen, indem Sie der neuen Datenbank den Namen der Quelldatenbank geben. Alternativ können Sie mithilfe von Transact-SQL-Abfragen eine bestimmte Teilmenge von Daten aus der neuen Datenbank wiederherstellen. Weitere Informationen zur SQL-Datenbank finden Sie unter [Geschäftskontinuität für die Cloud und Notfallwiederherstellung für Datenbanken mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md).

###Sichern von SQL Server auf Virtual Machines
Wenn SQL Server mit Azure-IaaS-VMs (Infrastructure-as-a-Service) verwendet wird, gibt es zwei Optionen: herkömmliche Sicherungen und Protokollversand. Die Nutzung herkömmlicher Sicherungen ermöglicht die Wiederherstellung auf einen bestimmten Zeitpunkt – allerdings ist der Wiederherstellungsprozess langsam. Um herkömmliche Sicherungen wiederherzustellen, müssen Sie mit einer vollständigen Erstsicherung beginnen und dann alle nachfolgend durchgeführten Sicherungen anwenden. Die zweite Option besteht in der Konfiguration einer Protokollversandsitzung, um den Wiederherstellungsvorgang von Protokollsicherungen beispielsweise um zwei Stunden zu verzögern. In dem entstandenen Zeitfenster können Fehler in der primären Datenbank behoben werden.

##Weitere Azure Platform-Dienste
Einige Azure Platform-Dienste speichern Informationen in einem benutzergesteuerten Speicherkonto oder in der Azure SQL-Datenbank. Wenn das Konto oder die Speicherressource gelöscht oder beschädigt wird, kann dies zu schwerwiegenden Problemen mit dem Dienst führen. Für diese Fälle sollten Sie unbedingt Sicherungen verwalten, mit denen Sie diese Ressourcen neu erstellen können, falls sie gelöscht oder beschädigt wurden.

Für Azure Websites und Azure Mobile Services müssen Sie die zugeordneten Datenbanken sichern und verwalten. Für Azure Media Service und Virtual Machines müssen Sie das zugeordneten Azure Storage-Konto und alle Ressourcen in diesem Konto verwalten. Für Virtual Machines müssen Sie beispielsweise die VM-Datenträger im Azure-Blobspeicher sichern und verwalten.

##Prüflisten für Datenbeschädigung oder versehentliches Löschen

##Prüfliste für Virtual Machines
  1. Lesen Sie den Abschnitt [Virtual Machines](#virtual-machines) in diesem Dokument
  2. Sichern und verwalten Sie die VM-Datenträger mit Azure Backup (oder Ihrem eigenen Sicherungssystem unter Verwendung von Azure-Blobspeicher und VHD-Momentaufnahmen).

##Prüfliste für Storage
  1. Lesen Sie den Abschnitt [Storage](#storage) in diesem Dokument.
  2. Erstellen Sie regelmäßig Sicherungen wichtiger Speicherressourcen.
  3. Verwenden Sie ggf. die Momentaufnahmefunktion für Blobs

##Prüfliste – Datenbank
  1. Lesen Sie den Abschnitt [Datenbank](#database) in diesem Dokument
  2. Erstellen Sie mit dem Befehl zum Kopieren von Datenbanken Zeitpunktsicherungen

##Prüfliste – Sicherung von SQL Server auf Virtual Machines
  1. Lesen Sie den Abschnitt [Sicherung von SQL Server auf Virtual Machines](#sql-server-on-virtual-machines-backup) in diesem Dokument
  2. Verwenden Sie die herkömmlichen Sicherungs- und Wiederherstellungsverfahren
  3. Erstellen Sie eine verzögerte Protokollversandsitzung

##Prüfliste – Web-Apps
  1. Sichern und verwalten Sie die zugeordnete Datenbank, falls vorhanden

##Prüfliste – Media Services
  1. Sichern und verwalten Sie die zugeordneten Speicherressourcen.

##Weitere Informationen
Weitere Informationen zu den Sicherungs- und Wiederherstellungsfunktionen in Azure finden Sie unter [Speicher-, Sicherungs- und Wiederherstellungsszenarien](https://azure.microsoft.com/documentation/scenarios/storage-backup-recovery/).

##Nächste Schritte
Dieser Artikel ist Teil einer Serie mit dem Schwerpunkt [Technischer Leitfaden zur Resilienz in Azure](./resiliency-technical-guidance.md). Weitere Ressourcen zu Resilienz, Notfallwiederherstellung und hoher Verfügbarkeit finden Sie unter [Weitere Ressourcen](./resiliency-technical-guidance.md#additional-resources) zum Technischen Leitfaden zur Resilienz in Azure.

<!---HONumber=AcomDC_0525_2016-->