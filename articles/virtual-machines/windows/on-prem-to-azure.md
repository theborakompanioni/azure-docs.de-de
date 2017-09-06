---
title: Migrieren von AWS und anderen Plattformen zu Managed Disks in Azure | Microsoft-Dokumentation
description: Erstellen Sie VMs in Azure mithilfe von VHDs, die aus anderen Clouds wie AWS oder von anderen Virtualisierungsplattformen hochgeladen werden, und sichern Sie sich die Vorteile von Azure Managed Disks.
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
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 685c35dbd4265ca6852de6db2e5a30fc2a611d7c
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="migrate-from-amazon-web-services-aws-and-other-platforms-to-managed-disks-in-azure"></a>Migrieren von Amazon Web Services (AWS) und anderen Plattformen zu Managed Disks in Azure

Sie können VHD-Dateien aus AWS oder lokalen Virtualisierungslösungen in Azure hochladen, um VMs zu erstellen, die Managed Disks nutzen. Dank Azure Managed Disks ist es nicht mehr erforderlich, Speicherkonten für Azure IaaS-VMs zu verwalten. Sie müssen nur den Typ (Premium oder Standard) und die benötigte Größe des Datenträgers angeben, der anschließend von Azure erstellt und verwaltet wird. 

Sie können sowohl generalisierte als auch spezialisierte VHDs hochladen. 
- **Generalisierte VHD**: es wurden alle Ihre persönlichen Kontoinformationen mit Sysprep entfernt. 
- **Spezialisierte VHD**: werden die Benutzerkonten, Anwendungen und andere Zustandsdaten Ihres ursprünglichen VM beibehalten. 

