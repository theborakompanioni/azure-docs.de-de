---
title: "Storage Premium-Hochleistungsspeicher und Azure-VM-Datenträger | Microsoft-Dokumentation"
description: "Hier werden Storage Premium-Hochleistungsspeicher und nicht verwaltete und verwaltete Azure-VM-Datenträger beschrieben. Virtuelle Computer der Azure-DS-, DSv2- und -GS-Serie unterstützen Storage Premium."
services: storage
documentationcenter: 
author: ramankumarlive
manager: aungoo-msft
editor: tysonn
ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: ramankum
translationtype: Human Translation
ms.sourcegitcommit: 3a353bc874c1827f8a0fc85352894ad96cff16b5
ms.openlocfilehash: c9e43df37784999036c6cf250f27a808f79ebe2f


---
# <a name="high-performance-premium-storage-and-unmanaged-and-managed-azure-vm-disks"></a>Storage Premium-Hochleistungsspeicher und nicht verwaltete und verwaltete Azure-VM-Datenträger
Microsoft Azure Storage Premium bietet Datenträgerunterstützung mit hoher Leistung und geringer Wartezeit für virtuelle Computer (VMs), auf denen E/A-intensive Workloads ausgeführt werden. Für VM-Datenträger, die Storage Premium nutzen, werden Daten auf SSD-Laufwerken (Solid State Drives) gespeichert. Sie können die VM-Datenträger Ihrer Anwendung zu Azure Storage Premium migrieren, um von der Geschwindigkeit und Leistung dieser Laufwerke zu profitieren.

Ein virtueller Azure-Computer unterstützt das Anfügen mehrerer Storage Premium-Datenträger, damit Ihre Anwendung bis zu 64 TB Speicher pro virtuellem Computer nutzen kann. Mit Storage Premium können Sie für Ihre Anwendungen bis zu 80.000 IOPS (Input/Output Operations Per Second, E/A-Vorgänge pro Sekunde) pro VM und einen Datenträgerdurchsatz von bis zu 2.000 MB/s pro VM mit äußerst niedriger Wartezeit für Lesevorgänge erzielen.

Dank Storage Premium haben Sie mit Azure die Möglichkeit, anspruchsvollste Unternehmensanwendungen wie Dynamics AX, Dynamics CRM, Exchange Server, SharePoint-Farmen und SAP Business Suite per Lift & Shift in die Cloud zu migrieren. Sie können eine Vielzahl von leistungsintensiven Datenbankworkloads wie SQL Server, Oracle, MongoDB, MySQL und Redis ausführen, die eine konsistent hohe Leistung und eine geringe Wartezeit für Storage Premium benötigen.

> [!NOTE]
> Es wird empfohlen, alle Datenträger von virtuellen Computern, die eine hohe IOPS-Leistung erfordern, zu Storage Premium zu migrieren, um die beste Leistung für Ihre Anwendung zu erzielen. Wenn Ihr Datenträger keine hohe IOPS-Leistung erfordert, können Sie die Kosten beschränken, indem Sie sie im Standard-Speicher belassen. Dieser Speicher speichert Daten auf Datenträgern virtueller Computer auf Festplatten (Hard Disk Drives, HDDs) anstelle von SSDs.
> 

Es gibt zwei Möglichkeiten zum Erstellen von Premium-Datenträgern für Azure-VMs:

**Nicht verwaltete Datenträger:** Bei dieser ursprünglichen Methode verwalten Sie die Speicherkonten, die zum Speichern der VHD-Dateien für die VM-Datenträger verwendet werden. Die VHD-Dateien werden als Seitenblobs in Speicherkonten gespeichert. 

**[Azure Managed Disks](storage-managed-disks-overview.md)**: Dieses Feature nimmt Ihnen die Verwaltung der Speicherkonten ab, die für die VM-Datenträger verwendet werden. Sie geben die Art (Premium oder Standard) und die benötigte Größe des Datenträgers an, und Azure erstellt und verwaltet ihn für Sie. Sie müssen die Datenträger nicht selbst auf mehrere Speicherkonten verteilen, um sicherzustellen, dass Sie die Skalierbarkeitsgrenzwerte für die Speicherkonten einhalten. Dies erledigt Azure für Sie.

Grundsätzlich stehen Ihnen zwar beide Arten von Datenträgern zur Verfügung, aber wir empfehlen Ihnen die Verwendung von Managed Disks, da Sie bei dieser Variante von zahlreichen Features profitieren.

