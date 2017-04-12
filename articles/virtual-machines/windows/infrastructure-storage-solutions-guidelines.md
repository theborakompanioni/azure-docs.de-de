---
title: "Speicherlösungen für Windows-VMs in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für die Bereitstellung von Speicherlösungen in Azure-Infrastrukturdiensten."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ceb7d32-7a0d-4004-b701-c2bbcaff6b0b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 23ca92cc12ed0ff70a4ad6147609289eef061a93
ms.lasthandoff: 03/31/2017


---
# <a name="azure-storage-infrastructure-guidelines-for-windows-vms"></a>Richtlinien für die Azure-Speicherinfrastruktur für Windows-VMs

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

In diesem Artikel liegt das Hauptaugenmerk darauf, den Speicherbedarf und Entwurfsüberlegungen für eine optimale Leistung virtueller Computer (VMs) zu erläutern.

## <a name="implementation-guidelines-for-storage"></a>Implementierungsrichtlinien für Speicher
Entscheidungen:

* Verwenden Sie Azure Managed Disks oder nicht verwaltete Datenträger?
* Möchten Sie Standard- oder Premium-Speicher für Ihre Workload verwenden?
* Benötigen Sie Datenträgerstriping, um Datenträger mit mehr als 1023 GB zu erstellen?
* Benötigen Sie Datenträgerstriping, um eine optimale E/A-Leistung für Ihre Workload zu erreichen?
* Welche Speicherkonten benötigen Sie, um Ihre IT-Workload oder -Infrastruktur zu hosten?

Aufgaben:

* Überprüfen Sie die E/A-Anforderungen der Anwendung, die Sie bereitstellen, und planen Sie die entsprechende Anzahl und den Typ von Speicherkonten.
* Erstellen Sie die Gruppe von Speicherkonten mit Ihrer Benennungskonvention. Sie können Azure PowerShell oder das Portal verwenden.

## <a name="storage"></a>Speicher
Azure Storage ist ein wichtiger Bestandteil beim Bereitstellen und Verwalten von virtuellen Computern (VMs) und Anwendungen. Azure Storage bietet Dienste zum Speichern von Dateidaten, unstrukturierten Daten und Nachrichten und ist außerdem Teil der Infrastruktur zur Unterstützung virtueller Computer.

[Azure Managed Disks](../../storage/storage-managed-disks-overview.md) verwaltet die Speicherung für Sie im Hintergrund. Bei nicht verwalteten Datenträgern mussten Sie Speicherkonten für die Datenträger (VHD-Dateien) Ihrer Azure-VMs erstellen. Beim zentralen Hochskalieren mussten Sie sicherstellen, dass zusätzliche Speicherkonten erstellt wurden, um den IOPS-Speichergrenzwert für Ihre Datenträger nicht zu überschreiten. Wenn Managed Disks die Verwaltung des Speichers übernimmt, gelten für Sie die Speicherkonto-Grenzwerte (z.B. 20.000 IOPS/Konto) nicht mehr. Außerdem ist es nicht mehr erforderlich, Ihre benutzerdefinierten Images (VHD-Dateien) in mehrere Speicherkonten zu kopieren. Sie können sie an einem zentralen Ort verwalten – ein Speicherkonto pro Azure-Region – und nutzen, um Hunderte von VMs unter einem Abonnement zu erstellen. Es wird empfohlen, für neue Bereitstellungen Managed Disks zu verwenden.

Es sind zwei Arten von Speicherkonten zur Unterstützung virtueller Computer verfügbar:

* Mit einem Standardspeicherkonto erhalten Sie Zugriff auf Blobspeicher (zum Speichern von Datenträgern in virtuellen Azure-Computern) sowie auf Tabellen-, Warteschlangen- und Dateispeicher.
* [Storage Premium](../../storage/storage-premium-storage.md) bietet Datenträgerunterstützung mit hoher Leistung und niedriger Latenz für E/A-intensive Workloads wie SQL-Server in einem Always On-Cluster. Storage Premium unterstützt derzeit nur Datenträger in virtuellen Azure-Computern.

Azure erstellt virtuelle Computer mit einem Betriebssystem-Datenträger, einem temporären Datenträger und keinen oder mehreren optionalen Datenträgern für Daten. Der Betriebssystem-Datenträger und Datenträger mit Daten sind Azure-Seitenblobs, während der temporäre Datenträger sich im lokalen Speicher des Knotens befindet, in dem auch der Computer gespeichert ist. Achten Sie beim Anwendungsentwurf darauf, diesen temporären Datenträger nur für nicht persistente Daten zu verwenden, da der virtuelle Computer bei Wartungen zwischen Hosts migriert werden kann. Alle auf dem temporären Datenträger gespeicherten Daten würden verloren gehen.

