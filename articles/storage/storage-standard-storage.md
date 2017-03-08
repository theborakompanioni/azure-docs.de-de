---
title: "Festplattenbasierter, kostengünstige Standardspeicher und Azure-VM-Datenträger | Microsoft-Dokumentation"
description: "Hier finden Sie Informationen zu kostengünstigem Standardspeicher und verwalteten/nicht verwalteten VM-Datenträgern."
services: storage
documentationcenter: 
author: yuemlu
manager: aungoo-msft
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/18/2017
ms.author: yuemlu
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: c208f44045ba414be2034f577435ae02ea4456cf
ms.lasthandoff: 02/27/2017


---
# <a name="cost-effective-standard-storage-and-unmanaged-and-managed-azure-vm-disks"></a>Kostengünstiger Standardspeicher und verwaltete/nicht verwaltete Azure-VM-Datenträger

Azure-Standardspeicher bietet zuverlässige, kostengünstige Datenträgerunterstützung für virtuelle Computer mit latenzempfindlichen Workloads. Darüber hinaus unterstützt er Blobs, Tabellen, Warteschlangen und Dateien. Bei Verwendung von Standardspeicher werden die Daten auf Festplattenlaufwerken (Hard Disk Drives, HDDs) gespeichert. Wenn Sie mit virtuellen Computern arbeiten, können Sie Standardspeicherdatenträger für Entwicklungs-und Testszenarien sowie für weniger kritische Workloads und Storage Premium-Datenträger für unternehmenskritische Produktionsanwendungen verwenden. Standardspeicher ist in allen Azure-Regionen verfügbar. 

Dieser Artikel beschäftigt sich hauptsächlich mit der Verwendung von Standardspeicher für VM-Datenträger. Weitere Informationen zur Verwendung von Speicher mit Blobs, Tabellen, Warteschlangen und Dateien finden Sie in der [Einführung in Storage](storage-introduction.md).

## <a name="disk-types"></a>Datenträgertypen

Standarddatenträger für virtuelle Azure-Computer können in zwei Varianten erstellt werden:

**Nicht verwaltete Datenträger:** Bei dieser ursprünglichen Methode verwalten Sie die Speicherkonten, die zum Speichern der VHD-Dateien für die VM-Datenträger verwendet werden. VHD-Dateien werden als Seitenblobs in Speicherkonten gespeichert. Nicht verwaltete Datenträger können an jede beliebige Azure-VM-Größe angefügt werden – auch an virtuelle Computer, die in erster Linie Storage Premium verwenden (wie etwa die DSv2- und die GS-Serie). Virtuelle Azure-Computer unterstützen das Anfügen mehrerer Standarddatenträger. Dadurch kann pro virtuellem Computer eine Speicherkapazität von bis zu 64 TB erreicht werden.

[**Azure Managed Disks**:](storage-managed-disks-overview.md) Dieses Feature nimmt Ihnen die Verwaltung der Speicherkonten ab, die für die VM-Datenträger verwendet werden. Sie geben die Art (Premium oder Standard) und die benötigte Größe des Datenträgers an, und Azure erstellt und verwaltet ihn für Sie. Sie müssen die Datenträger nicht selbst auf mehrere Speicherkonten verteilen, um sicherzustellen, dass Sie die Skalierbarkeitsgrenzwerte für die Speicherkonten einhalten. Dies erledigt Azure für Sie.

Grundsätzlich stehen Ihnen zwar beide Arten von Datenträgern zur Verfügung, wir empfehlen jedoch die Verwendung von Managed Disks, da Sie bei dieser Variante von zahlreichen Features profitieren.