Informationen zu den ersten Schritten mit Azure Storage Premium finden Sie unter [Starten Sie kostenlos](https://azure.microsoft.com/pricing/free-trial/). 

Informationen zur Migration Ihrer vorhandenen VMs zu Storage Premium finden Sie unter [Convert a VM from unmanaged disks to managed disks](../virtual-machines/virtual-machines-windows-convert-unmanaged-to-managed-disks.md) (Durchführen der Konvertierung von nicht verwalteten zu verwalteten Datenträgern für eine VM) oder [How to convert a Linux VM from unmanaged disks to Azure Managed Disks](../virtual-machines/virtual-machines-linux-convert-unmanaged-to-managed-disks.md) (Durchführen der Konvertierung von nicht verwalteten Datenträgern zu Azure Managed Disks für eine Linux-VM).

> [!NOTE]
> Storage Premium wird derzeit in den meisten Regionen unterstützt. Eine Liste mit den verfügbaren Regionen finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/#services). Ermitteln Sie, für welche Regionen die VMs der einzelnen Größenserien (DS, DSV2, Fs und GS) unterstützt werden.
> 

## <a name="premium-storage-features"></a>Storage Premium-Funktionen

In diesem Abschnitt werden einige Features von Storage Premium beschrieben.

**Storage Premium-Datenträger**: Azure Storage Premium unterstützt VM-Datenträger, die an VMs einer bestimmten Größenserie angefügt werden können, z.B. DS, DSv2, GS und Fs. Sie können zwischen drei Datenträgergrößen wählen: P10 (128 GiB), P20 (512 GiB) und P30 (1024 GiB), jeweils mit eigenen Leistungsspezifikationen. Je nach Anwendungsanforderung können Sie einen oder mehrere dieser Datenträger an Ihre VM anfügen. Im folgenden Abschnitt [Skalierbarkeits- und Leistungsziele für Storage Premium ](#premium-storage-scalability-and-performance-targets) werden die Spezifikationen ausführlicher beschrieben.

**Premium-Seitenblob**: Storage Premium unterstützt Seitenblobs, die zum Speichern von dauerhaften nicht verwalteten Datenträgern für VMs verwendet werden. Im Gegensatz zum Standard-Speicher werden für Storage Premium keine Blockblobs, Anfügeblobs, Dateien, Tabellen oder Warteschlangen unterstützt.
Alle Objekte in einem Storage Premium-Konto sind Seitenblobs. Die Objekte werden an einer der unterstützten bereitgestellten Größen ausgerichtet. Aus diesem Grund eignet sich ein Storage Premium-Konto nicht zum Speichern sehr kleiner Blobs.

**Storage Premium-Konto**: Erstellen Sie für die Verwendung von Storage Premium ein Storage Premium-Konto für nicht verwaltete Datenträger. Wenn Sie lieber das [Azure-Portal](https://portal.azure.com) verwenden möchten, können Sie ein Storage Premium-Konto erstellen, indem Sie als Leistungsstufe „Premium“ und als Replikationsoption „Lokal redundanter Speicher (LRS)“ angeben. Sie können auch ein Storage Premium-Konto erstellen, indem Sie mithilfe der [Azure Storage-Rest-API](/rest/api/storageservices/fileservices/Azure-Storage-Services-REST-API-Reference) in der Version 2014-02-14 oder höher, der [REST-API der Dienstverwaltung](http://msdn.microsoft.com/library/azure/ee460799.aspx) in der Version 2014-10-01 oder höher (klassische Bereitstellungen), der [Referenz zur REST-API des Azure Storage-Ressourcenanbieters](/rest/api/storagerp) (Resource Manager-Bereitstellungen) und [Azure PowerShell](../powershell-install-configure.md) in der Version 0.8.10 oder höher den Typ „Premium_LRS“ angeben. Informationen zu den Begrenzungen von Storage Premium-Konten finden Sie im folgenden Abschnitt über [Skalierbarkeits- und Leistungsziele für Storage Premium](#premium-storage-scalability-and-performance-targets.md).

**Lokal redundanter Storage Premium**: Ein Storage Premium-Konto unterstützt nur den lokal redundanten Speicher (LRS) als Replikationsoption. Dies bedeutet, dass innerhalb einer Region drei Kopien der Daten beibehalten werden. Überlegungen zur Georeplikation bei der Verwendung von Storage Premium finden Sie in diesem Artikel im Abschnitt [Momentaufnahmen und Kopieren von Blobs](#snapshots-and-copy-blob) .

Von Azure wird das Speicherkonto als Container für Ihre nicht verwalteten Datenträger genutzt. Wenn Sie einen virtuellen Azure-Computer der DS-, DSv2-, GS- oder Fs-Serie mit nicht verwalteten Datenträgern erstellen und ein Storage Premium-Konto auswählen, werden Ihr Betriebssystem und die Datenträger in diesem Speicherkonto gespeichert.

## <a name="premium-storage-supported-vms"></a>VMs mit Storage Premium-Unterstützung
Storage Premium unterstützt virtuelle VMs der DS-, DSv2-, GS- und Fs-Serie. Mit diesen VMs können Sie Standard- und Storage Premium-Datenträger verwenden. Sie können keine Storage Premium-Datenträger für VM-Serien verwenden, die nicht mit Storage Premium kompatibel sind.

Weitere Informationen zu den verfügbaren Typen und Größen von virtuellen Azure-Computern für Windows-VMs finden Sie unter [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Informationen zu VM-Typen und Größen von virtuelle Linux-Computer finden Sie unter [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Im Folgenden sind einige Funktionen der virtuellen Computer der DS-, DSv2-, GS- und Fs-Serie aufgeführt:

**Clouddienst:**Virtuelle Computer der DS-Serie können einem Clouddienst hinzugefügt werden, der nur virtuelle Computer der DS-Serie umfasst. Fügen Sie virtuelle Computer der DS-Serie nicht einem vorhandenen Clouddienst mit virtuellen Computer hinzu, die nicht aus der DS-Serie stammen. Sie können vorhandene virtuelle Festplatten zu einem neuen Clouddienst migrieren, in dem nur virtuelle Computer der DS-Serie ausgeführt werden. Wenn Sie für den neuen Clouddienst die gleiche virtuelle IP-Adresse (VIP) beibehalten möchten, unter der die virtuellen Computer der DS-Serie gehostet werden, verwenden Sie [reservierte IP-Adressen](../virtual-network/virtual-networks-instance-level-public-ip.md). Virtuelle Computer der GS-Serie können einem vorhandenen Clouddienst hinzugefügt werden, in dem ausschließlich virtuelle Computer der GS-Serie ausgeführt werden.

**Betriebssystemdatenträger**: Die VMs, die unter Storage Premium ausgeführt werden können, können für die Verwendung von Betriebssystemdatenträgern vom Typ Premium oder Standard konfiguriert werden. Es wird empfohlen, Storage Premium-basierte Betriebssystemdatenträger zu verwenden, um optimale Ergebnisse zu erzielen.

**Datenträger**: Sie können auf einer VM, die unter Storage Premium ausgeführt wird, sowohl Premium- als auch Standard-Datenträger verwenden. Mit Storage Premium können Sie eine VM bereitstellen und mehrere permanente Datenträger an die VM anfügen. Bei Bedarf können Sie Daten über die Datenträger verteilen, um die Kapazität und die Leistung des Volumens zu erhöhen.

> [!NOTE]
> Wenn Sie Daten mithilfe von [Speicherplätzen](http://technet.microsoft.com/library/hh831739.aspx) über Storage Premium-Datenträger verteilen, sollten Sie für jeden verwendeten Datenträger eine Spalte konfigurieren. Andernfalls kann die Gesamtleistung des Stripesetvolume aufgrund ungleicher Verteilung des Datenverkehrs auf die Datenträger niedriger sein als erwartet. Standardmäßig können Sie auf der Server-Manager-Benutzeroberfläche Spalten für bis zu acht Datenträger einrichten. Wenn Sie mehr als acht Datenträger besitzen, müssen Sie PowerShell verwenden, um das Volume zu erstellen. Außerdem müssen Sie die Anzahl der Spalten manuell angeben. Andernfalls verwendet die Server-Manager-Benutzeroberfläche weiterhin 8 Spalten, auch wenn Sie mehr Datenträger besitzen. Wenn Sie beispielsweise 32 Datenträger in einem einzelnen Stripeset besitzen, sollten Sie 32 Spalten angeben. Sie können mit dem *NumberOfColumns*-Parameter des PowerShell-Cmdlets [New-VirtualDisk](http://technet.microsoft.com/library/hh848643.aspx) die Anzahl der vom virtuellen Datenträger verwendeten Spalten angeben. Weitere Informationen finden Sie unter [Übersicht über Speicherplätze](http://technet.microsoft.com/library/hh831739.aspx) und [Häufig gestellte Fragen zu Speicherplätzen](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).
> 

**Cache:** VMs der Größenserien, die Storage Premium unterstützen, verfügen über eine einzigartige Cachefunktion, mit der Sie eine hohe Durchsatzrate mit geringer Wartezeit erzielen können, die die zugrunde liegende Storage Premium-Datenträgerleistung übersteigt. Sie können die Datenträger-Cacherichtlinie auf den Storage Premium-Datenträgern als „ReadOnly“, „ReadWrite“ oder „None“ konfigurieren. Die standardmäßige Datenträger-Cacherichtlinie lautet „ReadOnly“ für alle Premium-Datenträger und „ReadWrite“ für Betriebssystemdatenträger. Verwenden Sie jeweils die richtige Konfigurationseinstellung, um die optimale Leistung für Ihre Anwendung zu erreichen. Legen Sie die Datenträger-Cacherichtlinie beispielsweise für Datenträger mit hohem Leseaufkommen oder schreibgeschützte Datenträger, z.B. SQL Server-Datendateien, auf „ReadOnly“ fest. Legen Sie die Datenträger-Cacherichtlinie bei Datenträgern mit hohem Schreibaufkommen oder vom Typ „Nur schreiben“, z.B. SQL Server-Protokolldateien, dagegen auf „None“ fest. Weitere Informationen zur Optimierung Ihres Entwurfs mit Storage Premium finden Sie unter [Azure Storage Premium: Entwurf für hohe Leistung](storage-premium-storage-performance.md).

**Analyse**: Um die Leistung virtueller Computer anhand von Datenträgern unter Storage Premium zu analysieren, können Sie im [Azure-Portal](https://portal.azure.com) die VM-Diagnose aktivieren. Informationen hierzu finden Sie unter [Microsoft Azure Virtual Machine Monitoring with Azure Diagnostics Extension](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/) (Microsoft Azure Virtual Machine-Überwachung mit der Azure-Diagnoseerweiterung). Die Datenträgerleistung können Sie mithilfe von auf dem Betriebssystem basierenden Tools analysieren, z.B. [Windows-Leistungsüberwachung](https://technet.microsoft.com/library/cc749249.aspx) für virtuelle Windows-Computer und [IOSTAT](http://linux.die.net/man/1/iostat) für virtuelle Linux-Computer.

**Skalierungsgrenzwerte und Leistung virtueller Computer**: Jede Größe für durch Storage Premium unterstützte virtuelle Computer verfügt über Skalierungsgrenzwerte und Leistungsspezifikationen für IOPS, Bandbreite und die Anzahl von Datenträgern, die pro virtuellem Computer angefügt werden können. Stellen Sie bei Verwendung von Storage Premium-Datenträgern mit virtuellen Computern sicher, dass auf dem virtuellen Computer ausreichend IOPS und Bandbreite für den Datenverkehr des Datenträgers verfügbar sind.
Bei einem virtuellen Computer vom Typ „STANDARD_DS1“ steht für Storage Premium-Datenträgerverkehr beispielsweise eine dedizierte Bandbreite von 32 MB/s zur Verfügung. Mit einem Storage Premium-Datenträger vom Typ P10 kann ein Bandbreite von 100 MB/s bereitgestellt werden. Mit einem an diesen virtuellen Computer angefügten Storage Premium-Datenträger vom Typ „P10“ sind maximal 32 MB/s möglich (nicht bis zu 100 MB/s, wie sie der P10-Datenträger normalerweise bereitstellen kann).

Derzeit ist der größte virtuelle Computer der DS-Serie der Standard_DS15_v2. Er kann bis zu 960 MB/s über alle Datenträger hinweg bereitstellen. Der größte virtuelle Computer der GS-Serie ist der Standard_GS5. Er kann bis zu 2.000 MB/s über alle Datenträger hinweg bereitstellen.
Beachten Sie, dass diese Grenzwerte nur für den Datenträgerverkehr und nicht für Cachetreffer- und Netzwerkdatenverkehr gelten. Für den Netzwerkdatenverkehr virtueller Computer steht eine separate Bandbreite zur Verfügung. Diese unterscheidet sich von der dedizierten Bandbreite für Storage Premium-Datenträger.

Aktuelle Informationen zu maximalen IOPS- und Durchsatzwerten (d.h. Bandbreitenwerte) für durch Storage Premium unterstützte virtuelle Computer finden Sie unter [Größen für virtuelle Computer in Azure (Windows)](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bzw. [Größen für virtuelle Computer in Azure (Linux)](../virtual-machines/virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Informationen zu Storage Premium-Datenträgern und ihren IOPS und Durchsatzgrenzwerten finden Sie in diesem Artikel in der Tabelle im Abschnitt [Skalierbarkeits- und Leistungsziele für Storage Premium](#premium-storage-scalability-and-performance-targets).

## <a name="premium-storage-scalability-and-performance-targets"></a>Skalierbarkeits- und Leistungsziele für Storage Premium
In diesem Abschnitt werden alle Skalierbarkeits- und Leistungsziele beschrieben, die Sie bei der Verwendung von Storage Premium berücksichtigen müssen.

Storage Premium-Konten weisen folgende Skalierbarkeitsziele auf:

| Gesamtkapazität des Kontos | Gesamtbandbreite für ein lokal redundantes Speicherkonto |
| --- | --- | 
| Kapazität des Datenträgers: 35 TB <br>Kapazität für Momentaufnahmen: 10 TB | Bis zu 50 GB pro Sekunde für eingehenden und ausgehenden Datenverkehr |

* "Eingehend" bezieht sich auf alle Daten (Anforderungen), die an ein Speicherkonto gesendet werden.
* "Ausgehend" bezieht sich auf alle Daten (Antworten), die von einem Speicherkonto empfangen werden.

Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungszielen für Azure Storage](storage-scalability-targets.md).

Wenn Sie Storage Premium-Konten für nicht verwaltete Datenträger verwenden und Ihre Anwendung die Skalierbarkeitsziele eines einzelnen Speicherkontos überschreiten, sollten Sie die Migration zu Managed Disks erwägen. Falls Sie die Migration zu Managed Disks nicht durchführen möchten, sollten Sie Ihre Anwendung für die Verwendung von mehreren Speicherkonten erstellen und die Daten über diese Speicherkonten hinweg partitionieren. Wenn Sie beispielsweise Datenträger mit einer Kapazität von 51 Terabyte (TB) an mehrere virtuelle Computer anfügen möchten, sollten Sie diese auf zwei Speicherkonten verteilen, da 35 TB der Grenzwert für ein einzelnes Storage Premium-Konto ist. Stellen Sie sicher, dass ein Storage Premium-Konto stets Datenträger mit einer Größe von maximal 35 TB enthält.

### <a name="premium-storage-disk-limits"></a>Grenzwerte für Storage Premium-Datenträgergrößen
Wenn Sie einen Storage Premium-Datenträger bereitstellen, wird anhand der Größe des Datenträgers der maximale IOPS- und Durchsatzwert (Bandbreite) ermittelt. Es sind drei Arten von Storage Premium-Datenträgern verfügbar: P10, P20 und P30. Jeder Typ weist bestimmte Grenzwerte für IOPS und den Durchsatz auf, wie in der folgenden Tabelle angegeben:

|Datenträgertyp des Premium-Speichers | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Datenträgergröße | 128 GB | 512 GB | 1.024 GiB (1 TB) |
| IOPS pro Datenträger | 500 | 2.300 | 5.000 |
Durchsatz pro Datenträger | 100 MB/s | 150 MB/s | 200 MB/s |

> [!NOTE]
> Stellen Sie sicher, dass auf Ihrem virtuellen Computer ausreichend Bandbreite für den Datenverkehr des Datenträgers verfügbar ist. Informationen hierzu finden Sie weiter oben in diesem Artikel im Abschnitt [Durch Storage Premium unterstützte virtuelle Computer](#premium-storage-supported-vms). Andernfalls werden der Datenträgerdurchsatz und IOPS auf niedrigere Werte beschränkt. Die Grundlage für diese Beschränkung bilden die Limits der virtuellen Computer und nicht die in der vorherigen Tabelle aufgeführten Datenträgerlimits.  
> 
> 

Es folgen einige wichtige Punkte, die Sie in Bezug auf die Skalierbarkeits- und Leistungsziele für Storage Premium wissen müssen:

* **Bereitgestellte Kapazität und Leistung**: Im Gegensatz zu einem Standard-Speicherdatenträger sind bei der Bereitstellung eines Storage Premium-Datenträgers die Kapazität, die IOPS und der Durchsatz für diesen Datenträger garantiert. Wenn Sie beispielsweise einen P30-Datenträger erstellen, werden in Azure eine Speicherkapazität von 1.024 GB, 5.000 IOPS und ein Durchsatz von 200 MB/s für diesen Datenträger bereitgestellt. Die Anwendung kann die Kapazität und Leistung ganz oder teilweise nutzen.

* **Datenträgergröße**: Azure ordnet die (aufgerundete) Datenträgergröße der nächsten Option für Storage Premium-Datenträger wie in der Tabelle angegeben zu. Ein Datenträger mit der Größe 100 GiB wird beispielsweise als Option „P10“ klassifiziert. Er kann bis zu 500 IOPS mit einem Durchsatz von bis zu 100 MB/s ausführen. Entsprechend ist ein Datenträger der Größe 400 GiB als P20 klassifiziert und kann bis zu 2.300 IOPS mit einem Durchsatz von 150 MB/s ausführen.
  
> [!NOTE]
> Sie können die Größe der vorhandenen Datenträger problemlos erhöhen. Beispielsweise können Sie die Größe für einen 30-GB-Datenträger auf 128 GB oder sogar 1 TB erhöhen. Sie haben auch die Möglichkeit, einen P20-Datenträger in einen P30-Datenträger umzuwandeln, wenn Sie mehr Kapazität oder mehr IOPS und einen höheren Durchsatz benötigen. 
> 
 
* **E/A-Größe:** Die Größe der Eingabe-/Ausgabeeinheit (E/A) liegt bei 256 KB. Wenn die Daten, die übertragenen werden, kleiner als 256 KB sind, werden sie als einzelne E/A-Einheit betrachtet. Die größeren E/A-Größen werden als mehrere Ein- bzw. Ausgaben der Größe 256 KB gezählt. Eine EA von 1.100 KB wird beispielsweise als fünf E/A-Einheiten gezählt.

* **Durchsatz:**Zu den Durchsatzlimits zählen Schreib- und Lesevorgänge auf dem Datenträger, die nicht vom Cache verarbeitet werden. Bei P10-Datenträgern liegt der Durchsatz beispielsweise bei 100 MB/s pro Datenträger. Einige Beispiele für den gültigen Durchsatz bei P10-Datenträgern:

| Max. Durchsatz pro P10-Datenträger | Nicht vom Cache verarbeitete Lesevorgänge vom Datenträger | Nicht vom Cache verarbeitete Schreibvorgänge vom Datenträger |
| --- | --- | --- |
| 100 MB pro Sekunde | 100 MB pro Sekunde | 0 |
| 100 MB pro Sekunde | 0 | 100 MB pro Sekunde |
| 100 MB pro Sekunde | 60 MB pro Sekunde | 40 MB pro Sekunde |

* **Cachetreffer**: Cachetreffer werden durch die zugeordneten IOPS-/Durchsatzwerte des Datenträgers nicht eingeschränkt. Wenn Sie beispielsweise einen Datenträger mit der Cacheeinstellung „ReadOnly“ auf einer von Storage Premium unterstützten VM verwenden, unterliegen Lesevorgänge, die aus dem Cache bereitgestellt werden, nicht den IOPS- und Durchsatzbeschränkungen des Datenträgers. Daher können Sie einen sehr hohen Durchsatz mit einem Datenträger erzielen, wenn der Workload vorwiegend aus Lesevorgängen besteht. Beachten Sie, dass für den Cache separate IOPS-/Durchsatzgrenzwerte auf Ebene des virtuellen Computers basierend auf der VM-Größe gelten. Virtuelle Computer der DS-Serie bieten etwa 4.000 IOPS und 33 MB pro Sekunde und Kern für E/A von Caches und lokalen SSDs. Virtuelle Computer der GS-Serie weisen eine Beschränkung von 5.000 IOPS und 50 MB pro Sekunde und Kern für E/A von Caches und lokalen SSDs auf. 

## <a name="throttling"></a>Drosselung
Unter Umständen bemerken Sie eine Drosselung, wenn für IOPS oder beim Durchsatz der Anwendung die zugewiesenen Grenzwerte für einen Storage Premium-Datenträger überschritten werden oder der gesamte Datenträgerverkehr auf allen Datenträgern des virtuellen Computers das Bandbreitenlimit des Datenträgers für den virtuellen Computer überschreitet. Um die Drosselung zu verhindern, sollten Sie die Anzahl der ausstehenden E/A-Anforderungen für Datenträger einschränken, und zwar basierend auf den Skalierbarkeits- und Leistungszielen für den bereitgestellten Datenträger und auf der für den virtuellen Computer verfügbaren Datenträgerbandbreite.  

Ihre Anwendung kann die geringste Latenz erzielen, wenn sie so entworfen wird, dass Einschränkungen vermieden werden. Wenn die Anzahl der ausstehenden E/A-Anforderungen für den Datenträger zu klein ist, kann Ihre Anwendung andererseits die maximalen IOPS- und Durchsatzebenen nicht nutzen, die für den Datenträger verfügbar sind.

Die folgenden Beispiele veranschaulichen, wie die Einschränkungsebenen berechnet werden. Alle Berechnungen basieren auf der E/A-Einheitengröße von 256 KB.

### <a name="example-1"></a>Beispiel 1:
Ihre Anwendung hat 495 E/A-Einheiten mit einer Größe von 16 KB in einer Sekunde auf einem Datenträger des Typs P10 ausgeführt. Diese werden als 495 E/A-Einheiten pro Sekunde (IOPS) berechnet. Wenn Sie eine E/A von 2 MB in der gleichen Sekunde versuchen, entspricht die Summe der E/A-Einheiten 495 + 8. Dies liegt daran, dass 2 MB E/A zu 2048 KB / 256 KB = 8 E/A-Einheiten führt, wenn die E/A-Einheitengröße 256 KB beträgt. Da die Summe von 495 + 8 die IOPS-Grenze von 500 für den Datenträger überschreitet, tritt eine Einschränkung auf.

### <a name="example-2"></a>Beispiel 2:
Ihre Anwendung hat 400 E/A-Einheiten mit einer Größe von 256 KB auf einem Datenträger des Typs P10 ausgeführt. Verwendete Gesamtbandbreite: (400 * 256) / 1024 = 100 MB/s. Das Durchsatzlimit eines P10-Datenträgers liegt bei 100 MB pro Sekunde. Wenn Ihre Anwendung versucht, weitere E/A-Vorgänge in dieser Sekunde auszuführen, wird sie eingeschränkt, weil der zugewiesene Grenzwert überschritten wird.

### <a name="example-3"></a>Beispiel 3:
Sie besitzen einen virtuellen DS4-Computer mit zwei angefügten P30-Datenträgern. Bei jedem P30-Datenträger ist ein Durchsatz von 200 MB Datendurchsatz pro Sekunde möglich. Ein virtueller DS4-Computer verfügt jedoch über eine Datenträgerbandbreitenkapazität von insgesamt 256 MB pro Sekunde. Daher können Sie auf diesem virtuellen DS4-Computer für die angeschlossenen Datenträger nicht gleichzeitig den maximalen Durchsatz erzielen. Um dieses Problem zu beheben, können Sie auf einem Datenträger Datenverkehr von 200 MB pro Sekunde und auf dem anderen Datenträger 56 MB pro Sekunde tolerieren. Übersteigt die Summe des Datenträgerverkehrs 256 MB pro Sekunde, wird der Datenträgerverkehr gedrosselt.

> [!NOTE]
> Wenn der Datenverkehr des Datenträgers vor allem aus kleinen E/A-Größen besteht, ist es sehr wahrscheinlich, dass Ihre Anwendung den IOPS-Grenzwert vor dem Durchsatzgrenzwert erreicht. Wenn der Datenverkehr des Datenträgers vor allem aus großen E/A-Größen besteht, ist es andererseits sehr wahrscheinlich, dass Ihre Anwendung den Durchsatzgrenzwert und nicht den IOPS-Grenzwert erreicht. Sie können die IOPS und die Durchsatzkapazität Ihrer Anwendung mithilfe optimaler E/A-Größen und auch durch das Einschränken der Anzahl der ausstehenden E/A-Anforderungen für den Datenträger maximieren.
> 

Informationen zum Entwerfen einer hohen Anwendungsleistung mit Storage Premium finden Sie im Artikel [Hohe Anwendungsleistung mit Storage Premium](storage-premium-storage-performance.md).

## <a name="snapshots-and-copy-blob"></a>Momentaufnahmen und Kopieren von Blobs

Für den Storage-Dienst ist die VHD-Datei ein Seitenblob. Sie können Momentaufnahmen von Seitenblobs erstellen und sie an einen anderen Ort kopieren (etwa in ein anderes Speicherkonto).

### <a name="unmanaged-disks"></a>Nicht verwaltete Datenträger

Für nicht verwaltete Premium-Datenträger können [inkrementelle Momentaufnahmen](storage-incremental-snapshots.md) erstellt werden. Die Vorgehensweise ist dabei die gleiche wie bei Momentaufnahmen für Standardspeicher. Da Storage Premium nur den lokal redundanten Speicher (LRS) als Replikatsoption unterstützt ist, wird empfohlen, Momentaufnahmen zu erstellen und diese dann in ein georedundantes Standardspeicherkonto zu kopieren. Weitere Informationen finden Sie unter [Redundanzoptionen für Azure Storage](storage-redundancy.md).

Falls ein Datenträger an einen virtuellen Computer angefügt ist, sind bestimmte API-Vorgänge für die Datenträger nicht zulässig. Sie können beispielsweise den Vorgang [Kopieren von Blob](/rest/api/storageservices/fileservices/Copy-Blob) für das Blob nicht durchführen, solange der Datenträger an den virtuellen Computer angeschlossen ist. Erstellen Sie stattdessen zunächst eine Momentaufnahme dieses Blobs, indem Sie die REST-API-Methode [Momentaufnahme-Blob](/rest/api/storageservices/fileservices/Snapshot-Blob) verwenden und anschließend den Vorgang [Kopieren von Blob](/rest/api/storageservices/fileservices/Copy-Blob) der Momentaufnahme durchführen, um den angeschlossenen Datenträger zu kopieren. Alternativ können Sie den Datenträger auch trennen und dann die erforderlichen Vorgänge durchführen.

Für Momentaufnahmen von Storage Premium-Blobs gelten folgende Grenzwerte:

| Storage Premium-Grenzwert | Wert |
| --- | --- |
| Max. Anzahl von Momentaufnahmen pro Blob | 100 |
| Speicherkontokapazität für Snapshots (betrifft nur Daten in Snapshots und nicht die Daten im Basisblob) | 10 TB |
| Min. Zeit zwischen aufeinanderfolgenden Momentaufnahmen | 10 Minuten |

Um georedundante Kopien Ihrer Momentaufnahmen aufzubewahren, können Sie Momentaufnahmen aus einem Premium-Speicherkonto mithilfe von "AzCopy" oder "Copy Blob" in ein georedundantes Standardspeicherkonto kopieren. Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md) und [Copy Blob](/rest/api/storageservices/fileservices/Copy-Blob).

Detaillierte Informationen zum Durchführen von REST-Vorgängen für Seitenblobs in Storage Premium-Konten finden Sie unter [Verwenden von Blob-Dienstvorgängen mit Azure Premium-Speicher](http://go.microsoft.com/fwlink/?LinkId=521969) .

### <a name="managed-disks"></a>Verwaltete Datenträger

Bei einer Momentaufnahme für einen verwalteten Datenträger handelt es sich um eine schreibgeschützte Kopie des verwalteten Datenträgers, die als verwalteter Standard-Datenträger gespeichert wird. Für Managed Disks werden derzeit keine [inkrementellen Momentaufnahmen](storage-incremental-snapshots.md) unterstützt, aber dies ist bereits geplant. Informationen dazu, wie Sie eine Momentaufnahme für einen verwalteten Datenträger erstellen, finden Sie unter [Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots (Windows)](../virtual-machines/virtual-machines-windows-snapshot-copy-managed-disk.md) (Erstellen einer Kopie einer als verwalteter Azure-Datenträger gespeicherten VHD mithilfe verwalteter Momentaufnahmen (Windows)) oder [Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots (Linux)](../virtual-machines/linux/virtual-machines-linux-snapshot-copy-managed-disk.md) (Erstellen einer Kopie einer als verwalteter Azure-Datenträger gespeicherten VHD mithilfe verwalteter Momentaufnahmen (Linux)).

Falls ein verwalteter Datenträger an einen virtuellen Computer angefügt ist, sind bestimmte API-Vorgänge für die Datenträger nicht zulässig. So können Sie beispielsweise keine Shared Access Signature (SAS) generieren, um einen Kopiervorgang auszuführen, während der Datenträger an einen virtuellen Computer angefügt ist. Erstellen Sie stattdessen zunächst eine Momentaufnahme des Datenträgers, und führen Sie dann den Kopiervorgang für die Momentaufnahme aus. Alternativ können Sie den Datenträger trennen und anschließend eine Shared Access Signature (SAS) generieren, um den Kopiervorgang auszuführen.


## <a name="using-linux-vms-with-premium-storage"></a>Verwenden von virtuellen Linux-Computern mit Premium-Speicher
Nachfolgend finden Sie wichtige Anweisungen zum Konfigurieren virtueller Linux-Computer für Storage Premium:

* Für alle Storage Premium-Datenträger mit der Cacheeinstellung „ReadOnly“ oder „None“ müssen Sie so genannte Sperren bei der Bereitstellung des Dateisystems deaktivieren, um die Skalierbarkeitsziele für Storage Premium zu erreichen. Sie benötigen keine Sperren für dieses Szenario, da die Schreibvorgänge auf Storage Premium-Datenträgern für diese Cacheeinstellungen beständig sind. Wenn die Schreibanforderung erfolgreich abgeschlossen wurde, wurden Daten in den permanenten Speicher geschrieben. Verwenden Sie je nach Dateisystem die folgenden Methoden zum Deaktivieren von Sperren:
  
* Wenn Sie **reiserFS**verwenden, deaktivieren Sie Sperren mithilfe der Bereitstellungsoption „barrier=none“ (verwenden Sie zum Aktivieren von Sperren „barrier=flush“).
* Wenn Sie **ext3/ext4**verwenden, deaktivieren Sie Sperren mithilfe der Bereitstellungsoption „barrier=0“ (verwenden Sie zum Aktivieren von Sperren „barrier=1“).
* Wenn Sie **XFS**verwenden, deaktivieren Sie Sperren mithilfe der Bereitstellungsoption „nobarrier“ (verwenden Sie zum Aktivieren von Sperren „barrier“).
* Bei Storage Premium-Datenträgern mit der Cacheeinstellung „ReadWrite“ müssen Sperren aktiviert werden, um die Beständigkeit von Schreibvorgängen zu gewährleisten.
* Damit die Volumebezeichnungen nach dem Neustart des virtuellen Computers beibehalten werden, müssen Sie „/etc/fstab“ mit den UUID-Verweisen auf die Datenträger aktualisieren. Weitere Informationen finden Sie unter [Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer](../virtual-machines/virtual-machines-linux-add-disk.md).

Nachfolgend sind die Linux-Distributionen aufgeführt, die für Premium-Speicher überprüft wurden. Es wird empfohlen, dass Sie Ihre virtuellen Computer auf mindestens eine dieser Versionen (oder eine höhere Version) aktualisieren, um eine bessere Leistung und Stabilität mit Premium-Speicher zu erzielen. Außerdem sind für einige Versionen die neuesten LIS (Linux-Integrationsdienste) v4.0 für Microsoft Azure erforderlich. Der Download und die Installation sind über folgenden Link möglich: Wir fügen der Liste mehr Images hinzu, wenn weitere Überprüfungen ausgeführt wurden. Beachten Sie, dass unsere Überprüfungen ergaben, dass die Leistung für diese Images variiert. Sie hängt auch von den Workloadmerkmalen und -einstellungen der Images ab. Verschiedene Images werden für verschiedene Arten von Workloads optimiert.

| Verteilung | Version | Unterstützter Kernel | Details |
| --- | --- | --- | --- |
| Ubuntu | 12.04 | 3.2.0-75.110+ | Ubuntu-12_04_5-LTS-amd64-server-20150119-en-us-30GB |
| Ubuntu | 14.04 | 3.13.0-44.73+ | Ubuntu-14_04_1-LTS-amd64-server-20150123-en-us-30GB |
| Debian | 7.x, 8.x | 3.16.7-ckt4-1+ | &nbsp; |
| SUSE | SLES 12| 3.12.36-38.1+| suse-sles-12-priority-v20150213 <br> suse-sles-12-v20150213 |
| SUSE | SLES 11 SP4 | 3.0.101-0.63.1+ | &nbsp; |
| CoreOS | 584.0.0+| 3.18.4+ | CoreOS 584.0.0 |
| CentOS | 6.5, 6.6, 6.7, 7.0 | &nbsp; | [LIS4 erforderlich](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Siehe Hinweis unten* |
| CentOS | 7.1+ | 3.10.0-229.1.2.el7+ | [LIS4 empfohlen](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Siehe Hinweis unten* |
| RHEL | 6.8+, 7.2+ | &nbsp; | &nbsp; |
| Oracle | 6.0+, 7.2+ | &nbsp; | UEK4 oder RHCK |
| Oracle | 7.0-7.1 | &nbsp; | UEK4 oder RHCK mit [LIS 4.1+](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |
| Oracle | 6.4-6.7 | &nbsp; | UEK4 oder RHCK mit [LIS 4.1+](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |


### <a name="lis-drivers-for-openlogic-centos"></a>LIS-Treiber für OpenLogic CentOS

Kunden mit virtuellen OpenLogic CentOS-Computern sollten den folgenden Befehl zum Installieren der neuesten Treiber ausführen:

```
sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
sudo yum install microsoft-hyper-v
```

Anschließend ist ein Neustart erforderlich, um die neuen Treiber zu aktivieren.

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Bei der Verwendung des Premium-Speichers gelten die folgenden Abrechnungserwägungen:

* Größe von Storage Premium-Datenträgern/-Blobs
* Storage Premium-Momentaufnahmen
* Ausgehende Datenübertragungen

**Größe von Storage Premium-Datenträgern/-Blobs**: Die Abrechnung für einen Storage Premium-Datenträger bzw. ein Storage Premium-Blob hängt von der bereitgestellten Größe des Datenträgers/Blobs ab. Azure ordnet die bereitgestellte (aufgerundete) Größe der nächsten Option für den Storage Premium-Datenträger zu (siehe Tabelle im Abschnitt [Skalierbarkeits- und Leistungsziele für Storage Premium](#premium-storage-scalability-and-performance-targets)). Jeder Datenträger wird einer der unterstützten bereitgestellten Größen zugeordnet und entsprechend berechnet. Die Abrechnung für bereitgestellte Datenträger erfolgt anteilig auf Stundenbasis unter Verwendung des monatlichen Preises für das Premium-Speicherangebot. Wenn Sie z.B. einen Datenträger des Typs P10 bereitgestellt und diesen nach 20 Stunden gelöscht haben, erfolgt die Abrechnung für das P10-Angebot anteilig für 20 Stunden. Dies ist unabhängig von der tatsächlichen Menge von Daten, die auf den Datenträger geschrieben wurden, oder dem verwendeten IOPS/Durchsatz.

**Momentaufnahmen für nicht verwaltete Premium-Datenträger**: Momentaufnahmen für nicht verwaltete Premium-Datenträger werden in Bezug auf die zusätzliche Kapazität berechnet, die von den Momentaufnahmen genutzt wird. Informationen zu Momentaufnahmen finden Sie unter [Erstellen einer Momentaufnahme eines Blobs](/rest/api/storageservices/fileservices/Snapshot-Blob).

**Momentaufnahmen für verwaltete Premium-Datenträger**: Bei einer Momentaufnahme für einen verwalteten Datenträger handelt es sich um eine schreibgeschützte Kopie des Datenträgers, die als verwalteter Standard-Datenträger gespeichert wird. Die Kosten für eine Momentaufnahme sind die gleichen wie für einen verwalteten Standard-Datenträger. Wenn Sie beispielsweise eine Momentaufnahme eines verwalteten Premium-Datenträgers mit 128 GB erstellen, entsprechen die Kosten für die Momentaufnahme den Kosten für einen verwalteten Standard-Datenträger mit 128 GB.  

**Ausgehende Datenübertragungen:**[Ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/data-transfers/) (Daten, die von den Azure-Rechenzentren ausgehen) verursachen Kosten bei der Bandbreitenverwendung.

Ausführliche Informationen zu den Preisen für Storage Premium, von Storage Premium unterstützte VMs und Managed Disks finden Sie unter:

* [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Preisgestaltung für virtuelle Computer](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Preise für Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="azure-backup-service-support"></a>Azure Backup-Dienst – Unterstützung 

Virtuelle Azure-Computer mit nicht verwalteten Datenträgern können mithilfe von Azure Backup gesichert werden. [Weitere Informationen](../backup/backup-azure-vms-first-look-arm.md)

Der Azure Backup-Dienst kann auch in Kombination mit Managed Disks verwendet werden, um einen Sicherungsauftrag mit zeitbasierten Sicherungen, unkomplizierter Wiederherstellung von virtuellen Computern und Aufbewahrungsrichtlinien für Sicherungen zu erstellen. Weitere Informationen hierzu finden Sie im Artikel zur [Verwendung des Azure Backup-Diensts für virtuelle Computer mit Managed Disks](../backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Azure Storage Premium finden Sie in den folgenden Artikeln.

### <a name="design-and-implement-with-azure-premium-storage"></a>Entwurf und Implementierung mit Azure Storage Premium
* [Hohe Anwendungsleistung mit Storage Premium](storage-premium-storage-performance.md)
* [Verwenden von Blob-Dienstvorgängen mit dem Azure Premium-Speicher](http://go.microsoft.com/fwlink/?LinkId=521969)

### <a name="operational-guidance"></a>Leitfaden
* [Migrieren zu Azure Premium-Speicher](storage-migration-to-premium-storage.md)

### <a name="blog-posts"></a>Blogbeiträge
* [Azure Premium Storage Generally Available (Azure Storage Premium allgemein verfügbar)](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/)
* [Announcing the GS-Series: Adding Premium Storage Support to the Largest VMs in the Public Cloud (Ankündigung der GS-Serie: Unterstützung von Storage Premium für die größten virtuellen Computer in der öffentlichen Cloud)](https://azure.microsoft.com/blog/azure-has-the-most-powerful-vms-in-the-public-cloud/)


<!--HONumber=Feb17_HO2-->