Dauerhaftigkeit und hohe Verfügbarkeit werden von der zugrunde liegenden Azure Storage-Umgebung bereitgestellt, um sicherzustellen, dass Ihre Daten vor einer nicht geplante Wartung oder Hardwarefehlern geschützt bleiben. Beim Entwerfen Ihrer Azure Storage-Umgebung können Sie auswählen, wie VM-Speicher repliziert werden soll:

* lokal in einem bestimmten Azure-Rechenzentrum
* in mehreren Azure-Rechenzentren in einer bestimmten Region
* in mehreren Azure-Rechenzentren in verschiedenen Regionen

Informieren Sie sich über [weitere Replikationsoptionen für hohe Verfügbarkeit](../../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Betriebssystem-Datenträger und reine Datenträger haben eine maximale Größe von 1.023 Gigabyte (GB). Die maximale Größe eines Blobs ist 1.024 GB. Darin enthalten sein müssen die Metadaten (Fußzeile) der VHD-Datei (ein GB hat 1.024<sup>3</sup> Bytes). Mit Speicherplätzen unter Windows Server 2012 können Sie diese Beschränkung umgehen, indem Sie Datenträger für Daten zu einem Pool zusammenfassen, um dem virtuellen Computer logische Volumes mit mehr als 1023 GB bereitzustellen.

Es gibt einige Skalierbarkeitsgrenzwerte beim Entwurf der Azure Storage-Bereitstellung. Weitere Informationen finden Sie unter [Microsoft Azure-Abonnements und Diensteinschränkungen, Kontingente und Einschränkungen](../../azure-subscription-service-limits.md#storage-limits). Weitere Informationen finden Sie auch unter [Skalierbarkeits- und Leistungsziele für Azure Storage](../../storage/storage-scalability-targets.md).

Für Anwendungsspeicher können Sie unstrukturierte Objektdaten wie z.B. Dokumente, Bilder, Sicherungen, Konfigurationsdaten, Protokolle usw. mithilfe von Blob Storage speichern. Ihre Anwendung schreibt dann nicht auf eine virtuelle Festplatte, die dem virtuellen Computer angefügt ist, sondern direkt in den Azure-Blobspeicher. Blob Storage bietet je nach Verfügbarkeitsanforderungen und Budget auch die Option für [„heiße“ und „kalte“ Speicherebenen](../../storage/storage-blob-storage-tiers.md).

## <a name="striped-disks"></a>Stripesetdatenträger
Neben der Möglichkeit, Datenträger mit mehr als 1023 GB zu erstellen, wird durch Datenträgerstriping in vielen Fällen die Leistung verbessert, indem mehrere Blobs als Speicher für ein einzelnes Volume dienen können. Mit Striping läuft die erforderliche E/A zum Schreiben und Lesen von Daten aus einem einzigen logischen Datenträger parallel ab.

Azure erzwingt Grenzwerte für die Anzahl von Datenträgern und die verfügbare Bandbreite, die sich nach der Größe des virtuellen Computers richten. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Wenn Sie Datenträgerstriping für Azure-Datenträger verwenden, beachten Sie die folgenden Richtlinien:

* Datenträger sollten immer die maximale Größe (1.023 GB) haben.
* Fügen Sie die maximal zulässige Anzahl von Datenträgern für die Größe des virtuellen Computers an.
* Verwenden Sie Speicherplätze.
* Vermeiden Sie die Verwendung von Azure-Datenträger-Cachingoptionen (Cachingrichtlinie = Keine)

Weitere Informationen finden Sie unter [Storage Spaces – Designing for Performance](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx)(in englischer Sprache).

## <a name="multiple-storage-accounts"></a>Mehrere Speicherkonten
Dieser Abschnitt gilt nicht für [Azure Managed Disks](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), da Sie keine separaten Speicherkonten erstellen. 

Beim Entwerfen Ihrer Azure Storage-Umgebung für nicht verwaltete Datenträger können Sie mehrere Speicherkonten verwenden, wenn sich die Anzahl der bereitgestellten virtuellen Computer erhöht. Bei diesem Ansatz werden die E/A-Vorgänge auf die zugrunde liegende Azure Storage-Infrastruktur verteilt, um die optimale Leistung für Ihre virtuellen Computer und Anwendungen zu gewährleisten. Berücksichtigen Sie beim Entwerfen der Anwendungen, die bereitgestellt werden, die E/A-Anforderungen der einzelnen virtuellen Computer, und sorgen Sie für einen Ausgleich der virtuellen Computer über Azure-Speicherkonten. Vermeiden Sie das Gruppieren aller virtuellen Computer mit hohen E/A-Anforderungen in nur einem oder zwei Speicherkonten.

Weitere Informationen zu den E/A-Funktionen verschiedener Azure Storage-Optionen und einige empfohlene Höchstwerte finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](../../storage/storage-scalability-targets.md).

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]


