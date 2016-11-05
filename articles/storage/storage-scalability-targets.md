---
title: Skalierbarkeits- und Leistungsziele für Azure Storage | Microsoft Docs
description: Informationen zu den Skalierbarkeits- und Leistungszielen für Azure Storage, einschließlich Kapazität Anforderungsrate sowie eingehende und ausgehende Bandbreite für Standard- und Premium-Speicherkonten. Machen Sie sich mit den Leistungszielen für Partitionen in den einzelnen Azure Storage-Diensten vertraut.
services: storage
documentationcenter: na
author: robinsh
manager: carmonm
editor: tysonn

ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 08/03/2016
ms.author: robinsh

---
# <a name="azure-storage-scalability-and-performance-targets"></a>Skalierbarkeits- und Leistungsziele für Azure Storage
## <a name="overview"></a>Übersicht
In diesem Thema werden die Skalierbarkeits- und Leistungsaspekte von Microsoft Azure Storage beschrieben. Eine Zusammenfassung anderer Einschränkungen von Azure, finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).

> [!NOTE]
> Alle Speicherkonten werden auf der neuen, flachen Netzwerktopologie ausgeführt und unterstützen die unten beschriebenen Skalierbarkeits- und Leistungsziele, unabhängig davon, wann sie erstellt wurden. Weitere Informationen zur flachen Netzwerkarchitektur von Azure Storage sowie zur Skalierbarkeit finden Sie unter [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)(in englischer Sprache).
> 
> [!IMPORTANT]
> Die hier aufgelisteten Ziele für Skalierbarkeit und Leistung sind hochgesteckte Ziele, die jedoch erreichbar sind. In jedem Fall hängen die von Ihrem Speicherkonto erzielte Anforderungsrate und Bandbreite von der Größe der gespeicherten Objekte, den verwendeten Zugriffsmustern und der Art der von der Anwendung ausgeführten Workload ab. Testen Sie unbedingt Ihren Dienst, um festzustellen, ob seine Leistung Ihren Anforderungen entspricht. Wenn möglich, vermeiden Sie plötzliche Lastspitzen bei der Datenverkehrsrate, und stellen Sie sicher, dass der Datenverkehr über alle Partitionen verteilt ist.
> 
> Wenn Ihre Anwendung die Grenze dessen erreicht, was eine Partition an Workload bewältigen kann, dann gibt Azure Storage den Fehlercode 503 (Server ausgelastet) oder den Fehlercode 500 (Zeitüberschreitung für Vorgang) zurück. In diesem Fall sollte die Anwendung eine exponentielle Backoffrichtlinie für Wiederholungen verwenden. Durch exponentielle Backoffs kann die Auslastung der Partition verringert werden, um die Datenverkehrsspitzen bei dieser Partition auszugleichen.
> 
> 

Wenn die Anforderungen Ihrer Anwendung die Skalierbarkeitsziele eines einzelnen Speicherkontos überschreiten, können Sie die Anwendung so erstellen, dass mehrere Speicherkonten verwendet werden, und die Datenobjekte in diesen Speicherkonten partitionieren. Informationen zu Volumenpreisen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/) .

## <a name="scalability-targets-for-blobs,-queues,-tables,-and-files"></a>Skalierbarkeitsziele für Blobs, Warteschlangen, Tabellen und Dateien
[!INCLUDE [azure-storage-limits](../../includes/azure-storage-limits.md)]

## <a name="scalability-targets-for-virtual-machine-disks"></a>Skalierbarkeitsziele für Festplatten virtueller Computer
[!INCLUDE [azure-storage-limits-vm-disks](../../includes/azure-storage-limits-vm-disks.md)]

Weitere Informationen finden Sie unter [Windows VM-Größen](../virtual-machines/virtual-machines-windows-sizes.md) oder [Linux VM-Größen](../virtual-machines/virtual-machines-linux-sizes.md).

### <a name="standard-storage-accounts"></a>Standardspeicherkonten
[!INCLUDE [azure-storage-limits-vm-disks-standard](../../includes/azure-storage-limits-vm-disks-standard.md)]

### <a name="premium-storage-accounts"></a>Storage Premium-Konten
[!INCLUDE [azure-storage-limits-vm-disks-premium](../../includes/azure-storage-limits-vm-disks-premium.md)]

