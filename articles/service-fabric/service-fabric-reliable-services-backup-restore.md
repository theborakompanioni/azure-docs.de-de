---
title: Sichern und Wiederherstellen mit Service Fabric | Microsoft Docs
description: Dokumentation zum Sichern und Wiederherstellen mit Service Fabric
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: subramar,jessebenson

ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/19/2016
ms.author: mcoskun

---
# Sichern und Wiederherstellen von Reliable Services und Reliable Actors
Azure Service Fabric ist eine Plattform mit hoher Verfügbarkeit, bei der der Status über mehrere Knoten repliziert wird, um diese hohe Verfügbarkeit zu gewährleisten. Auch wenn ein Knoten im Cluster ausfällt, bleiben die Dienste somit verfügbar. Diese von der Plattform bereitgestellte integrierte Redundanz reicht in manchen Fällen aus. In bestimmten Fällen wäre es jedoch wünschenswert, dass der Dienst Daten (auf einem externen Speicher) sichert.

> [!NOTE]
> Das Sichern und die Möglichkeit des Wiederherstellens Ihrer Daten (sowie die Überprüfung, ob diese Vorgänge wie erwartet ausgeführt werden) sind wichtig, damit Sie sie bei Datenverlust wiederherstellen können.
> 
> 

Ein Dienst möchte Daten beispielsweise in den folgenden Szenarien sichern:

* Bei dauerhaftem Verlust eines gesamten Service Fabric-Clusters oder aller Knoten, die eine bestimmte Partition ausführen.
* Administrative Fehler, durch die der Zustand versehentlich gelöscht oder beschädigt wird. Das kann z. B. passieren, wenn ein Administrator mit ausreichenden Berechtigungen versehentlich den Dienst löscht.
* Fehler im Dienst, die zu einer Beschädigung von Daten führen. Dies kann beispielsweise bei einem Dienstcode-Upgrade geschehen, bei dem fehlerhafte Daten in eine Reliable Collection geschrieben werden. In diesem Fall müssen unter Umständen der Code und die Daten in einen früheren Zustand zurückversetzt werden.
* Offline-Datenverarbeitung. Es kann zweckmäßig sein, Daten für Business Intelligence separat von dem Dienst, der die Daten generiert, offline zu verarbeiten.

Das Feature für die Sicherung/Wiederherstellung ermöglicht mit Reliable Services-API entwickelten Diensten die Erstellung und Wiederherstellung von Sicherungen. Die von der Plattform bereitgestellten Sicherungs-APIs ermöglichen das Anlegen von Sicherungen des Status einer Dienstpartition, ohne Lese- oder Schreibvorgänge zu blockieren. Die Wiederherstellungs-APIs ermöglichen die Wiederherstellung des Status einer Dienstpartition aus einer Sicherung.

## Sicherungstypen
Es stehen zwei Optionen zur Sicherung zur Verfügung: vollständige Sicherung und inkrementelle Sicherung. Eine vollständige Sicherung ist eine Sicherung, die alle Daten umfasst, die zum Neuerstellen des Status des Replikats erforderlich sind, d.h. Prüfpunkte und alle Protokolleinträge. Da eine vollständige Sicherung die Prüfpunkte und das Protokoll umfasst, kann sie eigenständig wiederhergestellt werden.

Probleme mit vollständigen Sicherungen entstehen, wenn die Prüfpunkte umfangreich sind. Ein Replikat mit einem Status von 16 GB weist beispielsweise Prüfpunkte auf, die sich auf ca. 16 GB belaufen. Bei einer Recovery Point Objective von 5 Minuten muss das Replikat alle 5 Minuten gesichert werden. Bei jeder Sicherung müssen neben Protokollen von 50 MB (konfigurierbar mit **CheckpointThresholdInMB**) Prüfpunkte von 16 GB kopiert werden.

![Beispiel für vollständige Sicherung.](media/service-fabric-reliable-services-backup-restore/FullBackupExample.PNG)