Informationen zu den ersten Schritten mit Azure-Standardspeicher finden Sie unter [Erstellen Sie noch heute Ihr kostenloses Azure-Konto](https://azure.microsoft.com/pricing/free-trial/). 

Informationen zum Erstellen eines virtuellen Computers mit Managed Disks finden Sie in den folgenden Artikeln:

* [Erstellen eines virtuellen Computers mithilfe von Resource Manager und PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md)
* [Erstellen eines virtuellen Linux-Computers mit Azure CLI 2.0](../virtual-machines/virtual-machines-linux-quick-create-cli.md)

## <a name="standard-storage-features"></a>Features von Standardspeicher 

In diesem Abschnitt werden einige der Features von Standardspeicher erläutert. Ausführlichere Informationen finden Sie unter [Einführung in Microsoft Azure Storage](storage-introduction.md).

**Standardspeicher:** Azure-Standardspeicher unterstützt Azure-Datenträger, Azure-Blobs, Azure-Dateien, Azure-Tabellen und Azure-Warteschlangen. Wenn Sie Standardspeicherdienste verwenden möchten, beginnen Sie mit der [Erstellung eines Azure Storage-Kontos](storage-create-storage-account.md#create-a-storage-account).

**Standardspeicher-Datenträger:** Standardspeicher-Datenträger können an beliebige virtuelle Azure-Computer angefügt werden. Dies schließt auch virtuelle Computer aus Größenserien mit Storage Premium (beispielsweise die DSv2- und die GS-Serie) mit ein. Ein Standardspeicher-Datenträger kann nur an einen einzelnen virtuellen Computer angefügt werden. An einen virtuellen Computer können allerdings mehrere dieser Datenträger angefügt werden (bis zur maximalen, für die VM-Größe definierten Datenträgeranzahl). Die Spezifikationen werden im folgenden Abschnitt zu Skalierbarkeits- und Leistungszielen für Standardspeicher ausführlicher beschrieben. 

**Standardseitenblob:** Standardseitenblobs dienen zum Speichern persistenter Datenträger für virtuelle Computer, und Sie können wie bei anderen Arten von Azure-Blobs direkt über REST auf sie zugreifen. Bei [Seitenblobs](/rest/api/storageservices/fileservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) handelt es sich um eine Sammlung von 512-Byte-Seiten, die für zufällige Lese- und Schreibvorgänge optimiert sind. 

**Speicherreplikation:** In den meisten Regionen können in Standardspeicherkonten gespeicherte Daten lokal oder georedundant in mehreren Rechenzentren repliziert werden. Zur Verfügung stehen vier Replikationsarten: lokal redundanter Speicher (LRS), zonenredundanter Speicher (ZRS), georedundanter Speicher (GRS) und georedundanter Speicher mit Lesezugriff (RA-GRS). Für Managed Disks in Standardspeicher wird derzeit nur lokal redundanter Speicher (LRS) unterstützt. Weitere Informationen finden Sie unter [Azure Storage-Replikation](storage-redundancy.md).

## <a name="scalability-and-performance-targets"></a>Skalierbarkeits- und Leistungsziele

In diesem Abschnitt werden die Skalierbarkeits- und Leistungsziele beschrieben, die bei der Verwendung von Standardspeicher zu berücksichtigen sind.

### <a name="account-limits--does-not-apply-to-managed-disks"></a>Kontogrenzwerte (gelten nicht für verwaltete Datenträger)

| **Ressource** | **Standardlimit** |
|--------------|-------------------|
| TB pro Speicherkonto  | 500 TB |
| Max. Eingang<sup>1</sup> pro Speicherkonto (US-Regionen) | 10 GBit/s bei aktiviertem GRS/ZRS, 20 GBit/s für LRS |
| Max. Ausgang<sup>1</sup> pro Speicherkonto (US-Regionen) | 20 GBit/s bei aktiviertem RA-GRS/GRS/ZRS, 30 GBit/s für LRS |
| Max. Eingang<sup>1</sup> pro Speicherkonto (europäische und asiatische Regionen) | 5 GBit/s bei aktiviertem GRS/ZRS, 10 GBit/s für LRS |
| Max. Ausgang<sup>1</sup> pro Speicherkonto (europäische und asiatische Regionen) | 10 GBit/s bei aktiviertem RA-GRS/GRS/ZRS, 15 GBit/s für LRS |
| Gesamtanfragerate (ausgehend von einer Objektgröße von 1 KB) pro Speicherkonto | Bis zu 20.000 IOPS, Entitäten pro Sekunde oder Nachrichten pro Sekunde |

<sup>1</sup> „Eingang“ bezieht sich auf alle Daten (Anforderungen), die an ein Speicherkonto gesendet werden. „Ausgang“ bezieht sich auf alle Daten (Antworten), die von einem Speicherkonto empfangen werden.

Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungszielen für Azure Storage](storage-scalability-targets.md).

Wenn die Anforderungen Ihrer Anwendung die Skalierbarkeitsziele eines einzelnen Speicherkontos überschreiten, erstellen Sie die Anwendung so, dass mehrere Speicherkonten verwendet werden, und partitionieren Sie Ihre Daten in diesen Speicherkonten. Alternativ können Sie Azure Managed Disks verwenden und die Verwaltung der Partitionierung und Platzierung von Daten Azure überlassen.

### <a name="standard-disks-limits"></a>Grenzwerte für Standarddatenträger

Im Gegensatz zu Premium-Datenträgern werden für Standarddatenträger keine E/A-Vorgänge pro Sekunde (Input/Output Operations Per Second, IOPS) und kein Durchsatz (Bandbreite) bereitgestellt. Die Leistung von Standarddatenträgern hängt nicht von der Größe des Datenträgers ab, sondern von der Größe des virtuellen Computers, an den der jeweilige Datenträger angefügt ist. Erwartungsgemäß kann maximal der in der folgenden Tabelle angegebene Leistungsgrenzwert erreicht werden.

**Grenzwerte für (verwaltete und nicht verwaltete) Standarddatenträger**

| **VM-Tarif**            | **Virtueller Computer – Basic-Tarif** | **Virtueller Computer – Standard-Tarif** |
|------------------------|-------------------|----------------------|
| Maximale Datenträgergröße          | 1023 GB           | 1023 GB              |
| Max. 8 KB an IOPS pro Datenträger | Bis zu 300         | Bis zu 500            |
| Max. Bandbreite pro Datenträger | Bis zu 60 MB/s     | Bis zu 60 MB/s        |

Wenn Ihre Workload Datenträgerunterstützung für hohe Leistung und geringe Latenz erfordert, empfiehlt sich unter Umständen die Verwendung von Storage Premium. Weitere Informationen zu den Vorteilen von Storage Premium finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](storage-premium-storage.md). 

