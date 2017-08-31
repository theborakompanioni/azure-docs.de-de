---
title: "Einführung in Azure File Storage | Microsoft-Dokumentation"
description: "Einführung in den Azure File Storage-Dienst, der Netzwerkdateifreigaben in Microsoft Cloud bereitstellt"
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
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 498af5cffb76e026c9a87127cab238f0f23b668a
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="introduction-to-azure-file-storage"></a>Einführung in Azure File Storage

Azure File Storage bietet Netzwerkdateifreigaben in der Cloud mit dem branchenüblichen [SMB-Protokoll (Server Message Block)](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) und [CIFS (Common Internet File System)](https://technet.microsoft.com/library/cc939973.aspx). Azure-Dateifreigaben können von Azure Virtual Machines und lokalen Bereitstellungen von Windows, macOS oder Linux parallel eingebunden werden. Über ein allgemeines Speicherkonto haben Sie Zugriff auf Azure File Storage, Azure Blob Storage, Azur Blob Storage und Azure Queue Storage.

## <a name="videos"></a>Videos
| Einführung in Azure File Storage (27 Min.) | Tutorial zu Azure File Storage (5 Min.)  |
|-|-|
| [![Standbild aus dem Einführungsvideo für Azure File Storage. Klicken Sie, um die Wiedergabe zu starten.](./media/storage-files-introduction/azure-files-introduction-video-snapshot1.png)](https://www.youtube.com/watch?v=zlrpomv5RLs) | [![Standbild aus dem Tutorialvideo für Azure File Storage. Klicken Sie, um die Wiedergabe zu starten.](./media/storage-files-introduction/azure-files-introduction-video-snapshot2.png)](https://channel9.msdn.com/Blogs/Azure/Azure-File-storage-with-Windows/) |

## <a name="why-azure-file-storage-is-useful"></a>Praktische Features von Azure File Storage

Mit Azure File Storage können Sie lokal oder in der Cloud gehostete Windows Server-, Linux- oder NAS-basierte Dateiserver durch eine betriebssystemfreie cloudbasierte Dateifreigabe ersetzen. Azure File Storage bietet die folgenden Vorteile:

* **Gemeinsamer Zugriff:** Azure-Dateifreigaben unterstützen das branchenübliche SMB-Standardprotokoll. Dadurch können Sie Ihre lokalen Dateifreigaben problemlos durch Azure-Dateifreigaben ersetzen, ohne sich Gedanken über die Anwendungskompatibilität machen zu müssen. Ein deutlicher Vorteil von Azure File Storage ist die Möglichkeit, auf eine Dateifreigabe von mehreren Computern und Anwendungen oder Instanzen aus zuzugreifen.

* **Vollständige Verwaltung:** Sie können Azure-Dateifreigaben erstellen, ohne sich um die Hardware oder um das Betriebssystem zu kümmern. Sie müssen also weder das Serverbetriebssystem mit kritischen Sicherheitsupgrades patchen noch fehlerhafte Festplatten austauschen.

* **Skripts und Tools:** PowerShell-Cmdlets und die Azure CLI können im Rahmen der Verwaltung von Azure-Anwendungen zum Erstellen, Bereitstellen und Verwalten von Azure-Dateifreigaben verwendet werden. Sie können Azure-Dateifreigaben mit dem [Azure-Portal](https://portal.azure.com) und dem [Azure-Speicher-Explorer](https://storageexplorer.com) erstellen und verwalten. 

* **Resilienz:** Azure File Storage ist von Grund auf als hochverfügbare Lösung konzipiert. Anders als bei lokalen Dateifreigaben müssen Sie sich bei Azure File Storage nicht mehr mit lokalen Stromausfällen oder Netzwerkproblemen auseinandersetzen. 

* **Vertraute Programmierbarkeit:** In Azure ausgeführte Anwendungen können über [Dateisystem-E/A-APIs](https://msdn.microsoft.com/library/system.io.file.aspx) auf Daten in der Freigabe zugreifen. Entwickler können daher ihren vorhandenen Code und bereits erlernte Fertigkeiten für die Migration vorhandener Anwendungen verwenden. Neben System-E/A-APIs können Sie auch eine der Azure Storage-Clientbibliotheken, etwa die für [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet), oder die [Azure Storage-REST-API](/rest/api/storageservices/file-service-rest-api) verwenden.

Verwendungsmöglichkeiten für Azure-Dateifreigaben:

* **Ersatz für lokale Dateiserver:** Azure File Storage kann Dateifreigaben auf herkömmlichen lokalen Dateiservern oder NAS-Geräten vollständig ersetzen. Gängige Betriebssysteme wie Windows, macOS und Linux auf der ganzen Welt können problemlos eine Azure-Dateifreigabe einbinden.

* **Lift & Shift-Anwendungen**

    Mit Azure File Storage lassen sich Anwendungen, die lokale Dateifreigaben verwenden, komfortabel per Lift & Shift in die Cloud migrieren, um Daten zwischen verschiedenen Anwendungsbereichen zu teilen. Hierzu stellt jeder virtuelle Computer eine Verbindung mit der Dateifreigabe her und kann dadurch Dateien genau wie bei einer lokalen Dateifreigabe lesen und schreiben.

* **Einfachere Cloudentwicklung**
    
    Azure File Storage kann die Entwicklung neuer Cloudprojekte auf vielfältige Weise vereinfachen.
    
    * **Gemeinsame Anwendungseinstellungen**
    
        Bei verteilten Anwendungen befinden sich die Konfigurationsdateien häufig an einem zentralen Ort, sodass viele verschiedene virtuelle Computer auf sie zugreifen können. Solche Konfigurationsdateien können nun in einer Azure-Dateifreigabe gespeichert und von allen Anwendungsinstanzen gelesen werden. Die Einstellungen können auch über die REST-Schnittstelle verwaltet werden, sodass von überall auf der Welt auf die Konfigurationsdateien zugegriffen werden kann.

    * **Diagnosefreigabe**
    
        Eine Azure-Dateifreigabe kann auch zum Speichern von Diagnosedateien wie Protokollen, Metriken und Absturzabbildern verwendet werden. Da Dateifreigaben sowohl über die SMB- als auch über die REST-Schnittstelle zur Verfügung stehen, können Anwendungen die Diagnosedaten mit verschiedenen Analysetools verarbeiten und analysieren.

    * **Entwickeln/Testen/Debuggen**

        Für die Arbeit an virtuellen Computern in der Cloud benötigen Entwickler und Administratoren häufig verschiedene Tools oder Hilfsprogramme. Diese Hilfsprogramme für jeden virtuellen Computer zu installieren und zu verteilen, für den sie benötigt werden, ist äußerst zeitaufwendig. Mit Azure File Storage können Entwickler oder Administratoren ihre Lieblingstools in einer Dateifreigabe speichern und über einen beliebigen virtuellen Computer darauf zugreifen.
        
## <a name="how-does-it-work"></a>Funktionsweise

Die Verwaltung von Azure-Dateifreigaben ist deutlich einfacher als die lokale Verwaltung von Dateifreigaben. Das folgende Diagramm veranschaulicht die Verwaltungskonstrukte von Azure File Storage:

![Dateistruktur](./media/storage-files-introduction/files-concepts.png)

* **Speicherkonto**: Alle Zugriffe auf Azure Storage erfolgen über ein Speicherkonto. Ausführliche Informationen zur Kapazität von Speicherkonten finden Sie unter [Azure Storage Scalability and Performance Targets](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (Skalierbarkeits- und Leistungsziele für Azure Storage).

* **Freigabe:** Eine File Storage-Freigabe ist eine SMB-Dateifreigabe in Azure. Alle Verzeichnisse und Dateien müssen in der übergeordneten Freigabe erstellt werden. Ein Konto kann eine unbegrenzte Anzahl von Freigaben enthalten, und eine Freigabe kann eine unbegrenzte Anzahl von Dateien speichern, bis die Kapazitätsgrenze von 5 TB für die Dateifreigabe erreicht ist.

* **Verzeichnis:** Eine optionale Hierarchie von Verzeichnissen

* **Datei:** Eine Datei in der Freigabe. Die Datei kann bis zu 1 TB groß sein.

* **URL-Format:** Dateien sind über das folgende URL-Format adressierbar:  

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory/directories>/<file>
    ```

## <a name="next-steps"></a>Nächste Schritte

* [Create a File Share in Azure File storage](storage-how-to-create-file-share.md) (Erstellen einer Dateifreigabe in Azure File Storage)
* [Mount an Azure File share and access the share in Windows](storage-how-to-use-files-windows.md) (Einbinden einer Azure-Dateifreigabe und Zugreifen auf die Freigabe unter Windows)
* [Verwenden des Azure-Dateispeichers unter Linux](storage-how-to-use-files-linux.md)
* [Mount Azure File share over SMB with macOS](storage-how-to-use-files-mac.md) (Einbinden einer Azure-Dateifreigabe über SMB unter macOS)
* [Häufig gestellte Fragen](../storage-files-faq.md)
* [Troubleshoot Azure File storage problems in Windows](storage-troubleshoot-windows-file-connection-problems.md) (Beheben von Problemen mit Azure File Storage unter Windows)   
* [Troubleshoot Azure File storage problems in Linux](storage-troubleshoot-linux-file-connection-problems.md) (Beheben von Problemen mit Azure File Storage unter Linux)   

<!-- Rena I would remove any articles from here that are more than a year old. - Robin-->
### <a name="conceptual-articles-and-videos"></a>Konzeptionelle Artikel und Videos
* [Azure File Storage: ein reibungsloses Cloud-SMB-Dateisystem für Windows und Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)

### <a name="tooling-support-for-azure-file-storage"></a>Toolunterstützung für Azure File Storage
* [Verwenden von AzCopy mit Microsoft Azure Storage](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Verwenden der Azure-Befehlszeilenschnittstelle mit Azure-Speicher](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)

### <a name="blog-posts"></a>Blogbeiträge
* [Azure-Dateispeicher ist jetzt allgemein verfügbar](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inside Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/) (Informationen zu Azure File Storage)
* [Einführung in den Microsoft Azure-Dateidienst](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrating Data to Microsoft Azure Files](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/) (Migrieren von Daten zu Microsoft Azure Files)

### <a name="reference"></a>Referenz
* [Referenz zur Storage-Clientbibliothek für .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Referenz zur REST-API des Dateidiensts](http://msdn.microsoft.com/library/azure/dn167006.aspx)

