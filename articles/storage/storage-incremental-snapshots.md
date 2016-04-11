<properties
	pageTitle="Verwenden von inkrementellen Momentaufnahmen für Sicherung und Wiederherstellung von virtuellen Azure-Computern | Microsoft Azure"
	description="Erstellen Sie eine benutzerdefinierte Lösung für die Sicherung und Wiederherstellung der Datenträger Ihrer virtuellen Azure-Computer mithilfe inkrementeller Momentaufnahmen."
	services="storage"
	documentationCenter="na"
	authors="ms-prkhad"
	manager=""
	editor="tysonn"/>

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/21/2016"
	ms.author="tamram;prkhad"/>


# Sichern von Azure-VM-Datenträgern mit inkrementellen Momentaufnahmen

## Übersicht 

Azure Storage bietet die Möglichkeit, Momentaufnahmen von Blobs zu erstellen. Momentaufnahmen erfassen den Blobzustand zum jeweiligen Zeitpunkt. In diesem Artikel wird ein Szenario beschrieben, in dem Sie Sicherungen von Datenträgern des virtuellen Computers mithilfe von Momentaufnahmen verwalten können. Wenn Sie Azure Backup und den Wiederherstellungsdienst nicht verwenden, und eine benutzerdefinierte Sicherungsstrategie für die Datenträger Ihres virtuellen Computers erstellen möchten, können Sie diese Methodik anwenden.