## <a name="snapshots-and-copy-blob"></a>Momentaufnahmen und Kopieren von Blobs

Für den Storage-Dienst ist die VHD-Datei ein Seitenblob. Sie können Momentaufnahmen von Seitenblobs erstellen und sie an einen anderen Ort kopieren (etwa in ein anderes Speicherkonto).

### <a name="unmanaged-disks"></a>Nicht verwaltete Datenträger

Für nicht verwaltete Standarddatenträger können [inkrementelle Momentaufnahmen](storage-incremental-snapshots.md) erstellt werden. Die Vorgehensweise ist dabei die gleiche wie bei Momentaufnahmen für Standardspeicher. Falls sich Ihr Quelldatenträger in einem lokal redundanten Speicherkonto befindet, empfiehlt es sich, Momentaufnahmen zu erstellen und in ein georedundantes Standardspeicherkonto zu kopieren. Weitere Informationen finden Sie unter [Redundanzoptionen für Azure Storage](storage-redundancy.md).

Falls ein Datenträger an einen virtuellen Computer angefügt ist, sind bestimmte API-Vorgänge für die Datenträger nicht zulässig. Sie können beispielsweise den Vorgang [Kopieren von Blob](/rest/api/storageservices/fileservices/Copy-Blob) für das Blob nicht durchführen, solange der Datenträger an den virtuellen Computer angeschlossen ist. Erstellen Sie stattdessen zunächst eine Momentaufnahme dieses Blobs, indem Sie die REST-API-Methode [Momentaufnahme-Blob](/rest/api/storageservices/fileservices/Snapshot-Blob) verwenden und anschließend den Vorgang [Kopieren von Blob](/rest/api/storageservices/fileservices/Copy-Blob) der Momentaufnahme durchführen, um den angeschlossenen Datenträger zu kopieren. Alternativ können Sie den Datenträger auch trennen und dann die erforderlichen Vorgänge ausführen.

Zur Aufbewahrung georedundanter Kopien Ihrer Momentaufnahmen können Sie Momentaufnahmen mithilfe von „AzCopy“ oder „Copy Blob“ aus einem lokal redundanten Speicherkonto in ein georedundantes Standardspeicherkonto kopieren. Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md) und [Copy Blob](/rest/api/storageservices/fileservices/Copy-Blob).

Ausführliche Informationen zum Ausführen von REST-Vorgängen für Seitenblobs in Standardspeicherkonten finden Sie in der [Referenz zur REST-API für Azure Storage Services](/rest/api/storageservices/fileservices/Azure-Storage-Services-REST-API-Reference).

### <a name="managed-disks"></a>Verwaltete Datenträger