Die Lösung für dieses Problem besteht darin, inkrementelle Sicherungen durchzuführen, bei denen nur die seit der letzten Sicherung erfassten Protokolleinträge gesichert werden.

![Beispiel für inkrementelle Sicherung.](media/service-fabric-reliable-services-backup-restore/IncrementalBackupExample.PNG)

Da inkrementelle Sicherungen nur die seit der letzten Sicherung vorgenommenen Änderungen umfassen (ohne die Prüfpunkte), werden sie meistens schneller durchgeführt, können aber nicht eigenständig wiederhergestellt werden. Zum Wiederherstellen einer inkrementellen Sicherung ist die gesamte Sicherungskette erforderlich. Eine Sicherungskette ist eine Kette von Sicherungen, die mit einer vollständigen Sicherung beginnt, auf die dann mehrere zusammenhängende inkrementelle Sicherungen folgen.

## Sichern von Reliable Services
Der Dienstautor kann bestimmen, wann Sicherungen erstellt und wo sie gespeichert werden.

Der Dienst muss die geerbte Memberfunktion **BackupAsync** aufrufen, um die Sicherung zu starten. Sicherungen können nur von primären Replikaten erstellt werden und benötigen Schreibzugriff.

Wie Sie unten sehen, akzeptiert **BackupAsync** ein **BackupDescription**-Objekt, in dem Sie eine vollständige oder inkrementelle Sicherung angeben können, sowie die Rückruffunktion **Func<< BackupInfo, CancellationToken, Task<bool>>>**. Diese wird aufgerufen, wenn der Sicherungsordner lokal erstellt wurde und in einen externen Speicher verschoben werden kann.

```C#

BackupDescription myBackupDescription = new BackupDescription(backupOption.Incremental,this.BackupCallbackAsync);

await this.BackupAsync(myBackupDescription);

```

Bei der Anforderung zum Durchführen einer inkrementellen Sicherung kann der Fehler **FabricMissingFullBackupException** auftreten. Dies weist darauf hin, dass entweder niemals eine vollständige Sicherung für das Replikat erstellt wurde oder dass einige der Protokolleinträge seit der letzten Sicherung abgeschnitten wurden. Benutzer können durch Ändern von **CheckpointThresholdInMB** anpassen, mit welcher Rate Protokolleinträge abgeschnitten werden sollen.

**BackupInfo** liefert Informationen über die Sicherung, einschließlich des Speicherorts des Ordners, in dem die Runtime die Sicherung gespeichert hat (**BackupInfo.Directory**). Die Rückruffunktion kann **BackupInfo.Directory** in einen externen Speicher oder an einen anderen Speicherort verschieben. Diese Funktion gibt auch einen booleschen Wert zurück, der angibt, ob der Sicherungsordner erfolgreich an den Zielspeicherort verschoben werden konnte.

Der folgende Code zeigt, wie die Sicherung mithilfe der **BackupCallbackAsync**-Methode in Azure Storage hochgeladen werden kann:

```C#
private async Task<bool> BackupCallbackAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
{
    var backupId = Guid.NewGuid();

    await externalBackupStore.UploadBackupFolderAsync(backupInfo.Directory, backupId, cancellationToken);

    return true;
}
```

Im Beispiel oben ist **ExternalBackupStore** die Beispielklasse, die als Schnittstelle zum Azure-Blobspeicher verwendet wird, und **UploadBackupFolderAsync** ist die Methode, die den Ordner komprimiert und im Azure-Blobspeicher platziert.

Beachten Sie Folgendes:

* Pro Replikat kann jeweils nur ein Sicherungsvorgang ausgeführt werden. Mehrere **BackupAsync**-Aufrufe gleichzeitig lösen **FabricBackupInProgressException** aus, um nur eine Inflight-Sicherung zuzulassen.
* Wenn ein Replikat während einer Sicherung ausfällt, wird die Sicherung möglicherweise nicht abgeschlossen. Folglich muss der Dienst die Sicherung nach Abschluss des Failovers neu starten, indem bei Bedarf **BackupAsync** aufgerufen wird.

