---
title: Speicherdienste von Azure Deutschland | Microsoft-Dokumentation
description: "Vergleich der Speicherdienste für Azure Deutschland"
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: ralfwi
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: ccc26feea0df4032f1ccdadbb86bd7f05a667783
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---

# Speicherdienste von Azure Deutschland
<a id="azure-germany-storage-services" class="xliff"></a>
## Storage
<a id="storage" class="xliff"></a>
Einzelheiten zu Azure Storage und seiner Verwendung finden Sie in der [globalen Dokumentation zu Storage](../storage/index.md).

Die in Azure Storage gespeicherten Daten werden repliziert, um eine hohe Verfügbarkeit sicherzustellen. Bei georedundanten Speichern und georedundanten Speichern mit Lesezugriff repliziert Azure Daten zwischen den *Regionspaaren*. Bei Azure Deutschland sind folgende Regionspaare verfügbar:

| Primäre Region | Sekundäre (zugehörige) Region |
| --- | --- |
| Deutschland, Mitte | Deutschland, Nordosten |
| Deutschland, Nordosten | Deutschland, Mitte |

Bei der Datenreplikation werden die Daten innerhalb deutscher Grenzen gehalten. Die primäre und sekundäre Region werden einander zugeordnet, um sicherzustellen, dass ausreichend Abstand zwischen den Rechenzentren besteht, um bei einem Ausfall eines großen Gebiets oder bei einem Notfall die Verfügbarkeit sicherzustellen. Wählen Sie bei georedundanten Speichern mit hoher Verfügbarkeit beim Erstellen eines Speicherkontos entweder georedundante Speicher oder georedundante Speicher mit Lesezugriff aus.  

Die Speicherdienstverschlüsselung schützt ruhende Daten in Azure Storage-Konten. Wenn Sie diese Funktion aktivieren, verschlüsselt Azure automatisch Daten, bevor sie in den Speicher eingefügt werden. Die Daten werden mithilfe einer 256-Bit-AES-Verschlüsselung verschlüsselt. Die Speicherdienstverschlüsselung unterstützt die Verschlüsselung von Block-, Anfüge- und Seitenblobs.

### Speicherdienstverfügbarkeit nach Azure Deutschland-Region
<a id="storage-service-availability-by-azure-germany-region" class="xliff"></a>

| Dienst | Deutschland, Mitte | Deutschland, Nordosten |
| --- | --- | --- | --- |
| [Blob Storage](../storage/storage-introduction.md#blob-storage) |Allgemein verfügbar |Allgemein verfügbar |
| [Table Storage](../storage/storage-introduction.md#table-storage) |Allgemein verfügbar  |Allgemein verfügbar |
| [Queue Storage](../storage/storage-introduction.md#queue-storage) |Allgemein verfügbar | Allgemein verfügbar |
| [File Storage](../storage/storage-introduction.md#file-storage) |Allgemein verfügbar |Allgemein verfügbar |
| [Hot/Cold Blob Storage](../storage/storage-blob-storage-tiers.md) |Allgemein verfügbar |Allgemein verfügbar |
| [Storage Service Encryption](../storage/storage-service-encryption.md) |Allgemein verfügbar |Allgemein verfügbar |
| [Storage Premium](../storage/storage-premium-storage.md) |Allgemein verfügbar |Allgemein verfügbar |
| Import/Export |Nicht verfügbar |Nicht verfügbar |
| StorSimple |Nicht verfügbar |Nicht verfügbar |

### Abweichungen
<a id="variations" class="xliff"></a>
Die URLs für Speicherkonten in Azure Deutschland unterscheiden sich von denen in der globalen Azure-Umgebung:

| Dienstart | Globale Azure-Umgebung | Azure Deutschland |
| --- | --- | --- |
| Blob Storage | *.blob.core.windows.net | *.blob.core.cloudapi.de |
| Queue Storage | *.queue.core.windows.net | *.queue.core.cloudapi.de |
| Table Storage | *.table.core.windows.net | *.table.core.cloudapi.de |
| File Storage | *.file.core.windows.net | *.file.core.cloudapi.de | 

> [!NOTE]
> Ihre gesamten Skripts und Ihr Code müssen die passenden Endpunkte berücksichtigen. Weitere Informationen hierzu finden Sie unter [Konfigurieren von Azure Storage-Verbindungszeichenfolgen](../storage/storage-configure-connection-string.md). 
>
>

Weitere Informationen zu APIs finden Sie unter [CloudStorageAccount-Konstruktor](https://msdn.microsoft.com/library/azure/mt616540.aspx).

Das zu verwendende Endpunktsuffix für diese Überladungen lautet *core.cloudapi.de*.

> [!NOTE]
> Wenn während der [Einbindung der Dateifreigabe](../storage/storage-dotnet-how-to-use-files.md) der Fehler 53 („Der Netzwerkpfad wurde nicht gefunden.“) zurückgegeben wird, blockiert möglicherweise eine Firewall den ausgehenden Port. Versuchen Sie, die Dateifreigabe auf einem virtuellen Computer einzubinden, der sich im selben Azure-Abonnement wie das Speicherkonto befindet.
>
>


## Nächste Schritte
<a id="next-steps" class="xliff"></a>
Abonnieren Sie den [Azure Deutschland-Blog](https://blogs.msdn.microsoft.com/azuregermany/), um weitere Informationen und Updates zu erhalten.