Bei einer Momentaufnahme für einen verwalteten Datenträger handelt es sich um eine schreibgeschützte Kopie des verwalteten Datenträgers, die als verwalteter Standarddatenträger gespeichert wird. Für Managed Disks werden noch keine inkrementellen Momentaufnahmen unterstützt, dies ist jedoch bereits geplant.

Falls ein verwalteter Datenträger an einen virtuellen Computer angefügt ist, sind bestimmte API-Vorgänge für die Datenträger nicht zulässig. So können Sie beispielsweise keine Shared Access Signature (SAS) generieren, um einen Kopiervorgang auszuführen, während der Datenträger an einen virtuellen Computer angefügt ist. Erstellen Sie stattdessen zunächst eine Momentaufnahme des Datenträgers, und führen Sie dann den Kopiervorgang für die Momentaufnahme aus. Alternativ können Sie den Datenträger trennen und anschließend eine Shared Access Signature (SAS) generieren, um den Kopiervorgang auszuführen.

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Bei Verwendung von Standardspeicher sind folgende Abrechnungsaspekte zu berücksichtigen:

* Nicht verwaltete Standardspeicher-Datenträger/Datengröße 
* Verwaltete Standarddatenträger
* Standardspeicher-Momentaufnahmen
* Ausgehende Datenübertragungen
* Transaktionen

**Nicht verwaltete Speicherdaten und Datenträgergröße:** Bei nicht verwalteten Datenträgern und anderen Daten (Blobs, Tabellen, Warteschlangen und Dateien) fallen nur Kosten für den genutzten Speicherplatz an. Wenn Sie also beispielsweise über einen virtuellen Computer verfügen, bei dem das Seitenblob mit 127 GB bereitgestellt ist, aber nur 10 GB des Speicherplatzes tatsächlich genutzt werden, werden nur 10 GB in Rechnung gestellt. 

**Verwaltete Datenträger:** Verwaltete Datenträger werden nach bereitgestellter Größe abgerechnet. Wenn Ihr Datenträger mit einer Kapazität von 10 GB bereitgestellt wird, Sie aber nur 5 GB davon nutzen, wird trotzdem die bereitgestellte Größe von 10 GB in Rechnung gestellt.

**Momentaufnahmen:** Für Momentaufnahmen von Standarddatenträgern wird die zusätzliche Kapazität in Rechnung gestellt, die von den Momentaufnahmen beansprucht wird. Informationen zu Momentaufnahmen finden Sie unter [Erstellen einer Momentaufnahme eines Blobs](/rest/api/storageservices/fileservices/Creating-a-Snapshot-of-a-Blob).

**Ausgehende Datenübertragungen:**[Ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/data-transfers/) (Daten, die von den Azure-Rechenzentren ausgehen) verursachen Kosten bei der Bandbreitenverwendung.

**Transaktionen:** Bei Azure-Standardspeicher fällt pro 100.000 Transaktionen eine Gebühr von 0,0036 USD an. Transaktionen umfassen Lese- und Schreibvorgänge im Speicher.

Ausführliche Informationen zu den Preisen für Standardspeicher, Virtual Machines und Managed Disks finden Sie unter den folgenden Links:

* [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Preisgestaltung für virtuelle Computer](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Preise für Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks)

## <a name="azure-backup-service-support"></a>Azure Backup-Dienst – Unterstützung 

Virtuelle Azure-Computer mit nicht verwalteten Datenträgern können mithilfe von Azure Backup gesichert werden. [Weitere Informationen](../backup/backup-azure-vms-first-look-arm.md)

Der Azure Backup-Dienst kann auch in Kombination mit Managed Disks verwendet werden, um einen Sicherungsauftrag mit zeitbasierten Sicherungen, unkomplizierter Wiederherstellung von virtuellen Computern und Aufbewahrungsrichtlinien für Sicherungen zu erstellen. Weitere Informationen hierzu finden Sie im Artikel zur [Verwendung des Azure Backup-Diensts für virtuelle Computer mit Managed Disks](../backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Storage](storage-introduction.md)

* [Erstellen eines Speicherkontos](storage-create-storage-account.md)

* [Managed Disks Overview](storage-managed-disks-overview.md) (Übersicht über Azure Managed Disks)

* [Erstellen eines virtuellen Computers mithilfe von Resource Manager und PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md)

* [Erstellen eines virtuellen Linux-Computers mit Azure CLI 2.0](../virtual-machines/virtual-machines-linux-quick-create-cli.md)
