<properties
	pageTitle="Richtlinien für Speicherlösungen | Microsoft Azure"
	description="Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für die Bereitstellung von Speicherlösungen in Azure-Infrastrukturdiensten."
	documentationCenter=""
	services="virtual-machines-windows"
	authors="iainfoulds"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/30/2016"
	ms.author="iainfou"/>

# Richtlinien für die Speicherinfrastruktur

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

In diesem Artikel liegt das Hauptaugenmerk darauf, den Speicherbedarf und Entwurfsüberlegungen für eine optimale Leistung virtueller Computer (VMs) zu erläutern.


## Implementierungsrichtlinien für Speicher

Entscheidungen:

- Möchten Sie Standard- oder Premium-Speicher für Ihre Workload verwenden?
- Benötigen Sie Datenträgerstriping, um Datenträger mit mehr als 1023 GB zu erstellen?
- Benötigen Sie Datenträgerstriping, um eine optimale E/A-Leistung für Ihre Workload zu erreichen?
- Welche Speicherkonten benötigen Sie, um Ihre IT-Workload oder -Infrastruktur zu hosten?

Aufgaben:

- Überprüfen Sie die E/A-Anforderungen der Anwendung, die Sie bereitstellen, und planen Sie die entsprechende Anzahl und den Typ von Speicherkonten.
- Erstellen Sie die Gruppe von Speicherkonten mit Ihrer Benennungskonvention. Sie können Azure PowerShell oder das Portal verwenden.


## Speicher

Azure Storage ist ein wichtiger Bestandteil beim Bereitstellen und Verwalten von virtuellen Computern (VMs) und Anwendungen. Azure Storage bietet Dienste zum Speichern von Dateidaten, unstrukturierten Daten und Nachrichten und ist außerdem Teil der Infrastruktur zur Unterstützung virtueller Computer.

Es sind zwei Arten von Speicherkonten zur Unterstützung virtueller Computer verfügbar:

- Mit einem Standardspeicherkonto erhalten Sie Zugriff auf Blobspeicher (zum Speichern von Datenträgern in virtuellen Azure-Computern) sowie auf Tabellen-, Warteschlangen- und Dateispeicher.
- [Storage Premium](../storage/storage-premium-storage.md) bietet Datenträgerunterstützung mit hoher Leistung und niedriger Latenz für E/A-intensive Workloads wie SQL Server in einem AlwaysOn-Cluster und unterstützt derzeit nur Datenträger virtueller Azure-Computer.

Azure erstellt virtuelle Computer mit einem Betriebssystem-Datenträger, einem temporären Datenträger und keinen oder mehreren optionalen Datenträgern für Daten. Der Betriebssystem-Datenträger und Datenträger mit Daten sind Azure-Seitenblobs, während der temporäre Datenträger sich im lokalen Speicher des Knotens befindet, in dem auch der Computer gespeichert ist. Achten Sie beim Anwendungsentwurf darauf, diesen temporären Datenträger nur für nicht persistente Daten zu verwenden, da der virtuelle Computer bei Wartungen zwischen Hosts migriert werden kann. Alle auf dem temporären Datenträger gespeicherten Daten würden verloren gehen.

