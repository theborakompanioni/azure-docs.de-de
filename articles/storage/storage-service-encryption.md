---
title: "Azure Storage Service Encryption für ruhende Daten | Microsoft Docs"
description: "Verwenden Sie Azure Storage Service Encryption, um Ihren Azure-Blobspeicher auf der Dienstseite zu verschlüsseln, wenn die Daten gespeichert werden, und beim Abrufen der Daten zu entschlüsseln."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: edabe3ee-688b-41e0-b34f-613ac9c3fdfd
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: cd727a02716323c54d1c54a23cd4edd8f4940da1
ms.lasthandoff: 03/15/2017


---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage Service Encryption für ruhende Daten
Azure Storage Service Encryption (SSE) für ruhende Daten unterstützt Sie dabei, Ihre Daten zu schützen, um die Sicherheits- und Complianceverpflichtungen Ihrer Organisation zu erfüllen. Mit diesem Feature verschlüsselt Azure Storage Ihre Daten automatisch, bevor sie im Speicher abgelegt werden, und entschlüsselt sie vor dem Abrufen. Verschlüsselung, Entschlüsselung und Schlüsselverwaltung sind für Benutzer vollständig transparent.

Die folgenden Abschnitte enthalten eine ausführliche Anleitung zur Verwendung der Features von Storage Service Encryption sowie die unterstützten Szenarien und Benutzererfahrungen.

## <a name="overview"></a>Übersicht
Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Daten können während der Übertragung zwischen einer Anwendung und Azure mit [clientseitiger Verschlüsselung](storage-client-side-encryption.md), HTTPS oder SMB 3.0 geschützt werden. Storage Service Encryption (SSE) bietet eine Verschlüsselung ruhender Daten, die Verarbeitung der Ver- und Entschlüsselung sowie die Schlüsselverwaltung auf vollständig transparente Weise. Sämtliche Daten werden mittels 256-Bit- [AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)verschlüsselt, einem der sichersten verfügbaren Blockverschlüsselungsverfahren.

SSE verschlüsselt die Daten, wenn sie in Azure Storage geschrieben werden, und kann für Azure Blob Storage und File Storage (Vorschau) verwendet werden. Das Feature funktioniert wie folgt:

* Allgemeine Speicherkonten und Blobspeicherkonten
* Storage Standard und Storage Premium 
* Alle Redundanzebenen (LRS, ZRS, GRS, RA-GRS)
* Azure Resource Manager-Speicherkonten (nicht jedoch klassische Konten) 
* Alle Bereiche für Blob Storage. Überprüfen Sie für File Storage den Abschnitt zur Verfügbarkeit.

Storage Service Encryption – SSE-Dateivorschau steht jetzt zum Verschlüsseln Ihrer Daten in File Storage zur Verfügung. Sie befindet sich derzeit in der Vorschauphase. Nachfolgend sehen Sie eine Liste der Regionen, in denen SSE für File Storage verfügbar ist.

Zur Teilnahme an der SSE-Dateivorschau wenden Sie sich an ssediscussions@microsoft.com.

Weitere Informationen erhalten Sie in den FAQ.

### <a name="availability-for-file-storage"></a>Verfügbarkeit für File Storage
Speicherdienstverschlüsselung für File Storage ist derzeit in den Regionen USA, Osten; USA, Osten&2; USA, Mitte; USA, Norden-Mitte; USA, Süden-Mitte, USA, Westen-Mitte; USA, Westen; USA, Westen&2; Asien, Osten; Europa, Norden; Europa, Westen, Australien, Osten und Australien, Südosten verfügbar.


