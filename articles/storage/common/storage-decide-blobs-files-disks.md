---
title: "Entscheidung zwischen Azure-Blobs, Azure Files und Azure-Datenträger"
description: "In diesem Artikel erfahren Sie, wie Sie Daten auf unterschiedliche Weise in Azure speichern und darauf zugreifen können. So wird Ihnen die Entscheidung erleichtert, welche Technologie Sie verwenden sollten."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: robinsh
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 452030e2e55ebeae55be2bd858c59e45428c7621
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="deciding-when-to-use-azure-blobs-azure-files-or-azure-data-disks"></a>Entscheidung zwischen Azure-Blobs, Azure Files und Azure-Datenträger

Microsoft Azure bietet verschiedenen Funktionen in Azure Storage zum Speichern und Zugreifen auf Ihre Daten in der Cloud. Dieser Artikel befasst sich mit Azure Files, Blobs und Datenträger und soll Sie bei der Entscheidung zwischen diesen Funktionen unterstützen.

## <a name="scenarios"></a>Szenarios

In der folgenden Tabelle werden Files, Blobs und Data Disks miteinander verglichen. Darüber hinaus werden jeweils passende Beispielszenarios gegeben.

| Feature | Beschreibung | Einsatzgebiete |
|--------------|-------------|-------------|
| **Azure Files** | Bietet eine SMB-Schnittstelle, Clientbibliotheken und eine [REST-Schnittstelle](/rest/api/storageservices/file-service-rest-api), mit denen Sie von überall auf gespeicherte Dateien zugreifen können. | Wenn Sie eine Anwendung mit der „Lift and Shift“-Methode in die Cloud verschieben möchten, die bereits die nativen Dateisystem-APIs verwendet, um Daten für andere in Azure ausgeführte Anwendungen freizugeben.<br/><br/>Wenn Sie Tools zum Entwickeln und Debuggen speichern möchten, auf die von vielen virtuellen Computern zugegriffen werden muss. |
| **Azure-Blobs** | Bietet Clientbibliotheken und eine [REST-Schnittstelle](/rest/api/storageservices/blob-service-rest-api), mit der unstrukturierte Daten in großem Rahmen in Blockblobs gespeichert und abgerufen werden können. | Wenn Sie möchten, dass Ihre Anwendung Szenarios für das Streaming und den zufälligen Zugriff unterstützt.<br/><br/>Wenn Sie die Möglichkeit haben möchten, von überall auf Anwendungsdaten zugreifen zu können. |
| **Azure-Datenträger** | Bietet Clientbibliotheken und eine [REST-Schnittstelle](/rest/api/compute/virtualmachines/virtualmachines-create-or-update), mit der Sie Daten beständig von einer angefügten virtuellen Festplatte speichern und abrufen können. | Wenn Sie eine Anwendung mit der „Lift and Shift“-Methode verschieben möchten, die native Dateisystem-APIs verwenden, um Daten in beständigen Datenträgern zu lesen und dort hinein zu schreiben.<br/><br/>Wenn Sie Daten speichern möchten, auf die nicht von außerhalb des virtuellen Computers zugegriffen werden muss, an den der Datenträger angefügt ist. |

## <a name="comparison-files-and-blobs"></a>Vergleich: Files und Blobs

In der folgenden Tabelle wird Azure Files mit Azure-Blobs verglichen.  
  
