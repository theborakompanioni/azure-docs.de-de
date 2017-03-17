---
title: Migrieren von Azure-VMs zu Azure Managed Disks | Microsoft-Dokumentation
description: "Migrieren Sie virtuelle Azure-Computer, die in Speicherkonten mit nicht verwalteten Datenträgern erstellt wurden, zu Managed Disks."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: 9974b7a34749e689f6061b61056ae453690792e3
ms.lasthandoff: 03/10/2017


---

# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Migrieren von Azure-VMs zu Managed Disks in Azure

Azure Managed Disks vereinfacht die Speicherverwaltung dadurch, dass die gesonderte Verwaltung von Speicherkonten entfällt.  Sie können auch Ihre vorhandenen Azure-VMs zu Managed Disks migrieren, um von der höheren Zuverlässigkeit von VMs in einer Verfügbarkeitsgruppe zu profitieren. Es wird sichergestellt, dass die Datenträger verschiedener VMs in einer Verfügbarkeitsgruppe ausreichend voneinander isoliert sind, um einzelne Fehlerquellen zu vermeiden. Datenträger verschiedener VMs werden automatisch in einer Verfügbarkeitsgruppe in unterschiedlichen Skalierungseinheiten von Speicher (sog. „Stamps“) platziert. Dadurch werden die Auswirkungen des Ausfalls einzelner Speicherskalierungseinheiten eingedämmt, die von Hardware- und Softwarefehlern verursacht werden. Je nach Anforderungen stehen zwei Typen von Speicheroptionen zur Wahl: 
 
- [Verwaltete Premium-Datenträger](../storage/storage-premium-storage.md) sind SSD-basierte (Solid State Drives) Speichermedien, die für virtuelle Computer mit E/A-intensiven Workloads eine hohe Datenträgerleistung mit niedriger Latenz bieten. Durch Migration zu Premium Managed Disks können Sie von der Geschwindigkeit und Leistung dieser Laufwerke profitieren.

- [Standard Managed Disks](../storage/storage-standard-storage.md) (verwaltete Standard-Datenträger) arbeiten mit HDD-basierten Speichermedien (mit Magnetfestplatten) und eignen sich hervorragend für Entwicklungs- und Testaufgaben sowie andere weniger häufig anfallende Workloads, bei denen Leistungsschwankungen keine große Rolle spielen. 

Sie können in folgenden Szenarien zu Managed Disks migrieren:

| Migrieren Sie Folgendes:                                            | Link zur Dokumentation                                                                                                                                                                                                                                                                  |
|----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| VM in einer Verfügbarkeitsgruppe, die nicht verwaltete Datenträger verwenden, zu verwalteten Datenträgern   | [Durchführen der Konvertierung in verwaltete Datenträger für eine VM in einer Verfügbarkeitsgruppe](virtual-machines-windows-convert-unmanaged-to-managed-disks.md#convert-vms-in-an-availability-set-to-managed-disks-in-a-managed-availability-set)                                                                        |
| Nicht verwaltete Premium-Datenträger zu verwalteten Premium-Datenträgern   | [Migrieren vorhandener Azure-VMs zur Verwendung verwalteter Datenträger desselben Speichertyps](virtual-machines-windows-convert-unmanaged-to-managed-disks.md#convert-existing-azure-vms-to-managed-disks-of-the-same-storage-type)                                                                         |
| Nicht verwaltete Standard-Datenträger zu verwalteten Standard-Datenträgern | [Migrieren vorhandener Azure-VMs zur Verwendung verwalteter Datenträger desselben Speichertyps](virtual-machines-windows-convert-unmanaged-to-managed-disks.md#convert-existing-azure-vms-to-managed-disks-of-the-same-storage-type)                                                                         |
| Nicht verwaltete Standard-Datenträger zu verwalteten Premium-Datenträgern  | [Migrieren vorhandener Azure VMs mit nicht verwalteten Standard-Datenträgern zu Premium Managed Disks](virtual-machines-windows-convert-unmanaged-to-managed-disks.md#migrate-existing-azure-vms-using-standard-unmanaged-disks-to-premium-managed-disks)                            |
| Eine einzelne VM aus dem klassischen Bereitstellungsmodell zum Ressourcen-Manager-Bereitstellungsmodell auf verwalteten Datenträgern     | [Migrieren einer einzelnen VM](virtual-machines-windows-migrate-single-classic-to-resource-manager.md)  | 
| Alle VMs in einem VNet aus dem klassischen Bereitstellungsmodell zum Ressourcen-Manager-Bereitstellungsmodell auf verwalteten Datenträgern     | [Migrieren von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zum Ressourcen-Manager-Bereitstellungsmodell](virtual-machines-windows-ps-migration-classic-resource-manager.md) und dann [Konvertieren eines virtuellen Computers von nicht verwalteten Datenträgern in verwaltete Datenträger](virtual-machines-windows-convert-unmanaged-to-managed-disks.md) | 





## <a name="plan-for-the-conversion-to-managed-disks"></a>Planen der Migration zu Managed Disks

Dieser Abschnitt hilft Ihnen, hinsichtlich VM- und Datenträgertypen die beste Entscheidung zu treffen.


## <a name="location"></a>Standort

Wählen Sie einen Standort, an dem Azure Managed Disks verfügbar sind. Wenn Sie zu Premium Managed Disks wechseln, stellen Sie außerdem sicher, dass Storage Premium in der Region verfügbar ist, zu der der Wechsel erfolgen soll. Aktuelle Informationen zu verfügbaren Standorten finden Sie unter [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services).

## <a name="vm-sizes"></a>VM-Größen

Wenn Sie zu Premium Managed Disks migrieren, müssen Sie die Größe der VM in eine Storage Premium-fähige Größe ändern, die in der Region mit dem Standort der VM verfügbar ist. Überprüfen Sie die Storage Premium-fähigen VM-Größen. Die Größenspezifikationen der Azure-VM sind unter [Größen für virtuelle Computer](virtual-machines-windows-sizes.md)aufgelistet.
Sehen Sie sich die Leistungsmerkmale von virtuellen Computern für Storage Premium an, und wählen Sie die am besten für Ihre Workload geeignete VM-Größe aus. Stellen Sie sicher, dass auf Ihrem virtuellen Computer ausreichend Bandbreite zum Steuern des Datenverkehrs des Datenträgers verfügbar ist.

## <a name="disk-sizes"></a>Datenträgergrößen

**Premium Managed Disks**

Es gibt drei verwaltete Premium-Datenträgertypen, die Sie mit Ihrer VM verwenden können. Jeder Typ weist bestimmte IOPS- und Durchsatzeinschränkungen auf. Berücksichtigen Sie diese Beschränkungen beim Auswählen des Premium-Datenträgertyps für Ihren virtuellen Computer je nach Anforderungen Ihrer Anwendung hinsichtlich Kapazität, Leistung, Skalierbarkeit und Spitzenlasten.

| Premium-Datenträgertyp  | P10               | P20               | P30               |
|---------------------|-------------------|-------------------|-------------------|
| Datenträgergröße           | 128 GB            | 512 GB            | 1024 GB (1 TB)    |
| IOPS pro Datenträger       | 500               | 2.300              | 5.000              |
| Durchsatz pro Datenträger | 100 MB pro Sekunde | 150 MB pro Sekunde | 200 MB pro Sekunde |

**Verwaltete Standard-Datenträger**

Es gibt fünf Arten von verwalteten Standard-Datenträgern, die mit Ihrer VM verwendet werden können. Jede davon bietet unterschiedliche Kapazität, jedoch gelten für alle dieselben IOPS- und Durchsatzgrenzwerte. Wählen Sie den Typ der verwalteten Standard-Datenträger basierend auf den Kapazitätsanforderungen Ihrer Anwendung.

| Standard-Datenträgertyp  | S4               | S6               | S10              | S20              | S30              |
|---------------------|------------------|------------------|------------------|------------------|------------------|
| Datenträgergröße           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1024 GB (1 TB)   |
| IOPS pro Datenträger       | 500              | 500              | 500              | 500              | 500              |
| Durchsatz pro Datenträger | 60 MB pro Sekunde | 60 MB pro Sekunde | 60 MB pro Sekunde | 60 MB pro Sekunde | 60 MB pro Sekunde |

## <a name="disk-caching-policy"></a>Zwischenspeicherungsrichtlinie für Datenträger 

**Premium Managed Disks**

Standardmäßig ist die Richtlinie für das Zwischenspeichern für alle Premium-Datenträger *Schreibgeschützt* und für die Premium-Betriebssystem-Datenträger, die an den virtuellen Computer angeschlossen sind, *Lesen/Schreiben*. Diese Konfigurationseinstellung wird empfohlen, um die optimale E/A-Leistung für Ihre Anwendung zu erreichen. Für Datenträger mit hohem oder ausschließlichem Schreibzugriff (z. B. SQL Server-Protokolldateien) deaktivieren Sie das Zwischenspeichern, sodass Sie eine bessere Anwendungsleistung erzielen können.

## <a name="pricing"></a>Preise

Überprüfen Sie die [Preise für Managed Disks](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Die Preise für Premium Managed Disks (verwaltete Premium-Datenträger) sind identisch mit denen für Premium Unmanaged Disks (nicht verwaltete Premium-Datenträger). Doch die Preise für verwaltete Standard-Datenträger unterscheiden sich von denen für nicht verwaltete Standard-Datenträger.



## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Managed Disks](../storage/storage-managed-disks-overview.md).