Melden Sie sich zum Aktivieren oder Deaktivieren der Speicherdienstverschlüsselung für ein Speicherkonto beim [Azure-Portal](https://azure.portal.com) an, und wählen Sie ein Speicherkonto aus. Suchen Sie auf dem Blatt „Einstellungen“ nach dem Abschnitt mit dem Blobdienst, und klicken Sie auf „Verschlüsselung“.

![Portal-Screenshot mit der Verschlüsselungsoption](./media/storage-service-encryption/image1.png)
<br/>*Abbildung 1: Aktivieren von SSE für Blob-Dienst (Schritt 1)*

![Portal-Screenshot mit der Verschlüsselungsoption](./media/storage-service-encryption/image3.png)
<br/>*Abbildung 2: Aktivieren von SSE für File Service (Schritt 1)*

Nach Klicken auf die Einstellung „Verschlüsselung“ können Sie Storage Service Encryption aktivieren oder deaktivieren.

![Portal-Screenshot mit Verschlüsselungseigenschaften](./media/storage-service-encryption/image2.png)
<br/>*Abbildung 1.1: Aktivieren von SSE für Blob-Dienst (Schritt 2)*

![Portal-Screenshot mit Verschlüsselungseigenschaften](./media/storage-service-encryption/image4.png)
<br/>*Abbildung 2.1: Aktivieren von SSE für File Service (Schritt 2)*
## <a name="encryption-scenarios"></a>Verschlüsselungsszenarien
Storage Service Encryption kann auf einer Speicherkontoebene aktiviert werden. Folgende Kundenszenarien werden unterstützt:

* Verschlüsselung von Blob Storage und File Storage
* Die Verschlüsselung von klassischen Speicherkonten, die zu Resource Manager-Speicherkonten migriert wurden, wird für die Verschlüsselung für Blob-Dienst, jedoch nicht für File Service unterstützt.
* Die Verschlüsselung für File Storage wird nur für neu erstellte Speicherkonten unterstützt.

Für SSE gelten die folgenden Einschränkungen:

* Verschlüsselung von klassischen Speicherkonten wird nicht unterstützt.
* Die Verschlüsselung von klassischen Speicherkonten, die zu Resource Manager-Speicherkonten migriert wurden, wird für die Verschlüsselung für Blob-Dienst, jedoch nicht für File Service unterstützt.
* Die Verschlüsselung für File Storage wird nur für neu erstellte Speicherkonten unterstützt.
* Vorhandene Daten – SSE verschlüsselt nur nach Aktivierung der Verschlüsselung neu erstellte Daten. Wenn Sie z.B. ein neues Resource Manager-Speicherkonto erstellen, jedoch keine Verschlüsselung aktivieren, und dann Blobs oder archivierte VHDs in das Speicherkonto hochladen und SSE aktivieren, werden diese Blobs nicht verschlüsselt, sofern sie nicht neu geschrieben oder kopiert werden.
* Unterstützung für Marketplace – Aktivieren der Verschlüsselung von virtuellen Computern, die über den Marketplace mit dem [Azure-Portal](https://portal.azure.com), mit PowerShell und mit der Azure-Befehlszeilenschnittstelle erstellt werden. Das VHD-Basisimage bleibt unverschlüsselt. Allerdings werden alle Schreibvorgänge verschlüsselt, die nach Erstellen der VM stattfinden.
* Tabellen- und Warteschlangendaten werden nicht verschlüsselt.

## <a name="getting-started"></a>Erste Schritte
### <a name="step-1-create-a-new-storage-accountstorage-create-storage-accountmd"></a>Schritt 1: [Erstellen eines neuen Speicherkontos](storage-create-storage-account.md)
### <a name="step-2-enable-encryption"></a>Schritt 2: Aktivieren der Verschlüsselung
Sie können die Verschlüsselung im [Azure-Portal](https://portal.azure.com)aktivieren.

> [!NOTE]
> Wenn Sie SSE programmgesteuert für ein Speicherkonto aktivieren oder deaktivieren möchten, können Sie die [REST-API des Azure Storage-Ressourcenanbieters](https://msdn.microsoft.com/library/azure/mt163683.aspx), die [Clientbibliothek des Speicherressourcenanbieters für .NET](https://msdn.microsoft.com/library/azure/mt131037.aspx), [Azure PowerShell](/powershell/azureps-cmdlets-docs) oder die [Azure-CLI](storage-azure-cli.md) verwenden.
> 
> 

### <a name="step-3-copy-data-to-storage-account"></a>Schritt 3: Kopieren von Daten in das Speicherkonto
Wenn Sie SSE für Blob-Dienst aktivieren, werden alle Blobs in diesem Speicherkonto verschlüsselt. Alle Blobs, die sich bereits in diesem Speicherkonto befinden, werden erst verschlüsselt, nachdem sie erneut geschrieben wurden. Sie können die Daten aus einem Speicherkonto in ein mit SSE verschlüsseltes Konto kopieren und sogar SSE aktivieren und die Blobs aus einem Container in einen anderen kopieren, um sicherzustellen, dass ältere Daten verschlüsselt werden. Sie können dazu eines der folgenden Tools verwenden. Dies ist dasselbe Verhalten wie auch bei File Storage.

#### <a name="using-azcopy"></a>Verwenden von AzCopy
AzCopy ist ein Windows Befehlszeilenprogramm, das zum Kopieren von Daten zu und von Microsoft Azure Blob, File und Table Storage entwickelt wurde, wobei durch einfache Befehle optimale Leistung erzielt wird. Mit diesem Tool können Sie Ihre Blobs oder Dateien aus einem Speicherkonto in ein anderes kopieren, für das SSE aktiviert ist. 

Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md).

#### <a name="using-smb"></a>Verwendung von SMB
Der Azure-Dateispeicher verfügt über Dateifreigaben in der Cloud unter Verwendung des standardmäßigen SMB-Protokolls. Sie können eine Dateifreigabe auf einem Client lokal oder in Azure bereitstellen. Nach der Bereitstellung können Tools wie Robocopy zum Kopieren von Dateien auf Azure-Dateifreigaben verwendet werden. Weitere Informationen finden Sie unter [Bereitstellen der Azure-Dateifreigabe unter Windows](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-files#mount-the-file-share) und [Bereitstellen der Azure-Dateifreigabe unter Linux](https://docs.microsoft.com/en-us/azure/storage/storage-how-to-use-files-linux#mount-the-file-share).


#### <a name="using-the-storage-client-libraries"></a>Verwenden von Speicherclientbibliotheken
Sie können mithilfe unseres umfangreichen Angebots an Speicherclientbibliotheken, einschließlich .NET, C++, Java, Android, Node.js, PHP, Python und Ruby, Blob- oder Dateidaten in und aus Blobspeicher oder zwischen Speicherkonten kopieren.

Weitere Informationen finden Sie unter [Erste Schritte mit Azure Blob Storage mit .NET](storage-dotnet-how-to-use-blobs.md).

#### <a name="using-a-storage-explorer"></a>Verwenden eines Storage-Explorers
Mit einem Storage-Explorer können Sie Speicherkonten erstellen, Daten hoch- und herunterladen, Inhalte von Blobs anzeigen und durch Verzeichnisse navigieren. Sie können eines dieser Konten zum Hochladen von Blobs in Ihr Speicherkonto mit aktivierter Verschlüsselung verwenden. Mit einigen Storage-Explorern können Sie auch Daten aus Ihrem vorhandenen Blobspeicher in einen anderen Container in Ihrem Speicherkonto oder ein neues Speicherkonto kopieren, für das SSE aktiviert ist.

Weitere Informationen finden Sie unter [Microsoft Azure-Speicher-Explorer](storage-explorers.md).

### <a name="step-4-query-the-status-of-the-encrypted-data"></a>Schritt 4: Abfragen des Status der verschlüsselten Daten
Eine aktualisierte Version der Speicherclientbibliotheken wurde bereitgestellt, mit denen Sie den Status eines Objekts ermitteln können, um zu bestimmen, ob es verschlüsselt ist oder nicht. Dies ist derzeit nur für Blob Storage verfügbar. Support für File Storage ist allerdings geplant. 

Rufen Sie in der Zwischenzeit [Kontoeigenschaften abrufen](https://msdn.microsoft.com/library/azure/mt163553.aspx) auf, um zu überprüfen, ob die Verschlüsselung für das Speicherkonto aktiviert wurde, oder zeigen Sie die Eigenschaften des Speicherkontos im Azure-Portal an.

## <a name="encryption-and-decryption-workflow"></a>Verschlüsselungs- und Entschlüsselungsworkflow
Hier ist eine kurze Beschreibung des Verschlüsselungs-/Entschlüsselungsworkflows:

* Der Kunde aktiviert die Verschlüsselung für das Speicherkonto.
* Wenn der Kunde neue Daten (PUT Blob, PUT Block, PUT Page, PUT File usw.) in Blob Storage oder in File Storage schreibt, wird jeder Schreibvorgang mit 256-Bit-[AES-Verschlüsselung](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) verschlüsselt, einem der sichersten verfügbaren Blockverschlüsselungsverfahren.
* Wenn der Kunde auf Daten zugreifen muss (GET Blob usw.), werden die Daten vor der Rückgabe an den Benutzer automatisch entschlüsselt.
* Wenn die Verschlüsselung deaktiviert ist, werden neue Schreibvorgänge nicht mehr verschlüsselt, und vorhandene verschlüsselte Daten bleiben verschlüsselt, bis sie vom Benutzer neu geschrieben werden. Bei aktivierter Verschlüsselung werden Schreibvorgänge in Blob Storage oder in File Storage verschlüsselt. Der Zustand der Daten ändert sich nicht, wenn der Benutzer zwischen Aktivieren und Deaktivieren der Verschlüsselung für das Speicherkonto umschaltet.
* Alle Verschlüsselungsschlüssel werden von Microsoft gespeichert, verschlüsselt und verwaltet.

## <a name="frequently-asked-questions-about-storage-service-encryption-for-data-at-rest"></a>Häufig gestellte Fragen zu Storage Service Encryption für ruhende Daten
**F: Ich habe ein klassisches Speicherkonto. Kann ich dafür SSE aktivieren?**

A: Nein, SSE wird nur für Resource Manager-Speicherkonten unterstützt.

**F: Wie kann ich Daten in meinem klassischen Speicherkonto verschlüsseln?**

A: Sie können ein neues Resource Manager-Speicherkonto erstellen und Ihre Daten mit [AzCopy](storage-use-azcopy.md) aus Ihrem vorhandenen klassischen Speicherkonto in Ihr neu erstelltes Resource Manager-Speicherkonto kopieren. 

Während der Migration des klassischen Speicherkontos zu einem Resource Manager-Speicherkonto werden die Daten nicht verschlüsselt. Wenn Sie das Speicherkonto jedoch migrieren und dann die Verschlüsselung aktivieren, werden sämtliche neuen Daten verschlüsselt, die in das Speicherkonto geschrieben werden. Weitere Informationen finden Sie unter [Plattformgestützte Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Azure Resource Manager](https://azure.microsoft.com/blog/iaas-migration-classic-resource-manager/). Beachten Sie, dass dies nur für Blob Storage unterstützt wird. Für die File Storage-Vorschau müssen Benutzer neue Resource Manager-Speicherkonten erstellen.

**F: Ich habe ein vorhandenes Resource Manager-Speicherkonto. Kann ich dafür SSE aktivieren?**

A: Ja, aber nur neu geschriebene Blobs werden verschlüsselt. Ältere, bereits vorhandene Daten werden nicht verschlüsselt. Dies wird jedoch für die File Storage-Vorschau noch nicht unterstützt.

**F: Kann ich die aktuellen Daten in einem vorhandenen Resource Manager-Speicherkonto verschlüsseln?**

A: Sie können SSE jederzeit in einem Resource Manager-Speicherkonto aktivieren. Blobs, die bereits vorhanden waren, werden jedoch nicht verschlüsselt. Um diese Blobs zu verschlüsseln, können Sie sie unter einem anderen Namen oder in einen anderen Container kopieren und anschließend die nicht verschlüsselten Versionen entfernen. Dies wird jedoch für die File Storage-Vorschau noch nicht unterstützt.

**F: Ich verwende Storage Premium; kann ich SSE verwenden?**

A: Ja, SSE wird von Standard Storage und Storage Premium unterstützt. Dies wird jedoch für die File Storage-Vorschau noch nicht unterstützt.

**F: Wenn ich ein neues Speicherkonto erstelle, SSE aktiviere und dann eine neue VM erstelle, die dieses Speicherkonto verwendet, bedeutet dies, dass mein virtueller Computer verschlüsselt ist?**

A: Ja. Alle erstellten Datenträger, die das neue Speicherkonto verwenden, werden verschlüsselt, sofern sie nach der SSE-Aktivierung erstellt wurden. Wurde die VM mit Azure Market Place erstellt, bleibt das VHD-Basisimage unverschlüsselt. Allerdings werden alle Schreibvorgänge verschlüsselt, die nach Erstellen der VM stattfinden.

**F: Kann ich neue Speicherkonten mithilfe von Azure PowerShell und Azure-CLI erstellen, wenn SSE aktiviert ist?**

A: Ja.

**F: Wie hoch sind die Mehrkosten für Azure Storage, wenn SSE aktiviert ist?**

A: Es fallen keine zusätzlichen Kosten an.

**F: Wer verwaltet die Verschlüsselungsschlüssel?**

A: Die Schlüssel werden von Microsoft verwaltet.

**F: Kann ich meine eigenen Verschlüsselungsschlüssel verwenden?**

A: Wir arbeiten darauf hin, dass Kunden ihre eigenen Verschlüsselungsschlüssel verwenden können.

**F: Kann ich den Zugriff auf die Verschlüsselungsschlüssel widerrufen?**

A: Derzeit nicht; die Schlüssel werden vollständig von Microsoft verwaltet.

**F: Ist SSE standardmäßig aktiviert, wenn ich ein neues Speicherkonto erstelle?**

A: SSE ist nicht standardmäßig aktiviert. Sie können dieses Feature im Azure-Portal aktivieren. Außerdem können Sie dieses Feature programmgesteuert mit der Speicherressourcenanbieter-REST-API aktivieren.

**F: Wo liegt der Unterschied zu Azure Disk Encryption?**

A: Dieses Feature wird zum Verschlüsseln von Daten im Azure-Blobspeicher verwendet. Azure Disk Encryption wird zum Verschlüsseln von Betriebssystemdatenträgern und sonstigen Datenträgern in virtuellen IaaS-Computern verwendet. Weitere Informationen finden Sie in unserem [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md).

**F: Was geschieht, wenn ich SSE aktiviere, und dann Azure Disk Encryption auf den Datenträgern aktiviere?**

A: Dies funktioniert nahtlos. Ihre Daten werden durch beide Methoden verschlüsselt.

**F: Mein Speicherkonto ist so eingerichtet, dass es georedundant repliziert wird. Werden meine redundanten Kopien ebenfalls verschlüsselt, wenn ich SSE aktiviere?**

A: Ja, alle Kopien des Speicherkontos werden verschlüsselt, und alle Redundanzoptionen – lokal redundanter Speicher (LRS), zonenredundanter Speicher (ZRS), georedundanter Speicher (GRS) und lesezugriff-georedundanter Speicher (RA-GRS, Read-Access Geo Redundant) werden unterstützt.

**F: Ich kann die Verschlüsselung nicht für mein Speicherkonto aktivieren.**

A: Handelt es sich um ein Resource Manager-Speicherkonto? Klassische Speicherkonten werden nicht unterstützt. 

**F: Ist SSE nur in bestimmten Regionen möglich?**

A: SSE ist in allen Regionen für Blob Storage verfügbar. Überprüfen Sie den Abschnitt zur Verfügbarkeit für File Storage. 

**F: Wohin wende ich mich, wenn Probleme auftreten oder ich Feedback geben möchte?**

A: Bitte wenden Sie sich in allen Angelegenheiten, die Storage Service Encryption betreffen, an [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com) .

## <a name="next-steps"></a>Nächste Schritte
Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Weitere Informationen finden Sie im [Azure Storage-Sicherheitsleitfaden](storage-security-guide.md).