## Wiederherstellen von Reliable Services
Im Allgemeinen können die Fälle, in denen Sie möglicherweise eine Wiederherstellung durchführen müssen, einer der folgenden Kategorien zugeordnet werden:

* Die Dienstpartition hat Daten verloren. Beispielsweise wird die Festplatte für zwei von drei Replikaten einer Partition (einschließlich des primären Replikats) beschädigt oder gelöscht. Das neue primäre Replikat muss möglicherweise Daten aus einer Sicherung wiederherstellen.
* Der gesamte Dienst geht verloren. Beispielsweise entfernt ein Administrator den gesamten Dienst, sodass Dienst und Daten wiederhergestellt werden müssen.
* Der Dienst hat beschädigte Anwendungsdaten repliziert (z. B. aufgrund eines Anwendungsfehlers). In diesem Fall muss der Dienst aktualisiert oder zurückgesetzt werden, um die Ursache der Beschädigung zu entfernen, und nicht beschädigte Daten müssen wiederhergestellt werden.

Da viele Ansätze möglich sind, nennen wir einige Beispiele zum Verwenden von **RestoreAsync** zur Wiederherstellung in den obigen Szenarios.

## Verlust von Partitionsdaten in Reliable Services
In diesem Fall würde die Runtime automatisch den Datenverlust erkennen und die **OnDataLossAsync**-API aufrufen.

Der Dienstautor muss Folgendes wiederherstellen:

* Überschreiben Sie die Methode **OnDataLossAsync** der virtuellen Basisklasse.
* Suchen Sie am externen Speicherort, der die Sicherungen des Diensts enthält, nach den neuesten Sicherungen.
* Laden Sie die neueste Sicherung herunter (und dekomprimieren Sie die Sicherung in den Sicherungsordner, wenn sie komprimiert wurde).
* Die **OnDataLossAsync**-Methode stellt ein **RestoreContext**-Element bereit. Rufen Sie die **RestoreAsync**-API für das bereitgestellte **RestoreContext**-Element auf.
* Geben Sie „true“ zurück, wenn die Wiederherstellung erfolgreich war.

Im Folgenden finden Sie ein Beispiel für das Implementieren der **OnDataLossAsync**-Methode:

```C#

protected override async Task<bool> OnDataLossAsync(RestoreContext restoreCtx, CancellationToken cancellationToken)
{
    var backupFolder = await this.externalBackupStore.DownloadLastBackupAsync(cancellationToken);

    var restoreDescription = new RestoreDescription(backupFolder);

    await restoreCtx.RestoreAsync(restoreDescription);

    return true;
}
```

**RestoreDescription** wird an den **RestoreContext.RestoreAsync**-Aufruf übergeben und enthält ein Mitglied mit dem Namen **BackupFolderPath**. Beim Wiederherstellen einer einzelnen vollständigen Sicherung muss **BackupFolderPath** auf den lokalen Pfad des Ordners festgelegt werden, der die vollständige Sicherung enthält. Beim Wiederherstellen einer vollständigen Sicherung und mehrerer inkrementeller Sicherungen muss **BackupFolderPath** auf den lokalen Pfad des Ordners festgelegt werden, der die vollständige Sicherung sowie alle inkrementellen Sicherungen enthält. Der **RestoreAsync**-Aufruf kann **FabricMissingFullBackupException** auslösen, wenn das angegebene **BackupFolderPath**-Element keine vollständige Sicherung enthält. Zudem kann **ArgumentException** ausgelöst werden, wenn **BackupFolderPath** eine unterbrochene Kette von inkrementellen Sicherungen enthält, beispielsweise die vollständige Sicherung, die erste inkrementelle und die dritte inkrementelle Sicherung, jedoch nicht die zweite inkrementelle Sicherung.