Dauerhaftigkeit und hohe Verfügbarkeit werden von der zugrunde liegenden Azure Storage-Umgebung bereitgestellt, um sicherzustellen, dass Ihre Daten vor einer nicht geplante Wartung oder Hardwarefehlern geschützt bleiben. Beim Entwerfen Ihrer Azure Storage-Umgebung können Sie festlegen, dass der Speicher virtueller Computer lokal in einem bestimmten Azure-Rechenzentrum, in mehreren Azure-Rechenzentren innerhalb einer bestimmten Region oder sogar in Azure-Rechenzentren in verschiedenen Regionen repliziert wird. Sie können sich [über Replikationsoptionen für hohe Verfügbarkeit informieren](../storage/storage-introduction.md#replication-for-durability-and-high-availability).

Betriebssystem-Datenträger und Datenträger mit Daten verfügen über eine maximale Größe von 1023 GB, da die maximale Größe eines Blobs 1024 GB beträgt, und darin müssen die Metadaten (Fußzeile) der VHD-Datei (ein GB hat 1024<sup>3</sup> Bytes) enthalten sein. Mit Speicherplätzen unter Windows Server 2012 können Sie diese Beschränkung umgehen, indem Sie Datenträger für Daten zu einem Pool zusammenfassen, um dem virtuellen Computer logische Volumes mit mehr als 1023 GB bereitzustellen.

Es gibt einige Skalierbarkeitsgrenzwerte beim Entwerfen der Azure Storage-Bereitstellung. Weitere Details finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](azure-subscription-service-limits.md#storage-limits). Informationen hierzu finden Sie auch unter [Skalierbarkeits- und Leistungsziele für Azure Storage](../storage/storage-scalability-targets.md).

Im Hinblick auf Anwendungsspeicher können Sie unstrukturierte Objektdaten wie z.B. Dokumente, Bilder, Backups, Konfigurationsdaten, Protokolle usw. mit Blobspeichern speichern. Ihre Anwendung schreibt dann nicht auf eine virtuelle Festplatte, die dem virtuellen Computer angefügt ist, sondern direkt in den Azure-Blobspeicher. Blobspeicher bietet je nach Verfügbarkeitsanforderungen und Budget auch die Option für [„heiße“ und „kalte“ Speicherebenen](../storage/storage-blob-storage-tiers.md).


## Stripesetdatenträger
Neben der Möglichkeit, Datenträger mit mehr als 1023 GB zu erstellen, wird durch Datenträgerstriping in vielen Fällen die Leistung verbessert, indem mehrere Blobs als Speicher für ein einzelnes Volume dienen können. Mit Striping läuft die erforderliche E/A zum Schreiben und Lesen von Daten aus einem einzigen logischen Datenträger parallel ab.

Azure erzwingt Grenzwerte für die Anzahl von Datenträgern und die verfügbare Bandbreite, die sich nach der Größe des virtuellen Computers richten. Ausführliche Informationen finden Sie unter [Größen für virtuelle Computer](virtual-machines-windows-sizes.md).

Wenn Sie Datenträgerstriping für Azure-Datenträger verwenden, beachten Sie die folgenden Richtlinien:

- Datenträger sollten immer die maximale Größe (1.023 GB) haben.
- Fügen Sie die maximal zulässige Anzahl von Datenträgern für die Größe des virtuellen Computers an.
- Speicherplätze verwenden
- Vermeiden Sie die Verwendung von Azure-Datenträger-Cachingoptionen (Cachingrichtlinie = Keine)

Weitere Informationen finden Sie unter [Storage Spaces – Designing for Performance](http://social.technet.microsoft.com/wiki/contents/articles/15200.storage-spaces-designing-for-performance.aspx) (in englischer Sprache).


## Mehrere Speicherkonten

Beim Entwerfen Ihrer Azure Storage-Umgebung können Sie mehrere Speicherkonten verwenden, wenn sich die Anzahl der bereitgestellten virtuellen Computer erhöht. Auf diese Weise werden die E/A auf die zugrunde liegende Azure Storage-Infrastruktur verteilt, um die optimale Leistung für Ihre virtuellen Computer und Anwendungen zu gewährleisten. Berücksichtigen Sie beim Entwerfen der Anwendungen, die bereitgestellt werden, die E/A-Anforderungen der einzelnen virtuellen Computer, und sorgen Sie für einen Ausgleich der virtuellen Computer über Azure-Speicherkonten. Vermeiden Sie das Gruppieren aller virtuellen Computer mit hohen E/A-Anforderungen in nur einem oder zwei Konten.

Weitere Informationen über die E/A-Funktionen von verschiedenen Azure Storage-Optionen und einige empfohlene Höchstwerte finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](../storage/storage-scalability-targets.md).


## Nächste Schritte

[AZURE.INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]

<!---HONumber=AcomDC_0706_2016-->