Datenträger virtueller Azure-Computer werden als Seitenblobs in Azure Storage gespeichert. Da es in diesem Artikel um eine Sicherungsstrategie für die Datenträger des virtuellen Computers geht, beziehen wir uns im Kontext von Seitenblobs auf Momentaufnahmen. Weitere Informationen zu Momentaufnahmen finden Sie unter [Erstellen einer Momentaufnahme eines BLOB](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## Was ist eine Momentaufnahme?

Eine Momentaufnahme eines Blobs ist eine schreibgeschützte Version eines Blobs, die zu einem bestimmten Zeitpunkt erfasst wird. Nachdem eine Momentaufnahme erstellt wurde, kann sie zwar gelesen, kopiert oder gelöscht, aber nicht geändert werden. Momentaufnahmen bieten eine Möglichkeit, um ein Blob so zu sichern, wie es zu einem bestimmten Zeitpunkt dargestellt wird. Bis zur REST-Version XX konnten Sie vollständige Momentaufnahmen kopieren. Mit REST-Version YY und höher können Sie inkrementelle Momentaufnahmen kopieren.

## Vollständige Momentaufnahmenkopie

Momentaufnahmen können in ein anderes Speicherkonto kopiert werden als das, in dem sich ein Blob befindet, um Sicherungen des Basisblobs zu speichern. Sie können auch eine Momentaufnahme über das zugehörige Basisblob kopieren, was der Wiederherstellung einer früheren Version des Blobs entspricht. Wenn eine Momentaufnahme von einem Speicherkonto in ein anderes kopiert wird, nimmt sie den gleichen Speicherplatz wie das Basisseitenblob im Zielspeicherkonto ein. Daher verläuft das Kopieren gesamter Momentaufnahmen von einem Speicherkonto in ein anderes langsam und verbraucht auch viel Platz im Zielspeicherkonto.

>[AZURE.NOTE] Wenn Sie das Basisblob in ein anderes Ziel kopieren, werden die Momentaufnahmen des Blobs nicht mitkopiert. Wenn Sie ein Basisblob mit einer Kopie überschreiben, sind dem Basisblob zugeordnete Momentaufnahmen ebenfalls nicht betroffen und bleiben unter dem Basisblobnamen erhalten.

### Sichern von Datenträgern mit Momentaufnahmen

Eine Sicherungsstrategie für die Datenträger Ihrer VM könnte sein, dass Sie regelmäßige Momentaufnahmen des Datenträgers oder Seitenblobs aufnehmen und sie mit Tools wie dem [Copy Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx)-Vorgang oder [AzCopy](storage-use-azcopy.md) in ein anderes Speicherkonto kopieren. Sie können eine Momentaufnahme in ein Zielseitenblob mit einem anderen Namen kopieren. Das resultierende Zielseitenblob ist ein beschreibbares Blob und keine Momentaufnahme. Später in diesem Artikel werden Schritte beschrieben, mit denen Sie Sicherungen von Datenträgern des virtuellen Computers mithilfe von Momentaufnahmen erstellen können.

### Wiederherstellen von Datenträgern mit Momentaufnahmen

Wenn Sie eine frühere, in einer der Sicherungsmomentaufnahmen erfasste stabile Version Ihres Datenträgers wiederherstellen möchten, können Sie eine Momentaufnahme über das Basisseitenblob kopieren. Nach dem Höherstufen als Basisseitenblob bleibt die Momentaufnahme erhalten, ihre Quelle wird jedoch mit einer Kopie überschrieben, die gelesen und beschrieben werden kann. Später in diesem Artikel werden Schritte zum Wiederherstellen einer früheren Version des Datenträgers aufgrund seiner Momentaufnahme beschrieben.

### Implementieren der vollständigen Momentaufnahmenkopie

Sie können mit den folgenden Schritten eine vollständige Momentaufnahmenkopie implementieren.

-   Zunächst erstellen Sie eine Momentaufnahme des Basisblobs mit dem [Snapshot Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx)-Vorgang.

-   Dann kopieren Sie die Momentaufnahme mit [Copy Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) in ein Zielspeicherkonto.

&lt; Codeausschnitte, die das Erstellen einer Momentaufnahme und Kopieren des Blobs zeigen &gt;

## Inkrementelle Momentaufnahmenkopie

Die New Diff Page Ranges-API bietet eine wesentlich bessere Möglichkeit, die Momentaufnahmen Ihrer Seitenblobs oder Datenträger zu sichern. Die API gibt die Liste der Änderungen zurück, in denen sich Basisblob und Momentaufnahmen unterscheiden. Dies reduziert die Menge des auf dem Sicherungskonto verwendeten Speicherplatzes. Die API unterstützt Seitenblobs sowohl in Storage Premium als auch Standard Storage. Mit dieser API können Sie nun schnellere und effiziente Sicherungslösungen für Azure-VMs erstellen. Dies wird ab REST-Version xx verfügbar sein.

Mit der inkrementellen Kopie der Momentaufnahme können Sie von einem Speicherkonto in ein anderes den Unterschied kopieren zwischen

-   Basisblob und seiner Momentaufnahme ODER

-   Zwei beliebigen Momentaufnahmen des Basisblobs

**Hinweis**: Diese Funktion steht für Premium- und Standard-Azure-Seitenblobs zur Verfügung.

Wenn Sie über eine benutzerdefinierte Sicherungsstrategie verfügen, die Momentaufnahmen verwendet, wobei die Momentaufnahmen von einem Speicherkonto in ein anderes kopiert werden, kann diese Methode sehr langsam sein und viel Speicherplatz verbrauchen. Anstatt die gesamte Momentaufnahme in ein Sicherungsspeicherkonto zu kopieren, können Sie den Unterschied in ein Sicherungsseitenblob schreiben. Dies reduziert die Zeit zum Kopieren und den Speicherplatz zum Speichern von Sicherungen erheblich.

### Implementieren der inkrementellen Momentaufnahmenkopie

Sie können mit den folgenden Schritten eine inkrementelle Momentaufnahmenkopie implementieren.

-   Erstellen Sie eine Momentaufnahme des Basisblobs mit [Snapshot Blob](https://msdn.microsoft.com/library/azure/ee691971.aspx).

-   Kopieren Sie das Basisblob mit [Copy Blob](https://msdn.microsoft.com/library/azure/dd894037.aspx) in ein Zielspeicherkonto.

-   Rufen Sie den Unterschied zwischen Basisblob und Momentaufnahme mit [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) ab. Verwenden Sie den neuen Parameter **prevsnapshot**, um den DateTime-Wert der Momentaufnahme anzugeben, zu der Sie den Unterschied abrufen möchten. Wenn dieser Parameter vorhanden ist, enthält die REST-Antwort nur die Seiten, die zwischen Zielblob und vorheriger Momentaufnahme geändert wurden, einschließlich gelöschter Seiten.

-   Übernehmen Sie diese Änderungen schließlich mit [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) in das Sicherungsseitenblob.

&lt; Codeausschnitte, die die Get- und Put-Seitenbereiche zeigen &gt;

Im nächsten Abschnitt wird ausführlich beschrieben, wie Sie Sicherungen von Datenträgern mit inkrementellen Momentaufnahmenkopien verwalten können.

## Szenario

In diesem Abschnitt wird ein Szenario beschrieben, das eine benutzerdefinierte Sicherungsstrategie für Datenträger des virtuellen Computers mithilfe von Momentaufnahmen umfasst.

Stellen Sie sich eine Azure-VM der DS-Serie mit angefügtem Storage Premium-P30-Datenträger vor. Der P30-Datenträger „mypremiumdisk“ wird in einem Storage Premium-Konto namens „mypremiumaccount“ gespeichert. Ein Standard Storage-Konto namens „mybackupstdaccount“ wird zum Speichern der Sicherung von „mypremiumdisk“ verwendet. Wir würden gerne alle 12 Stunden eine Momentaufnahme von „mypremiumdisk“ speichern.

Informationen zum Erstellen von Speicherkonto und Datenträgern finden Sie unter [Informationen zu Azure-Speicherkonten](storage-create-storage-account.md).

Informationen zum Sichern von Azure-VMs finden Sie unter [Planen der Sicherungsinfrastruktur für virtuelle Computer in Azure](../backup/backup-azure-vms-introduction.md).

## Schritte zum Verwalten von Sicherungen eines Datenträgers mit inkrementellen Momentaufnahmen

Mit den unten beschriebenen Schritten werden Momentaufnahmen von „mypremiumdisk“ erstellt und die Sicherungen in „mybackupstdaccount“ verwaltet. Die Sicherung wird ein Standardseitenblob mit dem Namen „mybackupstdpageblob“ sein, das zu jedem Zeitpunkt den Zustand der letzten Momentaufnahme von „mypremiumdisk“ widerspiegelt.

1.  Erstellen Sie eine Kopie des Storage Premium-Datenträgers im Sicherungs-Standard Storage-Konto. Kopieren Sie „mypremiumdisk“ als neues Seitenblob „mybackupstdpageblob“ von „mypremiumaccount“ zu „mybackupstdaccount“.

2.  Erstellen Sie mit „Snapshot Blob“ von „mybackupstdpageblob“ eine Momentaufnahme mit Namen „mybackupstdpageblob\_ss1“, und speichern Sie sie in „mybackupstdaccount“.

3.  Erstellen Sie während des Sicherungsfensters eine Momentaufnahme von „mypremiumdisk“, z. B. „mypremiumdisk\_ss1“, und speichern Sie sie in „mypremiumaccount“.

4.  Rufen Sie die inkrementellen Änderungen, durch die sich „mypremiumdisk“ von seiner ersten Momentaufnahme „mypremiumdisk\_ss1“ unterscheidet, mit „GetPageRanges“ ab. Schreiben Sie diese inkrementellen Änderungen in „mybackupstdaccount“ in das Sicherungsseitenblob „mybackupstdpageblob“. Wenn die inkrementellen Änderungen gelöschte Bereiche enthalten, müssen sie aus dem Sicherungsseitenblob gelöscht werden. Verwenden Sie „PutPage“, um inkrementelle Änderungen in das Sicherungsseitenblob zu schreiben.

5.  Erstellen Sie vom Sicherungsseitenblob „mybackupstdpageblob“ eine Momentaufnahme mit Namen „mybackupstdpageblob\_ss2“, und speichern Sie sie in „mybackupstdaccount“. Löschen Sie die Momentaufnahme aus dem Storage Premium-Konto „mypremiumdisk\_ss1“.

6.  Wiederholen Sie die Schritte 3 bis 5 in jedem Sicherungsfenster. Auf diese Weise können Sie Sicherungen von „mypremiumdisk“ in einem Standard Storage-Konto verwalten.

![Datenträgersicherung mithilfe inkrementeller Momentaufnahmen](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

&lt; Codeausschnitt, der „Copy Blob“, „GetPageRanges“ und „PutPage“ zeigt &gt;

## Schritte zum Wiederherstellen eines Datenträgers aus Momentaufnahmen

Mit den unten beschriebenen Schritten wird der Premium-Datenträger „mypremiumdisk“ mit einer früheren Momentaufnahme aus dem Sicherungsspeicherkonto „mybackupstdaccount“ wiederhergestellt.

1.  Geben Sie den Zeitpunkt an, den Sie auf dem Premium-Datenträger wiederherstellen möchten. Nehmen wir an, die Momentaufnahme sei „mybackupstdpageblob\_ss1“, die in dem Sicherungsspeicherkonto „mybackupstdaccount“ gespeichert ist.

2.  Stufen Sie die Momentaufnahme „mybackupstdpageblob\_ss1“ in „mybackupstdaccount“ als neues Sicherungs-Basisseitenblob „mybackupstdpageblobrestored“ herauf.

3.  Kopieren Sie das Sicherungs-Basisseitenblob „mybackupstdpageblobrestored“ als neuen Premium-Datenträger „mypremiumdiskrestored“ aus „mybackupstdaccount“ in „mypremiumaccount“.

4.  Verweisen Sie die DS-Serien-VM auf den wiederhergestellten Datenträger „mypremiumdiskrestored“, und trennen Sie den alten Datenträger „mypremiumdisk“ von der VM.

5.  Beginnen Sie den im vorherigen Abschnitt beschriebenen Sicherungsvorgang für den wiederhergestellten Datenträger „mypremiumdiskrestored“, wobei Sie „mybackupstdpageblobrestored“ als Sicherungsseitenblob verwenden.

![Wiederherstellen von Datenträgern aus Momentaufnahmen](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

&lt; Codeausschnitt, der „Copy Blob“, Anfügen und Trennen des Datenträgers zeigt &gt;

## Weitere Informationen

- [Erstellen einer Momentaufnahme eines Blobs](https://msdn.microsoft.com/library/azure/hh488361.aspx)
- [Planen der Sicherungsinfrastruktur für virtuelle Computer](../backup/backup-azure-vms-introduction.md)

<!---HONumber=AcomDC_0330_2016-->