> [!NOTE]
> Für die RestorePolicy ist standardmäßig „sicher“ eingestellt. Dies bedeutet, dass bei der **RestoreAsync**-API der Fehler „ArgumentException“ auftritt, wenn sie erkennt, dass der Sicherungsordner einen Status enthält, der älter ist als der Status in diesem Replikat bzw. gleich alt ist. **RestorePolicy.Force** kann verwendet werden, um diese Sicherheitsprüfung zu überspringen. Dies wird als Teil von **RestoreDescription** angegeben.
> 
> 

## Gelöschter oder verlorener Dienst
Wenn ein Dienst entfernt wird, muss der Dienst erst neu erstellt werden, bevor die Daten wiederhergestellt werden können. Der Dienst muss unbedingt mit der gleichen Konfiguration erstellt werden, z. B. dem Partitionierungsschema, damit die Daten problemlos wiederhergestellt werden können. Sobald der Dienst wieder läuft, muss die API zum Wiederherstellen von Daten (**OnDataLossAsync** oben) auf jeder Partition dieses Dienstes aufgerufen werden. Eine Möglichkeit, dies zu erreichen, ist die Verwendung von **[FabricClient.TestManagementClient.StartPartitionDataLossAsync](https://msdn.microsoft.com/library/mt693569.aspx)** auf jeder Partition.

Ab diesem Zeitpunkt erfolgt die Implementierung wie im oben aufgeführten Szenario. Jede Partition muss die letzte relevante Sicherung aus dem externen Speicher wiederherstellen. Ein Nachteil ist, dass die Partitions-ID sich geändert haben kann, da die Runtime Partitions-IDs dynamisch erstellt. Daher muss der Dienst die entsprechenden Partitionsinformationen und den Dienstnamen speichern, um die aktuelle Sicherung zum Speichern für jede Partition zu finden.

> [!NOTE]
> Es wird davon abgeraten, **FabricClient.ServiceManager.InvokeDataLossAsync** auf jeder Partition zu verwenden, um den gesamten Dienst wiederherzustellen, da dadurch der Clusterstatus beschädigt werden kann.
> 
> 

## Replizieren beschädigter Anwendungsdaten
Wenn das neu bereitgestellte Anwendungsupgrade fehlerhaft ist, werden ggf. Daten beschädigt. Beispielsweise kann ein Anwendungsupgrade damit beginnen, jede Telefonnummer in einem Reliable Dictionary mit einer ungültigen Vorwahl zu versehen. In diesem Fall werden die ungültigen Telefonnummern repliziert, da Service Fabric die Art der gespeicherten Daten nicht kennt.

Treten solche Fehler auf, bei denen Daten beschädigt werden, frieren Sie den Dienst am besten auf Anwendungsebene ein und führen, falls möglich, ein Upgrade auf die Version des Anwendungscodes durch, in der dieser Fehler nicht auftritt. Auch nach dem Korrigieren des Dienstcodes sind die Daten möglicherweise immer noch beschädigt und müssen daher wiederhergestellt werden. In solchen Fällen reicht es möglicherweise nicht aus, die neueste Sicherung wiederherzustellen, da die neuesten Sicherungen möglicherweise ebenfalls beschädigt sind. Daher müssen Sie die letzte Sicherung vor Beschädigung der Daten finden.

Wenn Sie nicht sicher sind, welche Sicherungen fehlerhaft sind, können Sie einen neuen Service Fabric-Cluster bereitstellen und die Sicherungen der betroffenen Partitionen wie im oben beschriebenen Szenario „Gelöschter oder verlorener Dienst“ wiederherstellen. Beginnen Sie für jede Partition mit der Wiederherstellung der Sicherungen, beginnend mit der neuesten bis hin zur ältesten. Sobald Sie eine Sicherung ohne Beschädigung gefunden haben, verschieben/löschen Sie alle Sicherungen dieser Partition, die aktueller sind als diese Sicherung. Wiederholen Sie diesen Vorgang für jede Partition. Wird nun **OnDataLossAsync** auf der Partition im Produktionscluster aufgerufen, handelt es sich bei der letzten im externen Speicher gefundenen Sicherung um die vom oben beschriebenen Verfahren verwendete.

Mit den Schritten im Abschnitt „Gelöschter oder verlorener Dienst“ kann der Status des Diensts wieder auf den Status vor der Beschädigung der Datei gesetzt werden.

Beachten Sie Folgendes:

* Bei einer Wiederherstellung besteht die Möglichkeit, dass die wiederherzustellende Sicherung älter als der Status der Partition vor dem Verlust der Daten ist. Deshalb sollte die Wiederherstellung nur als letzter Ausweg verwendet werden, um so viele Daten wie möglich wiederherzustellen.
* Die Zeichenfolge, die den Sicherungsorderpfad und die Dateipfade im Sicherungsordner repräsentiert, kann je nach FabricDataRoot-Pfad und Namenslänge des Anwendungstyps länger als 255 Zeichen sein. Einige .NET-Methoden, z.B. **Directory.Move**, lösen daraufhin unter Umständen die Ausnahme **PathTooLongException** aus. Eine Problemumgehung besteht darin, Kernel32-APIs wie **CopyFile** direkt aufzurufen.

## Sichern und Wiederherstellen von Reliable Actors
Das Sichern und Wiederherstellen von Reliable Actors baut auf den Sicherungs- und Wiederherstellungsfunktionen von Reliable Services auf. Der Dienstbesitzer sollte einen benutzerdefinierten Actordienst erstellen, der von **ActorService** abgeleitet ist (d.h. einen Reliable Service von Service Fabric, der Actors hostet), und dann Sicherungen bzw. Wiederherstellungen wie bei Reliable Services durchführen, wie in den vorherigen Abschnitten beschrieben. Da Sicherungen jeweils pro Partition erstellt werden, werden Status für alle Actors einer bestimmten Partition gesichert (die Wiederherstellung erfolgt ähnlich und jeweils pro Partition).

* Wenn Sie einen benutzerdefinierten Actordienst erstellen, müssen Sie beim Registrieren des Actors auch den benutzerdefinierten Actordienst registrieren. Siehe die Beschreibung zu **ActorRuntime.RegistorActorAsync**.
* **KvsActorStateProvider** unterstützt derzeit nur vollständige Sicherungen. **KvsActorStateProvider** ignoriert zudem die Option **RestorePolicy.Safe**.

> [!NOTE]
> Der standardmäßige ActorStateProvider (d.h. **KvsActorStateProvider**) bereinigt die Sicherungsordner **nicht** eigenständig (im Arbeitsordner der Anwendung, abgerufen über ICodePackageActivationContext.WorkDirectory). Dadurch wird der Arbeitsordner möglicherweise übermäßig voll. Sie sollten den Sicherungsordner im Sicherungsrückruf ausdrücklich bereinigen, nachdem Sie die Sicherung in einen externen Speicher verschoben haben.
> 
> 

## Testen von Sicherung und Wiederherstellung
Es muss sichergestellt werden, dass wichtige Daten gesichert werden und wiederhergestellt werden können. Dies kann durch Aufrufen des Cmdlets **Invoke-ServiceFabricPartitionDataLoss** in PowerShell erfolgen, mit dem Datenverlust in einer bestimmten Partition ausgelöst werden kann, um zu überprüfen, ob die Funktionen zum Sichern und Wiederherstellen der Daten wie erwartet ausgeführt werden. Es ist auch möglich, den Datenverlust programmgesteuert auszulösen und die Wiederherstellung infolge dieses Ereignisses durchzuführen.

> [!NOTE]
> Ein Beispiel für die Implementierung der Sicherungs- und Wiederherstellungsfunktionalität finden Sie in der Beschreibung zur Web Reference-App auf GitHub. Ausführliche Informationen finden Sie in der Beschreibung zum Inventory.Service-Dienst.
> 
> 

## Weitere Informationen zum Sichern und Wiederherstellen
Im Folgenden erhalten Sie weitere ausführliche Informationen zum Sichern und Wiederherstellen.

### Sicherung
Mit dem Reliable State Manager können konsistente Sicherungen erstellt werden, ohne Lese- oder Schreibvorgänge zu blockieren. Zu diesem Zweck verwendet er einen Mechanismus für Prüfpunkt- und Protokollbeständigkeit. Der Reliable State Manager entlastet durch Prüfpunkte an bestimmten Punkten das Transaktionsprotokoll und verbessert Wiederherstellungszeiten. Wenn **BackupAsync** aufgerufen wird, weist der Reliable State Manager alle Reliable Objects an, ihre neuesten Prüfpunktdateien in einen lokalen Sicherungsordner zu kopieren. Der Reliable State Manager kopiert dann alle Protokolleinträge ab dem „Start“-Zeiger bis zum aktuellen Protokolleintrag in den Sicherungsordner. Da alle Protokolleinträge einschließlich des aktuellen Eintrags in der Sicherung enthalten sind und der Reliable State Manager die Write-Ahead-Protokollierung beibehält, garantiert der Reliable State Manager, dass alle bestätigten Transaktionen (**CommitAsync** wurde erfolgreich zurückgegeben) in der Sicherung enthalten sind.

Sämtliche nach dem Aufrufen von **BackupAsync** bestätigten Transaktionen können in der Sicherung enthalten oder nicht enthalten sein. Sobald der lokale Sicherungsordner der Plattform von der Plattform aufgefüllt wurde (z. B. Abschluss der lokalen Sicherung durch die Runtime), wird der Rückruf der Dienstsicherung aufgerufen. Durch diesen Rückruf wird der Sicherungsordner in einen externen Speicherort wie den Azure-Speicher verschoben.

### Wiederherstellen
Der Reliable State Manager ermöglicht das Wiederherstellen aus einer Sicherung mit der **RestoreAsync**-API. Die **RestoreAsync**-Methode für **RestoreContext** kann nur innerhalb der **OnDataLossAsync**-Methode aufgerufen werden. Die von **OnDataLossAsync** zurückgegebene Bool gibt an, ob der Dienst seinen Status aus einer externen Quelle wiederhergestellt hat. Wenn für **OnDataLossAsync** „true“ zurückgegeben wird, erstellt Service Fabric alle anderen Replikate aus diesem primären Replikat neu. Service Fabric stellt sicher, dass Replikate, die **OnDataLossAsync** empfangen, zunächst die primäre Rolle annehmen, aber keinen Lese- oder Schreibstatus erhalten. Dies bedeutet, dass **RunAsync** bei StatefulService-Implementierern erst aufgerufen wird, wenn **OnDataLossAsync** erfolgreich abgeschlossen wurde. **OnDataLossAsync** wird dann für das neue primäre Replikat aufgerufen. Bis ein Dienst diese API erfolgreich (durch Rückgabe von true oder false) abschließt und die relevanten Neukonfigurationen fertigstellt, wird weiterhin die API nacheinander aufgerufen.

**RestoreAsync** löscht zuerst jeden bestehenden Status in dem primären Replikat, für das es aufgerufen wurde. Danach erstellt der Reliable State Manager alle Reliable Objects, die im Sicherungsordner vorhanden sind. Als Nächstes werden die Reliable Objects angewiesen, ihre Prüfpunkte im Sicherungsordner wiederherzustellen. Schließlich stellt der Reliable State Manager seinen eigenen Status aus den Protokolldatensätzen im Sicherungsordner wieder her und führt die Wiederherstellung aus. Im Rahmen des Wiederherstellungsprozesses werden ab dem „Startpunkt“ beginnende Vorgänge, die Protokolldatensätze in den Sicherungsordner ausgeführt haben, in den Reliable Objects wiederholt. Dadurch wird sichergestellt, dass der wiederhergestellte Status konsistent ist.

## Nächste Schritte
* [Reliable Services – Schnellstart](service-fabric-reliable-services-quick-start.md)
* [Reliable Services – Benachrichtigungen](service-fabric-reliable-services-notifications.md)
* [Entwicklerreferenz für zuverlässige Auflistungen](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

<!---HONumber=AcomDC_0629_2016-->