||||  
|-|-|-|  
|**Attribut**|**Azure-Blobs**|**Azure Files**|  
|Optionen für die Dauerhaftigkeit|LRS, ZRS, GRS (und RA-GRS für hohe Verfügbarkeit)|LRS, GRS|  
|Barrierefreiheit|REST-APIs|REST-APIs<br /><br /> SMB 2.1 und SMB 3.0 (Standard-Dateisystem-APIs)|  
|Konnektivität|REST-APIs – Weltweit|REST-APIs – Weltweit<br /><br /> SMB 2.1 – Innerhalb einer Region<br /><br /> SMB 3.0 – Weltweit|  
|Endpunkte|`http://myaccount.blob.core.windows.net/mycontainer/myblob`|`\\myaccount.file.core.windows.net\myshare\myfile.txt`<br /><br /> `http://myaccount.file.core.windows.net/myshare/myfile.txt`|  
|Verzeichnisse|Flacher Namespace|Echte Verzeichnisobjekte|  
|Beachtung von Groß-/Kleinschreibung bei Namen|Groß-/Kleinschreibung beachten|Keine Beachtung von Groß-/Kleinschreibung, aber die Schreibweise wird beibehalten|  
|Kapazität|Container mit bis zu 500 TB|5 TB Dateifreigaben|  
|Durchsatz|Bis zu 60 MB/s pro Blockblob|Bis zu 60 MB/s pro Freigabe|  
|Objektgröße|Bis zu 200 GB/Blockblob|Bis zu 1 TB/Datei|  
|Berechnete Kapazität|Basierend auf geschriebenen Bytes|Basierend auf der Dateigröße|  
|Clientbibliotheken|Mehrere Sprachen|Mehrere Sprachen|  
  
## <a name="comparison-files-and-data-disks"></a>Vergleich: Azure Files und Datenträger

Azure Files ergänzt Azure-Datenträger. Ein Datenträger kann immer nur an eine Azure-VM angefügt werden. Datenträger sind VHDs mit festem Format, die als Seitenblobs in Azure Storage gespeichert werden. Sie werden von der VM verwendet, um permanente Daten zu speichern. Auf Dateifreigaben kann in Azure Files genauso zugegriffen werden wie auf den lokalen Datenträger (mit nativen Dateisystem-APIs). Sie können VM-übergreifend freigegeben werden.  
 
In der folgenden Tabelle wird Azure Files mit Azure-Datenträger verglichen.  
 
||||  
|-|-|-|  
|**Attribut**|**Azure-Datenträger**|**Azure Files**|  
|Umfang|Nur für einen einzelnen virtuellen Computer|Freigegebener Zugriff für mehrere virtuelle Computer|  
|Momentaufnahmen und Kopieren|Ja|Nein|  
|Konfiguration|Verbindung beim Start des virtuellen Computers|Verbindung nach dem Start des virtuellen Computers|  
|Authentifizierung|Integriert|Wird mit „net use“ eingerichtet|  
|Cleanup|Automatisch|Manuell|  
|Zugriff mithilfe von REST|Auf Dateien auf dem virtuellen Datenträger kann nicht zugegriffen werden.|Auf Dateien, die in einer Freigabe gespeicherte sind, kann zugegriffen werden.|  
|Max. Größe|1-TB-Datenträger|5-TB-Dateifreigabe und eine 1 TB große Datei innerhalb der Freigabe|  
|Max. 8 KB IOPS|500 IOPS|1.000 IOPS|  
|Durchsatz|Bis zu 60 MB/s pro Datenträger|Bis zu 60 MB/s pro Dateifreigabe|  

## <a name="next-steps"></a>Nächste Schritte

Bei der Entscheidung für die Art und Weise, wie Sie Ihre Daten speichern und auf diese zugreifen möchten, sollten Sie auch die Kosten berücksichtigen. Weitere Informationen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
  
Einige SMB-Funktion sind nicht in der Cloud verfügbar. Weitere Informationen finden Sie unter [Features not supported by the Azure File service (Funktionen, die vom Azure-Dateidienst nicht unterstützt werden)](/rest/api/storageservices/features-not-supported-by-the-azure-file-service).
  
Weitere Informationen zu Datenträgern finden Sie in den Artikeln [Informationen zu Datenträgern und VHDs für Azure-Linux-VMs](../../virtual-machines/windows/about-disks-and-vhds.md) und [Anfügen eines Datenträgers an einen virtuellen Windows-Computer, der mit dem klassischen Bereitstellungsmodell erstellt wurde](../../virtual-machines/windows/classic/attach-disk.md).
