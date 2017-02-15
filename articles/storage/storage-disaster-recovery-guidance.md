---
title: Vorgehensweise bei einem Ausfall von Azure Storage | Microsoft Docs
description: Vorgehensweise bei einem Ausfall von Azure Storage
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 14997080496adfd363fee249c858ed9a0b553066


---
# <a name="what-to-do-if-an-azure-storage-outage-occurs"></a>Vorgehensweise beim Ausfall von Azure Storage
Bei Microsoft arbeiten wir hart, um sicherzustellen, dass unsere Dienste immer verfügbar sind. Aufgrund höherer Gewalt können ungeplante Dienstausfälle in einer Region oder auch mehreren Regionen auftreten. Damit Sie mit diesen seltenen Vorkommen umgehen können, stellen wir Ihnen den folgenden allgemeinen Leitfaden für Azure Storage-Dienste zur Verfügung.

## <a name="how-to-prepare"></a>So bereiten Sie sich vor
Es ist wichtig für jeden Kunden, den eigenen Notfallwiederherstellungsplan zu erstellen. Die Wiederherstellung nach einem Speicherausfall erfordert üblicherweise sowohl Personal für den Betrieb als auch automatisierte Vorgehensweise, um wieder funktionsfähige Anwendungen zu erhalten. Weitere Informationen zum Erstellen eines Notfallwiederherstellungsplans finden Sie in der Azure-Dokumentation weiter unten:

* [Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Technischer Leitfaden zur Resilienz in Azure](../resiliency/resiliency-technical-guidance.md)
* [Azure Site Recovery-Dienste](https://azure.microsoft.com/services/site-recovery/)
* [Azure Storage-Replikation](storage-redundancy.md)
* [Azure Backup-Dienst](https://azure.microsoft.com/services/backup/)

## <a name="how-to-detect"></a>Vorgehensweise zum Erkennen eines Ausfalls von Azure Storage
Die empfohlene Methode zum Bestimmen des Azure-Dienststatus ist die Anmeldung beim [Azure Service Health Dashboard](https://azure.microsoft.com/status/)(Azure Service-Dashboard zum Dienststatus).

## <a name="what-to-do-if-a-storage-outage-occurs"></a>Vorgehensweise beim Ausfall von Storage
Wenn eine oder mehrere Storage-Dienste in einer Region oder in mehreren Regionen vorübergehend nicht verfügbar sind, stehen Ihnen zwei Optionen zur Auswahl. Wenn Sie sofortigen Zugriff auf Ihre Daten wünschen, sollten Sie Option 2 wählen.

### <a name="option-1-wait-for-recovery"></a>Option 1: Warten auf die Wiederherstellung
In diesem Fall ist keine weitere Aktion erforderlich. Wir arbeiten intensiv daran, die Verfügbarkeit des Azure-Diensts wiederherzustellen. Sie können den Dienststatus auf unserem [Azure Service Health Dashboard](https://azure.microsoft.com/status/)überwachen.

### <a name="option-2-copy-data-from-secondary"></a>Option 2: Kopieren von Daten von der sekundären Region
Wenn Sie [Read-Access Geo-Redundant-Speicher (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (empfohlen) für Ihre Speicherkonten ausgewählt haben, verfügen Sie über Lesezugriff auf Ihre Daten in der sekundären Region. Sie können Tools wie [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md) und [Azure Data Movement Library](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) verwenden, um Daten aus der sekundären Region in ein anderes Speicherkonto oder in eine nicht betroffene Region zu kopieren. Anschließend können Sie Ihre Anwendungen jeweils für Lese- und Schreibverfügbarkeit auf dieses Speicherkonto verweisen.

## <a name="what-to-expect-if-a-storage-failover-occurs"></a>Was bei einem Storage-Failover zu erwarten ist
Wenn Sie [Georedundanter Speicher (GRS)](storage-redundancy.md#geo-redundant-storage) oder [Read-Access Geo-Redundant-Speicher (RA-GRS)](storage-redundancy.md#read-access-geo-redundant-storage) (empfohlen) ausgewählt haben, wird Azure Storage Ihre Daten dauerhaft in zwei Regionen (primär und sekundär) beibehalten. In beiden Regionen unterhält Azure Storage ständig mehrere Replikate Ihrer Daten.

Wenn ein regionaler Notfall die primäre Region betrifft, versuchen wir zunächst, den Dienst in dieser Region wiederherzustellen. Abhängig von der Art des Notfalls und seinen Auswirkungen, ist es uns in seltenen Fällen nicht möglich, die primäre Region wiederherzustellen. Zu diesem Zeitpunkt werden wir ein Geofailover durchführen. Die regionsübergreifende Datenreplikation ist ein asynchroner Prozess, der eine Verzögerung mit sich bringen kann. Es ist daher möglich, dass Änderungen, die noch nicht in die sekundäre Region repliziert wurden, möglicherweise verlorengehen. Sie können die [letzte Synchronisierungszeit Ihres Speicherkontos](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/) abfragen, um Informationen zu Ihrem Replikationsstatus zu erhalten.

Ein paar Punkte in Bezug auf das Geofailoverereignis des Speichers:

* Das Geofailover des Speichers wird nur durch das Azure Storage-Team ausgelöst, es ist keine Kundenaktion erforderlich.
* Ihre vorhandenen Speicherdienstendpunkte für Blobs, Tabellen, Warteschlangen und Dateien bleiben nach dem Failover unverändert. Der DNS-Eintrag muss aktualisiert werden, um aus der primären Region in die sekundäre zu wechseln.
* Vor und während des Geofailovers besitzen Sie keinen Schreibzugriff auf das Speicherkonto aufgrund der Auswirkung des Notfalls. Sie können jedoch weiterhin von der sekundären Region auslesen, wenn das Speicherkonto als RA-GRS konfiguriert wurde.
* Wenn das Geofailover abgeschlossen wurde und die DNS-Änderungen übermittelt wurden, werden der Lese- und Schreibzugriff auf das Speicherkonto fortgesetzt. Sie können mithilfe der Abfrage [„LastGeoFailoverTime“](https://msdn.microsoft.com/library/azure/ee460802.aspx) weitere Details darüber erhalten, wann für Ihr Speicherkonto zuletzt ein Failover zur sekundären Region durchgeführt wurde.
* Nach dem Failover ist Ihr Speicherkonto voll funktionsfähig, hat aber den Status „heruntergestuft“, da es tatsächlich in einer eigenständigen Region ohne mögliche Georeplikation gehostet wird. Um dieses Risiko zu verringern, werden wir die ursprüngliche primäre Region wiederherstellen und anschließend ein Geofailback ausführen, um den ursprünglichen Status wiederherzustellen. Wenn die ursprüngliche primäre Region nicht verfügbar ist, wird eine andere sekundäre Region zugeordnet.
  Weitere Informationen über die Infrastruktur der Georeplikation von Azure Storage finden Sie im Artikel auf dem Storage-Teamblog zu [Windows Azure Storage Redundancy Options and Read Access Geo Redundant Storage](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)(Windows Azure Storage Redundanzoptionen und Georedundanter Speicher mit Lesezugriff (RA-GRS).

## <a name="best-practices-for-protecting-your-data"></a>Bewährte Methoden zum Schützen der Daten
Es gibt einige empfohlenen Vorgehensweisen, um die Speicherdaten in regelmäßigen Abständen zu sichern.

* VM-Datenträger – Verwenden Sie den [Azure Backup-Dienst](https://azure.microsoft.com/services/backup/) , um die von Ihrem virtuellen Computer verwendeten VM-Datenträger zu sichern.
* Blockblobs – Erstellen Sie eine [Momentaufnahme](https://msdn.microsoft.com/library/azure/hh488361.aspx) von jedem Blockblob, oder kopieren Sie die Blobs in ein anderes Speicherkonto in einer andere Region mithilfe von [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md) oder [Azure Data Movement Library](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/).
* Tabellen – Verwenden Sie [AzCopy](storage-use-azcopy.md) , um die Tabellendaten in ein anderes Speicherkonto in einer anderen Region zu exportieren.
* Dateien – Verwenden Sie [AzCopy](storage-use-azcopy.md) oder [Azure PowerShell](storage-powershell-guide-full.md), um die Dateien in ein anderes Speicherkonto in einer anderen Region zu kopieren.




<!--HONumber=Nov16_HO3-->