## <a name="scalability-targets-for-azure-resource-manager"></a>Skalierbarkeitsziele für Azure-Ressourcen-Manager
[!INCLUDE [azure-storage-limits-azure-resource-manager](../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="partitions-in-azure-storage"></a>Partitionen in Azure Storage
Jedes Objekt, das Daten enthält, die in Azure Storage (Blobs, Nachrichten, Entitäten und Dateien) gespeichert werden, gehört zu einer Partition und wird durch einen Partitionsschlüssel identifiziert. Die Partition bestimmt, wie in Azure Storage der Lastenausgleich für Blobs, Nachrichten, Entitäten und Dateien auf Servern erfolgt, sodass die Datenverkehrsanforderungen dieser Objekte erfüllt werden. Der Partitionsschlüssel ist einmalig und wird verwendet, um ein Blob, eine Nachricht oder Entität zu finden.

In der Tabelle oben unter [Skalierbarkeitsziele für Standardspeicherkonten](#standard-storage-accounts) sind die Leistungsziele für eine einzelne Partition für jeden Dienst aufgeführt.

Partitionen wirken sich wie folgt auf den Lastenausgleich und die Skalierbarkeit der einzelnen Speicherdienste aus:

* **Blobs**: Der Partitionsschlüssel für ein Blob setzt sich aus dem Kontonamen, Containernamen und dem Blobnamen zusammen. Dies bedeutet, dass jedes Blob seine eigene Partition aufweisen kann, wenn die Last auf den Blob danach verlangt. Blobs können über mehrere Server verteilt werden, um den Zugriff darauf horizontal hochzuskalieren, aber ein einzelnes Blob kann nur von einem einzelnen Server bedient werden. Blobs können zwar logisch in Blob-Containern zusammengefasst werden, allerdings wirkt sich dies nicht auf die Partitionierung einer solchen Gruppierung aus.
* **Dateien**: Der Partitionsschlüssel für eine Datei ist der Kontoname plus der Name der Dateifreigabe. Dies bedeutet, dass alle Dateien in einer Dateifreigabe sich auch in einer einzelnen Partition befinden.
* **Nachrichten:** Der Partitionsschlüssel für eine Nachricht setzt sich aus dem Kontonamen und dem Warteschlangennamen zusammen. Damit werden alle Nachrichten in einer Warteschlange in eine einzelnen Partition gruppiert und von einem einzelnen Server bedient. Verschiedene Warteschlangen können von verschiedenen Servern verarbeitet werden, um einen Lastenausgleich für die gegebene Anzahl von Warteschlangen eines Speicherkonto durchzuführen .
* **Entitäten:** Der Partitionsschlüssel für eine Entität setzt sich aus dem Kontonamen, dem Tabellennamen und dem Partitionsschlüssel zusammen, wobei der Partitionsschlüssel der Wert der erforderlichen benutzerdefinierten **PartitionKey**-Eigenschaft der Entität ist. Alle Entitäten mit dem gleichen Partitionsschlüsselwert werden in derselben Partition gruppiert und von demselben Partitionsserver bedient. Dies ist ein wichtiger Punkt, der beim Entwurf von Anwendungen zu berücksichtigt werden muss. In der Anwendung sollten die Vorteile bezüglich der Skalierbarkeit, den die Verteilung von Entitäten auf mehrere Partitionen bietet, und die Vorteile bezüglich des Datenzugriffs, den die Gruppierung von Entitäten in einer einzelnen Partition bietet, gegeneinander abgewogen werden.  

Ein wichtiger Vorteil der Gruppierung mehrerer Entitäten einer Tabelle in einer einzelnen Partition besteht darin, dass es möglich ist, kleine Batchvorgänge in verschiedenen Entitäten in der gleichen Partition auszuführen, da sich eine Partition auf einem einzelnen Server befindet. Falls Sie daher Batchvorgänge für eine Gruppe von Entitäten ausführen möchten, empfiehlt es sich daher, sie mit dem gleichen Partitionsschlüssel zu gruppieren. 

Andererseits kann für Entitäten, die in derselben Tabelle enthalten sind, jedoch verschiedenen Partitionierungsschlüssel haben, ein Lastausgleich auf verschiedenen Servern vorgenommen werden, sodass sich eine höherer Skalierbarkeit ergibt.

Detailierte Empfehlungen für das Entwerfen einer Partitionierungsstrategie für Tabellen finden Sie [hier](https://msdn.microsoft.com/library/azure/hh508997.aspx).

## <a name="see-also"></a>Weitere Informationen
* [Speicher – Preisdetails](https://azure.microsoft.com/pricing/details/storage/)
* [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md)
* [Premium-Speicher: Hochleistungsspeicher für Workloads in Azure Virtual Machine](storage-premium-storage.md)
* [Azure Storage-Replikation](storage-redundancy.md)
* [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](storage-performance-checklist.md)
* [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency (in englischer Sprache)](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

<!--HONumber=Oct16_HO2-->