> [!IMPORTANT]
> Bevor Sie eine VHD in Azure hochladen, befolgen Sie die Anweisungen unter [Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>


| Szenario                                                                                                                         | Dokumentation                                                                                                                       |
|----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| Sie verfügen über eine AWS EC2-Instanzen, die Sie zu Azure Managed Disks migrieren möchten.                                     | [Verschieben eines VM aus Amazon Web Services (AWS) zu Azure](aws-to-azure.md)                           |
| Sie verfügen über eine VM von einer anderen Virtualisierungsplattform, die Sie als Image zum Erstellen weiterer Azure-VMs nutzen möchten. | [Hochladen einer generalisierten VHD, und Verwendung dieser zum Erstellen eines neuen VMs in Azure](upload-generalized-managed.md) |
| Sie habe eine besonders angepasste VM, die Sie in Azure neu erstellen möchten.                                                      | [Hochladen einer spezialisierten VHD in Azure und Erstellen einer neuen VM](create-vm-specialized.md)         |


## <a name="overview-of-managed-disks"></a>Managed Disks (verwaltete Datenträger): Übersicht

Azure Managed Disks vereinfacht die VM-Verwaltung dadurch, dass die Verwaltung von Speicherkonten entfällt. Managed Disks profitieren auch von der höheren Zuverlässigkeit von VMs in einer Verfügbarkeitsgruppe. Es wird sichergestellt, dass die Datenträger verschiedener VMs in einer Verfügbarkeitsgruppe ausreichend voneinander isoliert sind, um einzelne Fehlerquellen zu vermeiden. Datenträger verschiedener VMs werden automatisch in einer Verfügbarkeitsgruppe in unterschiedlichen Skalierungseinheiten von Speicher (sog. „Stamps“) platziert. Dadurch werden die Auswirkungen des Ausfalls einzelner Speicherskalierungseinheiten eingedämmt, die von Hardware- und Softwarefehlern verursacht werden. Je nach Anforderungen stehen zwei Typen von Speicheroptionen zur Wahl: 
 
- [Premium Managed Disks](../../storage/common/storage-premium-storage.md) (verwaltete Premium-Datenträger) sind SSD-basierte (Solid State Drives) Speichermedien, die für virtuelle Computer mit E/A-intensiven Workloads eine hohe Datenträgerleistung mit niedriger Latenz bieten. Durch Migration zu Premium Managed Disks können Sie von der Geschwindigkeit und Leistung dieser Laufwerke profitieren.  

- [Standard Managed Disks](../../storage/common/storage-standard-storage.md) (verwaltete Standard-Datenträger) arbeiten mit HDD-basierten Speichermedien (mit Magnetfestplatten) und eignen sich hervorragend für Entwicklungs- und Testaufgaben sowie andere weniger häufig anfallende Workloads, bei denen Leistungsschwankungen keine große Rolle spielen.  

## <a name="plan-for-the-migration-to-managed-disks"></a>Planen der Migration zu Managed Disks

Dieser Abschnitt hilft Ihnen, hinsichtlich VM- und Datenträgertypen die beste Entscheidung zu treffen.


### <a name="location"></a>Standort

Wählen Sie einen Standort, an dem Azure Managed Disks verfügbar sind. Wenn Sie zu Premium Managed Disks migrieren, stellen Sie außerdem sicher, dass Storage Premium in der Region verfügbar ist, in die die Migration erfolgen soll. Aktuelle Informationen zu verfügbaren Standorten finden Sie unter [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services).

### <a name="vm-sizes"></a>VM-Größen

Wenn Sie zu Premium Managed Disks migrieren, müssen Sie die Größe der VM in eine Storage Premium-fähige Größe ändern, die in der Region mit dem Standort der VM verfügbar ist. Überprüfen Sie die Storage Premium-fähigen VM-Größen. Die Größenspezifikationen der Azure-VM sind unter [Größen für virtuelle Computer](sizes.md)aufgelistet.
Sehen Sie sich die Leistungsmerkmale von virtuellen Computern für Storage Premium an, und wählen Sie die am besten für Ihre Workload geeignete VM-Größe aus. Stellen Sie sicher, dass auf Ihrem virtuellen Computer ausreichend Bandbreite zum Steuern des Datenverkehrs des Datenträgers verfügbar ist.

### <a name="disk-sizes"></a>Datenträgergrößen

**Premium Managed Disks**

Es gibt drei verwaltete Premium-Datenträgertypen, die Sie mit Ihrer VM verwenden können. Jeder Typ weist bestimmte IOPS- und Durchsatzeinschränkungen auf. Berücksichtigen Sie diese Einschränkungen beim Auswählen des Premium-Datenträgertyps für Ihren virtuellen Computer je nach Anforderungen Ihrer Anwendung hinsichtlich Kapazität, Leistung, Skalierbarkeit und Spitzenlasten.

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

### <a name="disk-caching-policy"></a>Zwischenspeicherungsrichtlinie für Datenträger 

**Premium Managed Disks**

Standardmäßig ist die Richtlinie für das Zwischenspeichern für alle Premium-Datenträger *Schreibgeschützt* und für die Premium-Betriebssystem-Datenträger, die an den virtuellen Computer angeschlossen sind, *Lesen/Schreiben*. Diese Konfigurationseinstellung wird empfohlen, um die optimale E/A-Leistung für Ihre Anwendung zu erreichen. Für Datenträger mit hohem oder ausschließlichem Schreibzugriff (z. B. SQL Server-Protokolldateien) deaktivieren Sie das Zwischenspeichern, sodass Sie eine bessere Anwendungsleistung erzielen können.

### <a name="pricing"></a>Preise

Überprüfen Sie die [Preise für Managed Disks](https://azure.microsoft.com/en-us/pricing/details/managed-disks/). Die Preise für Premium Managed Disks (verwaltete Premium-Datenträger) sind identisch mit denen für Premium Unmanaged Disks (nicht verwaltete Premium-Datenträger). Doch die Preise für verwaltete Standard-Datenträger unterscheiden sich von denen für nicht verwaltete Standard-Datenträger.


## <a name="next-steps"></a>Nächste Schritte

- Bevor Sie eine VHD in Azure hochladen, befolgen Sie die Anweisungen unter [Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

