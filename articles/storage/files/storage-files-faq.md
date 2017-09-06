---
title: "Häufig gestellte Fragen zu Azure File Storage | Microsoft-Dokumentation"
description: "Hier erhalten Sie Antworten auf häufig gestellte Fragen zu Azure File Storage."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/19/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 17868591e1056ff2bddde0e082695af529f58f02
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="frequently-asked-questions-about-azure-file-storage"></a>Häufig gestellte Fragen zu Azure File Storage

## <a name="general"></a>Allgemein
* **F: Was ist Azure File Storage?**  
   
    Azure File Storage ist ein verteiltes Dateisystem in Azure. Es bietet eine SMB-Protokollschnittstelle, über die Benutzer den Speicher als native Freigabe auf unterstützten virtuellen Azure-Computern oder lokalen Computern einbinden können.

* **F: Warum ist Azure File Storage nützlich?**  
   
    Azure File Storage ermöglicht VM- und plattformübergreifenden Datenzugriff. Weitere Informationen finden Sie unter [Praktische Features von Azure File Storage](storage-files-introduction.md#why-azure-file-storage-is-useful).

* **F: Wann empfiehlt sich die Verwendung von Azure File Storage (anstelle von Azure Blob Storage oder Azure Data Disks)?**  
   
    Mit Microsoft Azure können Sie Daten auf unterschiedliche Weise in der Cloud speichern und darauf zugreifen.  
   
    Azure File Storage: Bietet eine SMB-Schnittstelle, Clientbibliotheken und eine REST-Schnittstelle für einfachen, ortsunabhängigen Zugriff auf gespeicherte Dateien.  
   
    Azure Blob Storage: Bietet Clientbibliotheken und eine REST-Schnittstelle, mit der umfangreiche unstrukturierte Daten in Blockblobs gespeichert und abgerufen werden können.  
   
    Azure Data Disks: Bietet Clientbibliotheken und eine REST-Schnittstelle, mit der Sie Daten dauerhaft auf einer angefügten virtuellen Festplatte speichern und davon abrufen können.  
   
    Weitere Informationen finden Sie unter [Entscheidung zwischen Azure-Blobs, Azure Files und Azure-Datenträger](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* **F: Wie kann ich Azure File Storage verwenden?**  
   
    Zunächst einmal erstellen Sie eine Azure-Dateifreigabe. Azure-Dateifreigaben können über das Azure-Portal, mithilfe von Azure Storage-PowerShell-Cmdlets, unter Verwendung der Azure Storage-Clientbibliotheken oder mithilfe der Azure Storage-REST-API erstellt werden. In diesem Tutorial wird Folgendes vermittelt:

    * [Erstellen einer Azure-Dateifreigabe über das Portal](storage-how-to-create-file-share.md#create-file-share-through-the-portal)
    * [Erstellen einer Azure-Dateifreigabe mithilfe von PowerShell](storage-how-to-create-file-share.md#create-file-share-through-powershell)
    * [Erstellen einer Azure-Dateifreigabe unter Verwendung der Befehlszeilenschnittstelle](storage-how-to-create-file-share.md#create-file-share-through-command-line-interface-cli)

* **F: Welche Replikationen werden von Azure File Storage unterstützt?**  
   
    Azure File Storage unterstützt momentan nur lokal redundanten Speicher (LRS) und georedundanten Speicher (GRS). Die Unterstützung von RA-GRS ist geplant, aber es liegt noch keine Zeitplanung vor.

## <a name="security-authentication-and-access-control"></a>Sicherheit, Authentifizierung und Zugriffssteuerung

* **F: Auf welche Arten kann auf Dateien in Azure File Storage zugegriffen werden?**
    
    Sie können die Dateifreigabe mithilfe des SMB 3.0-Protokolls auf Ihrem lokalen Computer einbinden oder mithilfe von Tools wie dem [Speicher-Explorer](http://storageexplorer.com/) auf Dateien in Ihrer Dateifreigabe zugreifen. In Ihrer Anwendung können Sie über Speicherclientbibliotheken, REST-APIs oder PowerShell auf Ihre Dateien in der Azure-Dateifreigabe zugreifen.

* **F: Wie kann ich mithilfe eines Webbrowsers Zugriff auf eine bestimmte Datei gewähren?**
    
    Mit SAS können Sie Token mit bestimmten Berechtigungen generieren, die für ein bestimmtes Zeitintervall gültig sind. Beispielsweise können Sie ein Token mit Lesezugriff auf eine bestimmte Datei für einen bestimmten Zeitraum generieren. Jeder, der über diese URL verfügt, kann bis zum Ablauf der Gültigkeit direkt über einen beliebigen Webbrowser auf die Datei zugreifen. SAS-Schlüssel können problemlos über eine Benutzeroberfläche wie Speicher-Explorer generiert werden.

* **F: Ist es möglich, Lese- oder Schreibberechtigungen für Ordner in der Freigabe anzugeben?**
    
    Sie verfügen nicht über dieses Maß an Kontrolle über Berechtigungen, wenn Sie die Dateifreigabe per SMB bereitstellen. Sie können dies aber erreichen, indem Sie über die REST-API oder Clientbibliotheken eine Shared Access Signature (SAS) erstellen.  

* **F: Wie kann ich die serverseitige Verschlüsselung für Azure File Storage aktivieren?**

    Die [serverseitige Verschlüsselung](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) für Azure File Storage ist in allen Regionen sowie in öffentlichen und nationalen Clouds verfügbar. Sie können SSE für den Azure File Storage über das [Azure-Portal](https://portal.azure.com/), mithilfe der [Microsoft Azure Storage-Ressourcenanbieter-API](/rest/api/storagerp/storageaccounts), mithilfe von [Azure PowerShell](https://msdn.microsoft.com/library/azure/mt607151.aspx) oder unter Verwendung der [Azure-Befehlszeilenschnittstelle](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) aktivieren.
    
    Nach der Aktivierung von SSE für Azure File Storage werden alle neuen Daten, die in den Dateispeicher unter diesem Speicherkonto geschrieben werden, automatisch verschlüsselt. Diese Funktion ist für alle neuen Daten verfügbar, die in vorhandene oder neue Freigaben in einem vorhandenen oder neuen Speicherkonto geschrieben werden. Für die Aktivierung dieses Features fallen keine zusätzlichen Gebühren an. Weitere Informationen zum Aktivieren von SSE für Azure File Storage finden Sie [hier](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* **F: Unterstützt Azure File Storage die Active Directory-basierte Authentifizierung?**
   
    Derzeit ist keine Unterstützung für die AD-basierte Authentifizierung oder Zugriffssteuerungslisten (ACLs) vorhanden, aber wir haben es in die Liste mit den angeforderten Features aufgenommen. Vorläufig werden die Azure Storage-Kontoschlüssel für die Authentifizierung gegenüber der Dateifreigabe genutzt. Als Alternative bieten wir die Verwendung von Shared Access Signatures (SAS) über die REST-API oder die Clientbibliotheken an. Mit SAS können Sie Token mit bestimmten Berechtigungen generieren, die für ein bestimmtes Zeitintervall gültig sind. So können Sie beispielsweise für eine bestimmte Datei ein Token mit Lesezugriff generieren, das nach zehn Minuten abläuft. Jeder Benutzer, der über dieses Token verfügt, hat für einen Zeitraum von zehn Minuten Lesezugriff auf die Datei.
   
    SAS wird nur über die REST-API oder Clientbibliotheken unterstützt. Wenn Sie die Dateifreigabe per SMB-Protokoll bereitstellen, können Sie den Zugriff auf deren Inhalte nicht per SAS delegieren. 
    
* **F: Welche Datenkonformitätsrichtlinien werden für Azure File Storage unterstützt?**

   Da Azure File Storage auf der gleichen Speicherarchitektur basiert wie andere Speicherdienste in Azure Storage, kommen auch die gleichen Datenkonformitätsrichtlinien zur Anwendung. Weitere Informationen zur Datenkonformität von Azure Storage finden Sie im herunterladbaren [Dokument zum Datenschutz in Microsoft Azure](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409).

## <a name="on-premises-access"></a>Lokaler Zugriff

* **F: Muss ich Azure ExpressRoute verwenden, wenn ich über einen lokalen virtuellen Computer eine Verbindung mit Azure File Storage herstellen möchte?**
   
    Nein. Auch wenn Sie nicht über ExpressRoute verfügen, können Sie auf die Dateifreigabe trotzdem lokal zugreifen, solange Port 445 (TCP ausgehend) für den Internetzugriff geöffnet ist. ExpressRoute kann aber zusammen mit Azure File Storage verwendet werden, falls gewünscht.

* **F: Wie kann ich eine Azure-Dateifreigabe auf meinem lokalen Computer einbinden?** 
    
    Sie können die Dateifreigabe über das SMB-Protokoll einbinden, solange Port 445 (TCP ausgehend) geöffnet ist und Ihr Client das SMB 3.0-Protokoll unterstützt (was beispielsweise unter Windows 10 oder Windows Server 2012 der Fall ist). Wenden Sie sich zwecks Aufhebung der Portblockierung an Ihren örtlichen Internetdienstanbieter. In der Zwischenzeit können Sie Ihre Dateien mit dem [Speicher-Explorer](../../vs-azure-tools-storage-explorer-files.md#view-a-file-shares-contents) anzeigen.


## <a name="billing-and-pricing"></a>Abrechnung und Preise
* **F: Zählt der Netzwerkdatenverkehr zwischen einem virtuellen Azure-Computer und einer Dateifreigabe als externe Bandbreite, die für das Abonnement berechnet wird?**
   
    Wenn sich die Dateifreigabe und der virtuelle Computer in der gleichen Azure-Region befinden, ist der Datenverkehr zwischen den Komponenten kostenlos. Befinden sie sich dagegen in unterschiedlichen Regionen, wird der Datenverkehr zwischen den Komponenten als externe Bandbreite berechnet.

## <a name="backup"></a>Sicherung

* **F: Wie sichere ich meine Azure-Dateifreigabe?**
    
    Sie können AzCopy, RoboCopy oder ein Sicherungstool eines Drittanbieters verwenden, das zum Sichern einer eingebundenen Dateifreigabe geeignet ist. Wir erwarten, dass es in naher Zukunft möglich sein wird, Momentaufnahmen von Dateifreigaben zu erstellen, sodass Sie Ihre Azure-Dateifreigabe dann mit diesem Feature sichern können.

## <a name="performance"></a>Leistung

* **F: Welche Skalierungsgrenzwerte gelten für Azure File Storage?**
    Informationen zu Skalierbarkeits- und Leistungszielen von Azure File Storage finden Sie unter [Skalierbarkeitsziele für Blobs, Warteschlangen, Tabellen und Dateien](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#scalability-targets-for-blobs-queues-tables-and-files).

* **F: Dateien wurden in Azure File Storage nur sehr langsam entzippt. Wie soll ich vorgehen?**
    
    Für die Übertragung zahlreicher Dateien an Azure File Storage empfehlen wir die Verwendung von AzCopy (Windows, Vorschauversion für Linux/Unix) oder Azure PowerShell, da diese Tools für die Netzwerkübertragung optimiert sind.

* **F: Welche Patches wurden zur Behebung des Leistungsproblems von Azure File Storage veröffentlicht?**
    
    Das Windows-Team hat kürzlich einen Patch veröffentlicht, der das Leistungsproblem behebt, das auftritt, wenn Benutzer unter Windows 8.1 oder Windows Server 2012 R2 auf Azure Files Storage zugreifen. Weitere Informationen finden Sie im KB-Artikel [Slow performance when you access Azure Files Storage from Windows 8.1 or Server 2012 R2](https://support.microsoft.com/kb/3114025) (Geringe Leistung beim Zugriff auf Azure Files Storage unter Windows 8.1 oder Server 2012 R2).

## <a name="features-and-interoperability-with-other-services"></a>Features und Interoperabilität mit anderen Diensten
* **F: Ist ein „Dateifreigabenzeuge“ für einen Failovercluster einer der Anwendungsfälle für Azure File Storage?**
   
    Dies wird derzeit nicht unterstützt.

* **F: Steht in der REST-API ein Umbenennungsvorgang zur Verfügung?**
   
    Derzeit leider nicht.

* **F: Können Freigaben geschachtelt werden? Kann eine Freigabe also einer anderen Freigabe untergeordnet werden?**
    
    Nein. Die Dateifreigabe ist der virtuelle Treiber, den Sie bereitstellen können. Geschachtelte Freigaben werden nicht unterstützt.

* **F: Kann ich Azure File Storage mit IBM MQ verwenden?**
    
    IBM hat ein Dokument mit Anleitungen für IBM MQ-Kunden veröffentlicht, die Azure File Storage mit ihrem Dienst konfigurieren möchten. Weitere Informationen finden Sie unter [How to setup IBM MQ Multi instance queue manager with Microsoft Azure File Service](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service)(Gewusst wie: Einrichten des IBM MQ-Warteschlangen-Managers für mehrere Instanzen mit dem Microsoft Azure-Dateidienst).


## <a name="troubleshooting"></a>Problembehandlung
* **F: Wie behandle ich Fehler von Azure File Storage?**
    
    Umfassende Unterstützung bei der Problembehandlung erhalten Sie im [Artikel zur Problembehandlung für Azure File Storage](storage-troubleshoot-windows-file-connection-problems.md). 


## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zum Azure-Dateispeicher erhalten Sie über diese Links.

### <a name="conceptual-articles-and-videos"></a>Konzeptionelle Artikel und Videos
* [Azure File Storage: ein reibungsloses Cloud-SMB-Dateisystem für Windows und Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Verwenden des Azure-Dateispeichers unter Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>Toolunterstützung für Dateispeicher
* [Verwenden von AzCopy mit Microsoft Azure Storage](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Verwenden der Azure-Befehlszeilenschnittstelle mit Azure-Speicher](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Beheben von Problemen mit Azure File Storage](storage-troubleshoot-linux-file-connection-problems.md)

### <a name="blog-posts"></a>Blogbeiträge
* [Azure-Dateispeicher ist jetzt allgemein verfügbar](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inside Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/) (Informationen zu Azure File Storage)
* [Einführung in den Microsoft Azure-Dateidienst](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrating Data to Microsoft Azure Files](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/) (Migrieren von Daten zu Microsoft Azure Files)

### <a name="reference"></a>Referenz
* [Referenz zur Storage-Clientbibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Referenz zur REST-API des Dateidiensts](http://msdn.microsoft.com/library/azure/dn167006.aspx)

