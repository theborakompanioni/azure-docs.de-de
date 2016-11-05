---
title: Verschieben von Daten in und aus Azure Storage | Microsoft Docs
description: Dieser Artikel bietet eine Übersicht über die verschiedenen Methoden zum Verschieben von Daten in und aus Azure Storage.
services: storage
documentationcenter: ''
author: micurd
manager: jahogg
editor: tysonn

ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2016
ms.author: micurd

---
# <a name="moving-data-to-and-from-azure-storage"></a>Verschieben von Daten in und aus Azure Storage
Wenn Sie lokale Daten in Azure Storage verschieben möchten (oder umgekehrt), steht Ihnen eine Vielzahl von Methoden zur Verfügung. Es hängt vom jeweiligen Szenario ab, welche Methode für Sie am besten geeignet ist. Dieser Artikel bietet eine kurze Übersicht über die verschiedenen Szenarien und die jeweiligen Angebote.

## <a name="building-applications"></a>Erstellen von Anwendungen
Wenn Sie eine Anwendung erstellen, ist das Entwickeln mit der REST-API oder einer unserer vielen Clientbibliotheken eine hervorragende Möglichkeit zum Verschieben von Daten in und aus Azure Storage.

Azure Storage bietet umfangreiche Clientbibliotheken für .NET, iOS, Java, Android, die universelle Windows-Plattform (UWP), Xamarin, C++, Node.JS, PHP, Ruby und Python. Die Clientbibliotheken bieten erweiterte Funktionen, beispielsweise Wiederholungslogik, Protokollierung und parallele Uploads. Die Entwicklung kann direkt mit der REST-API erfolgen. Diese API lässt sich mithilfe jeder Sprache aufrufen, die HTTP/HTTPS-Anforderungen verarbeitet.

Weitere Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md) .

Darüber hinaus bieten wir auch die [Azure Storage Data Movement Library](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) an, eine Bibliothek, die für das Kopieren von Daten zu und aus Azure mit hoher Leistung konzipiert wurde. Die [Dokumentation](https://github.com/Azure/azure-storage-net-data-movement) zu unserer Data Movement Library bietet weitere Informationen. 

## <a name="quickly-viewing/interacting-with-your-data"></a>Schnelles Anzeigen/Interagieren mit den Daten
Wenn Sie Ihre Azure Storage-Daten auf einfache Weise anzeigen und gleichzeitig die Möglichkeit haben möchten, Ihre Daten hoch- und herunterzuladen, sollten Sie einen Azure-Speicher-Explorer verwenden.

In der Liste der [Azure-Speicher-Explorer](storage-explorers.md) finden Sie weitere Informationen.

## <a name="system-administration"></a>Systemadministration
Wenn Sie ein Befehlszeilenprogramm benötigen oder mit einem solchen Programm besser vertraut sind (z. B. Systemadministratoren), stehen Ihnen u. a. die folgenden Optionen zur Auswahl:

### <a name="azcopy"></a>AzCopy
AzCopy ist ein Windows-Befehlszeilenprogramm, mit dem sehr effizient Daten in und aus Azure Storage kopiert werden können. Sie können auch Daten innerhalb eines Speicherkontos oder zwischen Speicherkonten kopieren.

Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md) .

### <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell ist ein Modul, das Cmdlets für die Verwaltung von Diensten in Azure bereitstellt. Bei diesem Modul handelt es sich um eine aufgabenbasierte Befehlszeilenshell und Skriptsprache, die speziell für die Systemverwaltung entwickelt wurde.

Weitere Informationen finden Sie unter [Verwenden von Azure PowerShell mit Azure Storage](storage-powershell-guide-full.md) .

### <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle
Die Azure-Befehlszeilenschnittstelle stellt eine Reihe von plattformübergreifenden Open Source-Befehlen für die Arbeit mit Azure-Diensten bereit. Die Azure-Befehlszeilenschnittstelle ist unter Windows, OS X und Linux verfügbar.

Weitere Informationen finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle mit Azure-Speicher](storage-azure-cli.md) .

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Verschieben großer Datenmengen in einem langsamen Netzwerk
Eines der größten Probleme beim Verschieben großer Datenmengen ist die Zeit für die Übertragung. Wenn Sie Daten in bzw. aus Azure Storage übertragen möchten, ohne die Kosten für Netzwerke bedenken oder Code schreiben zu müssen, ist Azure Import/Export eine geeignete Lösung.

Weitere Informationen finden Sie unter [Azure Import/Export](storage-import-export-service.md) .

## <a name="backing-up-your-data"></a>Sichern der Daten
Wenn Sie einfach Ihre Daten in Azure Storage sichern müssen, ist Azure Backup die beste Wahl. Dies ist eine leistungsfähige Lösung zum Sichern von lokalen Daten und Azure-VMs.

Weitere Informationen finden Sie unter [Azure Backup](../backup/backup-introduction-to-azure-backup.md) .

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Zugreifen auf Ihre Daten lokal und über die Cloud
Wenn Sie eine Lösung für den Zugriff auf Ihre Daten lokal und über die Cloud benötigen, dann sollten Sie StorSimple, die hybride Cloudspeicherlösung von Azure, in Betracht ziehen. Diese Lösung besteht aus einem physischen StorSimple-Gerät, auf dem auf intelligente Weise häufig verwendete Daten auf SSDs, gelegentlich verwendete Daten auf HDDs und inaktive/Backup-/Archivdaten in Azure Storage gespeichert werden.

Weitere Informationen finden Sie unter [StorSimple](../storsimple/storsimple-overview.md) .

## <a name="recovering-your-data"></a>Wiederherstellen der Daten
Wenn Sie über lokale Workloads und Anwendungen verfügen, benötigen Sie eine Lösung, die Geschäftskontinuität im Falle eines Notfalls ermöglicht. Azure Site Recovery steuert Replikation, Failover und Wiederherstellung virtueller Computer und physischer Server. Replizierte Daten werden in Azure Storage gespeichert, sodass Sie kein sekundäres lokales Rechenzentrum benötigen.

Weitere Informationen finden Sie unter [Azure Site Recovery](../site-recovery/site-recovery-overview.md) .

<!--HONumber=Oct16_HO2-->


