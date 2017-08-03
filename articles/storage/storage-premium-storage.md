---
title: "Storage Premium-Hochleistungsspeicher und verwaltete Azure-Datenträger für VMs | Microsoft-Dokumentation"
description: "Enthält Informationen zu Storage Premium-Hochleistungsspeicher und verwalteten Datenträgern für Azure-VMs. Virtuelle Computer der DS-, DSv2-, GS- und Fs-Serie von Azure unterstützen Storage Premium."
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
ms.date: 06/27/2017
ms.author: ramankum
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: 94ae2fa4600c007725ae2615554144d12306e833
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="high-performance-premium-storage-and-managed-disks-for-vms"></a>Storage Premium-Hochleistungsspeicher und verwaltete Datenträger für VMs
Azure Storage Premium bietet Datenträgerunterstützung mit hoher Leistung und geringer Wartezeit für virtuelle Computer (VMs) mit E/A-intensiven Workloads. Für VM-Datenträger, die Storage Premium nutzen, werden Daten auf SSD-Laufwerken (Solid-State Drives) gespeichert. Um die Geschwindigkeit und Leistung von Storage Premium-Datenträgern zu nutzen, können Sie vorhandene VM-Datenträger zu Storage Premium migrieren.

In Azure können Sie an eine VM mehrere Storage Premium-Datenträger anfügen. Durch die Nutzung mehrerer Datenträger erhalten Ihre Anwendungen bis zu 256 TB Speicher pro VM. Mit Storage Premium können Ihre Anwendungen 80.000 E/A-Vorgänge pro Sekunde (IOPS) pro VM und einen Datenträgerdurchsatz von bis zu 2.000 MB pro Sekunde (MB/s) pro VM erzielen. Lesevorgänge sind mit sehr kurzen Wartezeiten verbunden.

Dank Storage Premium bietet Azure die Möglichkeit, anspruchsvolle Unternehmensanwendungen wie Dynamics AX, Dynamics CRM, Exchange Server, SAP Business Suite und SharePoint-Farmen per Lift & Shift in die Cloud zu migrieren. Sie können leistungsintensive Datenbankworkloads in Anwendungen wie SQL Server, Oracle, MongoDB, MySQL und Redis ausführen, die eine konsistent hohe Leistung und eine geringe Wartezeit benötigen.

> [!NOTE]
> Zum Erzielen einer optimalen Leistung Ihrer Anwendung empfehlen wir, alle VM-Datenträger, die eine hohe IOPS-Leistung erfordern, zu Storage Premium zu migrieren. Wenn der Datenträger keinen hohen IOPS-Wert erfordert, können Sie zur Kostenreduzierung beitragen, indem Sie dafür Azure Storage-Standardspeicher nutzen. Im Standardspeicher werden VM-Datenträger nicht auf SSDs gespeichert, sondern auf Festplatten (HDDs).
> 

In Azure haben Sie zwei Möglichkeiten, um Storage Premium-Datenträger für VMs zu erstellen:

* **Nicht verwaltete Datenträger**

    Die ursprüngliche Methode besteht darin, nicht verwaltete Datenträger zu verwenden. Bei einem nicht verwalteten Datenträger verwalten Sie die Speicherkonten, die Sie zum Speichern von Dateien der virtuellen Festplatte (VHD) für Ihre VM-Datenträger verwenden. Die VHD-Dateien werden als Seitenblobs in Azure-Speicherkonten gespeichert. 

* **Verwaltete Datenträger**

    Bei Auswahl von [Azure Managed Disks](storage-managed-disks-overview.md) übernimmt Azure die Verwaltung der Speicherkonten, die Sie für Ihre VM-Datenträger verwenden. Sie geben den Datenträgertyp (Premium oder Standard) und die benötigte Datenträgergröße an. Azure erstellt und verwaltet den Datenträger für Sie. Sie müssen sich nicht darum kümmern, die Datenträger in mehreren Speicherkonten anzuordnen, um sicherzustellen, dass für Ihre Speicherkonten die Grenzen der Skalierbarkeit eingehalten werden. Dies wird von Azure für Sie durchgeführt.

Wir empfehlen Ihnen, verwaltete Datenträger zu wählen, um in den Genuss der vielen nützlichen Features zu kommen.

[Erstellen Sie als Erstes Ihr kostenloses Azure-Konto](https://azure.microsoft.com/pricing/free-trial/), um mit der Nutzung von Storage Premium zu beginnen. 

Informationen zur Migration Ihrer vorhandenen VMs zu Storage Premium finden Sie unter [Konvertieren eines virtuellen Computers von nicht verwalteten Datenträgern in verwaltete Datenträger](../virtual-machines/virtual-machines-windows-convert-unmanaged-to-managed-disks.md) und [Konvertieren einer Linux-VM von nicht verwalteten Datenträgern zu Azure Managed Disks](../virtual-machines/linux/convert-unmanaged-to-managed-disks.md).

> [!NOTE]
> Storage Premium ist in den meisten Regionen verfügbar. Eine Liste mit den verfügbaren Regionen finden Sie unter [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services). Suchen Sie nach den Regionen, in denen VM-Größen für Storage Premium (DS-Serie, DSV2-Serie, GS-Serie und Fs-Serie) unterstützt werden.
> 

## <a name="features"></a>Features

Hier sind einige Features von Storage Premium aufgeführt:

* **Storage Premium-Datenträger**

    Storage Premium unterstützt VM-Datenträger, die an VMs einer bestimmten Größenserie angefügt werden können. Storage Premium unterstützt virtuelle Computer der DS-, DSv2-, GS- und FS-Serie. Sie haben die Wahl zwischen sieben Datenträgergrößen: P4 (32 GB), P6 (64 GB), P10 (128 GB), P20 (512 GB), P30 (1.024 GB), P40 (2.048 GB) und P50 (4.095 GB). Die Datenträgergrößen P4 und P6 werden aktuell nur für verwaltete Datenträger unterstützt. Für jede Datenträgergröße gelten eigene Leistungsspezifikationen. Je nach Anwendungsanforderung können Sie einen oder mehrere Datenträger an Ihre VM anfügen. Unter [Skalierbarkeits- und Leistungsziele für Storage Premium](#scalability-and-performance-targets) werden die Spezifikationen ausführlicher beschrieben.

* **Premium-Seitenblobs**

    Storage Premium unterstützt Seitenblobs. Verwenden Sie Seitenblobs, um persistente, nicht verwaltete Datenträger für VMs in Storage Premium zu speichern. Im Gegensatz zum Azure Storage-Standardspeicher werden für Storage Premium keine Blockblobs, Anfügeblobs, Dateien, Tabellen oder Warteschlangen unterstützt. Premium-Seitenblob unterstützt sechs Größen von P10 bis P50 und P60 (8.191 GiB). Das Anfügen eines Premium-Seitenblobs mit der Größe P60 an VM-Datenträger wird nicht unterstützt. 

    Alle Objekte, die in einem Storage Premium-Konto angeordnet werden, werden zu einem Seitenblob. Das Seitenblob nimmt automatisch eine der unterstützten bereitgestellten Größen an. Dies ist der Grund dafür, warum ein Storage Premium-Konto nicht zum Speichern von sehr kleinen Blobs gedacht ist.

* **Premium-Speicherkonto**

    Erstellen Sie für die Verwendung von Storage Premium ein Premium-Speicherkonto für nicht verwaltete Datenträger. Wählen Sie im [Azure-Portal](https://portal.azure.com) zum Erstellen eines Premium-Speicherkontos die Leistungsstufe **Premium**. Wählen Sie die Replikationsoption **Lokal redundanter Speicher (LRS)**. Sie können ein Premium-Speicherkonto auch erstellen, indem Sie den Typ an einem der folgenden Orte auf **Premium_LRS** festlegen:
    * [Speicher-REST-API](https://docs.microsoft.com/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference) (Version 2014-02-14 oder höher)
    * [Dienstverwaltungs-REST-API](http://msdn.microsoft.com/library/azure/ee460799.aspx) (Version 2014-10-01 oder höher, für klassische Azure-Bereitstellungen)
    * [Azure Storage-Ressourcenanbieter-REST-API](https://docs.microsoft.com/rest/api/storagerp) (für Azure Resource Manager-Bereitstellungen)
    * [Azure PowerShell](../powershell-install-configure.md) (Version 0.8.10 oder höher)

    Informationen zu den Grenzen von Premium-Speicherkonten finden Sie unter [Skalierbarkeits- und Leistungsziele für Storage Premium](#premium-storage-scalability-and-performance-targets).

* **Lokal redundanter Premium-Speicher**

    Ein Premium-Speicherkonto unterstützt nur lokal redundanten Speicher als Replikationsoption. Beim lokal redundanten Speicher werden drei Kopien der Daten in derselben Region aufbewahrt. Für die regionale Notfallwiederherstellung müssen Sie Ihre VM-Datenträger mit [Azure Backup](../backup/backup-introduction-to-azure-backup.md) in einer anderen Region sichern. Außerdem ist es erforderlich, ein GRS-Konto (georedundanter Speicher) als Sicherungstresor zu verwenden. 

    Von Azure wird Ihr Speicherkonto als Container für Ihre nicht verwalteten Datenträger genutzt. Wenn Sie einen virtuellen Azure-Computer der DS-, DSv2-, GS- oder Fs-Serie von Azure mit nicht verwalteten Datenträgern erstellen und ein Premium-Speicherkonto auswählen, werden Ihr Betriebssystem und die Datenträger in diesem Speicherkonto gespeichert.

## <a name="supported-vms"></a>Unterstützte VMs
Storage Premium unterstützt virtuelle Computer der DS-, DSv2-, GS- und FS-Serie. Mit diesen VM-Typen können Sie Standard- und Premium-Speicherdatenträger verwenden. Sie können keine Premium-Speicherdatenträger für VM-Serien verwenden, die nicht mit Storage Premium kompatibel sind.

Informationen zu VM-Typen und -Größen in Azure für Windows finden Sie unter [Windows-VM-Größen](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Informationen zu VM-Typen und -Größen in Azure für Linux finden Sie unter [Linux-VM-Größen](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Hier sind einige Funktionen von VMs der DS-, DSv2-, GS- und Fs-Serie:

* **Clouddienst**

    Sie können VMs der DS-Serie einem Clouddienst hinzufügen, der nur über VMs der DS-Serie verfügt. Fügen Sie VMs der DS-Serie nicht einem vorhandenen Clouddienst mit virtuellen Computern hinzu, die nicht aus der DS-Serie stammen. Sie können vorhandene virtuelle Festplatten zu einem neuen Clouddienst migrieren, in dem nur virtuelle Computer der DS-Serie ausgeführt werden. Wenn Sie für den neuen Clouddienst, unter dem die virtuellen Computer der DS-Serie gehostet werden, die gleiche virtuelle IP-Adresse (VIP) nutzen möchten, verwenden Sie [reservierte IP-Adressen](../virtual-network/virtual-networks-instance-level-public-ip.md). Virtuelle Computer der GS-Serie können einem vorhandenen Clouddienst hinzugefügt werden, der ausschließlich über virtuelle Computer der GS-Serie verfügt.

* **Betriebssystemdatenträger**

    Sie können Ihre Premium Storage-VM so einrichten, dass entweder ein Premium- oder ein Standard-Betriebssystemdatenträger verwendet wird. Das beste Ergebnis erzielen Sie, wenn Sie einen Betriebssystemdatenträger verwenden, der auf Storage Premium basiert.

* **Datenträger**

    Sie können Premium- und Standard-Datenträger auf derselben Storage Premium-VM verwenden. Mit Storage Premium können Sie eine VM bereitstellen und mehrere permanente Datenträger an die VM anfügen. Bei Bedarf können Sie Daten über die Datenträger verteilen, um die Kapazität und die Leistung des Volumes zu erhöhen.

    > [!NOTE]
    > Wenn Sie [Speicherplätze](http://technet.microsoft.com/library/hh831739.aspx) für die Verteilung auf Premium-Speicherdatenträger nutzen, sollten Sie Speicherplätze mit einer Spalte für jeden verwendeten Datenträger einrichten. Andernfalls kann die Gesamtleistung des Stripesetvolume aufgrund ungleicher Verteilung des Datenverkehrs auf die Datenträger niedriger als erwartet sein. In Server-Manager können Sie standardmäßig Spalten für bis zu acht Datenträger einrichten. Wenn Sie mehr als acht Datenträger haben, können Sie PowerShell nutzen, um das Volume zu erstellen. Geben Sie die Anzahl von Spalten manuell an. Andernfalls verwendet die Server-Manager-Benutzeroberfläche weiterhin acht Spalten, auch wenn Sie mehr Datenträger besitzen. Wenn bei Ihnen beispielsweise 32 Datenträger in einem einzelnen Stripeset enthalten sind, geben Sie 32 Spalten an. Verwenden Sie zum Angeben der Anzahl von Spalten, die vom virtuellen Datenträger verwendet werden, im [New-VirtualDisk](http://technet.microsoft.com/library/hh848643.aspx)-PowerShell-Cmdlet den Parameter *NumberOfColumns*. Weitere Informationen finden Sie unter [Übersicht über Speicherplätze](http://technet.microsoft.com/library/hh831739.aspx) und [Häufig gestellte Fragen zu Speicherplätzen](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).
    >
    > 

* **Cache**

    VMs der Größenserien, die Storage Premium unterstützen, verfügen über eine einzigartige Cachefunktion für hohen Durchsatz und längere Wartezeiten. Die Leistung der Cachefunktion ist höher als die Leistung des zugrunde liegenden Premium-Speicherdatenträgers. Sie können die Datenträger-Cacherichtlinie auf den Storage Premium-Datenträgern auf **ReadOnly**, **ReadWrite** oder **None** festlegen. Die standardmäßige Datenträger-Cacherichtlinie lautet **ReadOnly** für alle Premium-Datenträger und **ReadWrite** für Betriebssystemdatenträger. Verwenden Sie die richtige Cacheeinstellung, um für Ihre Anwendung eine optimale Leistung zu erzielen. Legen Sie die Datenträger-Cacherichtlinie beispielsweise für Datenträger mit hohem Leseaufkommen oder schreibgeschützte Datenträger, z.B. SQL Server-Datendateien, auf **ReadOnly** fest. Legen Sie die Datenträger-Cacherichtlinie bei Datenträgern mit hohem Schreibaufkommen oder vom Typ „Nur schreiben“, z.B. SQL Server-Protokolldateien, dagegen auf **None** fest. Weitere Informationen zur Optimierung Ihres Entwurfs mit Storage Premium finden Sie unter [Hohe Anwendungsleistung mit Storage Premium](storage-premium-storage-performance.md).

* **Analyse**

    Aktivieren Sie die VM-Diagnose im [Azure-Portal](https://portal.azure.com), um die VM-Leistung mit Datenträgern in Storage Premium zu analysieren. Weitere Informationen finden Sie unter [Microsoft Azure Virtual Machine Monitoring with Azure Diagnostics Extension](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/) (Überwachung von virtuellen Microsoft Azure-Computern mit der Azure-Diagnoseerweiterung). 

    Die Datenträgerleistung können Sie mithilfe von auf dem Betriebssystem basierenden Tools analysieren, z.B. der [Windows-Leistungsüberwachung](https://technet.microsoft.com/library/cc749249.aspx) für virtuelle Windows-Computer und dem Befehl [iostat](http://linux.die.net/man/1/iostat) für virtuelle Linux-Computer.

* **Skalierungslimits und Leistung virtueller Computer**

    Jede Größe von virtuellen Computern, die von Storage Premium unterstützt wird, verfügt über Skalierungsgrenzwerte und Leistungsspezifikationen für IOPS, Bandbreite und die Anzahl von Datenträgern, die pro virtuellem Computer angefügt werden können. Stellen Sie bei Verwendung von Storage Premium-Datenträgern mit virtuellen Computern sicher, dass auf dem virtuellen Computer ein ausreichend hoher IOPS-Wert und genügend Bandbreite für den Datenverkehr des Datenträgers vorhanden sind.

    Bei einem virtuellen Computer vom Typ „STANDARD_DS1“ steht für Storage Premium-Datenträgerverkehr beispielsweise eine dedizierte Bandbreite von 32 MB/s zur Verfügung. Mit einem Storage Premium-Datenträger vom Typ P10 kann eine Bandbreite von 100 MB/s bereitgestellt werden. Wenn ein Storage Premium-Datenträger vom Typ P10 an diese VM angefügt wird, sind nur bis zu 32 MB/s möglich. Der Höchstwert von 100 MB/s, die von einem P10-Datenträger bereitgestellt werden können, kann nicht verwendet werden.

    Derzeit ist „Standard_DS15_v2“ die größte VM der DS-Serie. Für „Standard_DS15_v2“ können datenträgerübergreifend bis zu 960 MB/s bereitgestellt werden. „Standard_GS5“ ist die größte VM der GS-Serie. Für „Standard_GS5“ können datenträgerübergreifend bis zu 2.000 MB/s bereitgestellt werden.

    Beachten Sie, dass diese Grenzwerte nur für Datenverkehr von Datenträgern gelten. In die Grenzwerte sind keine Cachetreffer und kein Netzwerkdatenverkehr eingerechnet. Für VM-Netzwerkdatenverkehr ist eine separate Bandbreite verfügbar. Die Bandbreite für den Netzwerkdatenverkehr unterscheidet sich von der dedizierten Bandbreite, die von Storage Premium-Datenträgern verwendet wird.

    Aktuelle Informationen zu maximalen IOPS- und Durchsatzwerten (d.h. Bandbreitenwerte) für durch Storage Premium unterstützte virtuelle Computer finden Sie unter [Größen für virtuelle Computer in Azure (Windows)](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bzw. [Größen für virtuelle Computer in Azure (Linux)](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

    Weitere Informationen zu Storage Premium-Datenträgern und den geltenden Grenzwerten für IOPS und Durchsatz finden Sie in der Tabelle im nächsten Abschnitt.

## <a name="scalability-and-performance-targets"></a>Skalierbarkeits- und Leistungsziele
In diesem Abschnitt werden die Skalierbarkeits- und Leistungsziele beschrieben, die bei der Nutzung von Storage Premium berücksichtigt werden sollten.

Storage Premium-Konten weisen folgende Skalierbarkeitsziele auf:

| Gesamtkapazität des Kontos | Gesamtbandbreite für ein lokal redundantes Speicherkonto |
| --- | --- | 
| Kapazität des Datenträgers: 35 TB <br>Kapazität für Momentaufnahmen: 10 TB | Bis zu 50 Gigabit pro Sekunde für eingehenden<sup>1</sup> und ausgehenden<sup>2</sup> Datenverkehr |

<sup>1</sup> Alle Daten (Anforderungen), die an ein Speicherkonto gesendet werden

<sup>2</sup> Alle Daten (Antworten), die von einem Speicherkonto empfangen werden

Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](storage-scalability-targets.md).

Wenn Sie Storage Premium-Konten für nicht verwaltete Datenträger verwenden und Ihre Anwendung die Skalierbarkeitsziele eines einzelnen Speicherkontos überschreitet, sollten Sie die Migration zu verwalteten Datenträgern erwägen. Falls die Migration zu verwalteten Datenträgern für Sie keine Option ist, können Sie Ihre Anwendung für die Nutzung mehrerer Speicherkonten erstellen. Partitionieren Sie Ihre Daten dann basierend auf diesen Speicherkonten. Wenn Sie beispielsweise 51-TB-Datenträger an mehrere VMs anfügen möchten, können Sie sie auf zwei Speicherkonten verteilen. 35 TB ist der Grenzwert pro Storage Premium-Konto. Stellen Sie sicher, dass ein Storage Premium-Konto stets Datenträger mit einer Größe von maximal 35 TB enthält.

### <a name="premium-storage-disk-limits"></a>Grenzwerte für Storage Premium-Datenträger
Wenn Sie einen Storage Premium-Datenträger bereitstellen, wird anhand der Größe des Datenträgers der maximale IOPS- und Durchsatzwert (Bandbreite) ermittelt. Azure bietet sieben Typen von Storage Premium-Datenträgern: P4 (nur Managed Disks), P6 (nur Managed Disks), P10, P20, P30, P40 und P50. Für jeden Typ von Storage Premium-Datenträger gelten für IOPS und den Durchsatz bestimmte Grenzwerte. Die Grenzwerte für die Datenträgertypen sind in der folgenden Tabelle beschrieben:

| Premium-Datenträgertyp  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Datenträgergröße           | 32 GB| 64 GB| 128 GB| 512 GB            | 1024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS pro Datenträger       | 120   | 240   | 500   | 2.300              | 5.000              | 7.500              | 7.500              | 
| Durchsatz pro Datenträger | 25 MB pro Sekunde  | 50 MB pro Sekunde  | 100 MB pro Sekunde | 150 MB pro Sekunde | 200 MB pro Sekunde | 250 MB pro Sekunde | 250 MB pro Sekunde | 

> [!NOTE]
> Sorgen Sie dafür, dass auf Ihrer VM ausreichend Bandbreite für den Datenträger-Datenverkehr verfügbar ist. Dies ist unter [VMs mit Storage Premium-Unterstützung](#premium-storage-supported-vms) beschrieben. Andernfalls sind Ihr Datenträgerdurchsatz und die IOPS-Menge auf niedrigere Werte beschränkt. Der Höchstmengen für Durchsatz und IOPS basieren auf den VM-Grenzwerten, und nicht auf den Grenzwerten für Datenträger, die in der vorherigen Tabelle beschrieben sind.  
> 
> 

Es folgen einige wichtige Punkte, die Sie in Bezug auf die Skalierbarkeits- und Leistungsziele für Storage Premium kennen sollten:

* **Bereitgestellte Kapazität und Leistung**

    Im Gegensatz zu einem Standard-Speicherdatenträger sind bei der Bereitstellung eines Storage Premium-Datenträgers die Kapazität, die IOPS und der Durchsatz dieses Datenträgers garantiert. Wenn Sie beispielsweise einen P50-Datenträger erstellen, werden in Azure eine Speicherkapazität von 4.095 GB, 7.500 IOPS und ein Durchsatz von 250 MB/s für diesen Datenträger bereitgestellt. Die Anwendung kann die Kapazität und Leistung ganz oder teilweise nutzen.

* **Datenträgergröße**

    Azure ordnet die (aufgerundete) Datenträgergröße der nächsten Option für Storage Premium-Datenträger zu, wie in der Tabelle im vorherigen Abschnitt angegeben. Eine Datenträgergröße von 100 GB ist beispielsweise als P10-Option klassifiziert. Hierbei sind bis zu 500 IOPS und ein Durchsatz von maximal 100 MB/s möglich. Eine Datenträgergröße von 400 GB wird als P20 klassifiziert. Hierbei sind bis zu 2.300 IOPS und ein Durchsatz von maximal 150 MB/s möglich.
    
    > [!NOTE]
    > Sie können die Größe der vorhandenen Datenträger problemlos erhöhen. Beispielsweise können Sie die Größe für einen 30-GB-Datenträger auf 128 GB oder sogar auf 1 TB erhöhen. Sie haben auch die Möglichkeit, einen P20-Datenträger in einen P30-Datenträger umzuwandeln, wenn Sie mehr Kapazität oder mehr IOPS und einen höheren Durchsatz benötigen. 
    > 
 
* **E/A-Größe**

    Die Größe einer E/A-Einheit beträgt 256 KB. Wenn die Daten, die übertragen werden, kleiner als 256 KB sind, werden sie als eine E/A-Einheit angesehen. Höhere E/A-Größen werden als mehrere Ein- bzw. Ausgaben der Größe 256 KB gezählt. Ein E/A-Vorgang mit 1.100 KB wird beispielsweise als fünf E/A-Einheiten gezählt.

* **Durchsatz**

    Der Grenzwert für den Durchsatz enthält Schreibvorgänge auf den Datenträger und Datenträger-Lesevorgänge, die nicht über den Cache bereitgestellt werden. Bei P10-Datenträgern liegt der Durchsatz beispielsweise bei 100 MB/s pro Datenträger. Die folgende Tabelle enthält einige Beispiele für den gültigen Durchsatz eines P10-Datenträgers:

    | Max. Durchsatz pro P10-Datenträger | Nicht vom Cache verarbeitete Lesevorgänge vom Datenträger | Nicht vom Cache verarbeitete Schreibvorgänge vom Datenträger |
    | --- | --- | --- |
    | 100 MB/s | 100 MB/s | 0 |
    | 100 MB/s | 0 | 100 MB/s |
    | 100 MB/s | 60 MB/s | 40 MB/s |

* **Cachetreffer**

    Cachetreffer werden durch die zugeordneten IOPS- bzw. Durchsatzwerte des Datenträgers nicht eingeschränkt. Wenn Sie beispielsweise einen Datenträger mit der Cacheeinstellung **ReadOnly** auf einer von Storage Premium unterstützten VM verwenden, unterliegen Lesevorgänge, die über den Cache bereitgestellt werden, nicht den IOPS- und Durchsatzbeschränkungen des Datenträgers. Wenn die Workload eines Datenträgers hauptsächlich aus Lesevorgängen besteht, kann sich ein sehr hoher Durchsatz ergeben. Für den Cache gelten auf Ebene des virtuellen Computers separate IOPS- und Durchsatzgrenzwerte basierend auf der VM-Größe. Virtuelle Computer der DS-Serie ermöglichen etwa 4.000 IOPS und 33 MB/s Durchsatz pro Kern für E/A-Vorgänge von Caches und lokalen SSDs. Virtuelle Computer der GS-Serie weisen eine Beschränkung von 5.000 IOPS und 50 MB/s Durchsatz pro Kern für E/A-Vorgänge von Caches und lokalen SSDs auf. 

## <a name="throttling"></a>Drosselung
Es kann zu einer Drosselung kommen, wenn die IOPS-Menge oder der Durchsatz Ihrer Anwendung die zugeordneten Grenzwerte für einen Storage Premium-Datenträger überschreitet. Eine Drosselung kann auch auftreten, wenn Ihr gesamter Datenträger-Datenverkehr für alle Datenträger auf der VM den Grenzwert für die verfügbare Datenträgerbandbreite der VM überschreitet. Zur Vermeidung der Drosselung wird empfohlen, die Anzahl von ausstehenden E/A-Anforderungen für den Datenträger zu begrenzen. Verwenden Sie einen Grenzwert, der auf den Skalierbarkeits- und Leistungszielen für den von Ihnen bereitgestellten Datenträger und auf der Datenträgerbandbreite basiert, die für die VM verfügbar ist.  

Ihre Anwendung kann die geringste Latenz erzielen, wenn sie so entworfen wird, dass Einschränkungen vermieden werden. Wenn die Anzahl der ausstehenden E/A-Anforderungen für den Datenträger zu klein ist, kann Ihre Anwendung aber nicht die maximalen IOPS- und Durchsatzebenen nutzen, die für den Datenträger verfügbar sind.

Die folgenden Beispiele veranschaulichen, wie die Drosselungsebenen berechnet werden. Alle Berechnungen basieren auf einer E/A-Einheitengröße von 256 KB.

### <a name="example-1"></a>Beispiel 1
Ihre Anwendung hat 495 E/A-Einheiten mit einer Größe von 16 KB in einer Sekunde auf einem Datenträger des Typs P10 verarbeitet. Die E/A-Einheiten werden als 495 IOPS gezählt. Wenn Sie versuchen, in der gleichen Sekunde einen E/A-Vorgang mit 2 MB durchzuführen, entspricht die Summe der E/A-Einheiten 495 + 8 IOPS. Dies liegt daran, dass ein E/A-Vorgang mit 2 MB acht E/A-Einheiten entspricht (2.048 KB/256 KB), wenn die Größe der E/A-Einheiten 256 KB beträgt. Da die Summe von 495 + 8 die IOPS-Grenze von 500 für den Datenträger überschreitet, kommt es zu einer Drosselung.

### <a name="example-2"></a>Beispiel 2
Ihre Anwendung hat 400 E/A-Einheiten mit einer Größe von 256 KB auf einem Datenträger des Typs P10 verarbeitet. Verwendete Gesamtbandbreite: (400 · 256)/1.024 KB = 100 MB/s. Das Durchsatzlimit eines P10-Datenträgers liegt bei 100 MB/s. Wenn Ihre Anwendung versucht, weitere E/A-Vorgänge in dieser Sekunde auszuführen, wird sie gedrosselt, weil der zugewiesene Grenzwert überschritten wird.

### <a name="example-3"></a>Beispiel 3
Sie besitzen einen virtuellen DS4-Computer mit zwei angefügten P30-Datenträgern. Für jeden P30-Datenträger ist ein Durchsatz von 200 MB/s möglich. Ein virtueller DS4-Computer verfügt aber über eine Datenträger-Bandbreitenkapazität von insgesamt 256 MB/s. Sie können auf diesem virtuellen DS4-Computer für die angeschlossenen Datenträger nicht gleichzeitig den maximalen Durchsatz erzielen. Um dieses Problem zu beheben, können Sie auf einem Datenträger Datenverkehr von 200 MB/s und auf dem anderen Datenträger 56 MB/s tolerieren. Wenn die Summe Ihres Datenträger-Datenverkehrs 256 MB/s übersteigt, wird der Datenverkehr für den Datenträger gedrosselt.

> [!NOTE]
> Solange der Datenverkehr des Datenträgers vor allem aus kleinen E/A-Größen besteht, ist die Wahrscheinlichkeit hoch, dass Ihre Anwendung den IOPS-Grenzwert vor dem Durchsatzgrenzwert erreicht. Wenn der Datenverkehr des Datenträgers vor allem aus großen E/A-Größen besteht, ist es dagegen wahrscheinlich, dass Ihre Anwendung nicht zuerst den IOPS-Grenzwert erreicht, sondern den Durchsatzgrenzwert. Sie können die IOPS-Menge und die Durchsatzkapazität für Ihre Anwendung erhöhen, indem Sie optimale E/A-Größen verwenden. Außerdem können Sie die Anzahl von ausstehenden E/A-Anforderungen für einen Datenträger begrenzen.
> 

Weitere Informationen zum Erzielen einer hohen Anwendungsleistung mit Storage Premium finden Sie im Artikel [Hohe Anwendungsleistung mit Storage Premium](storage-premium-storage-performance.md).

## <a name="snapshots-and-copy-blob"></a>Momentaufnahmen und Kopieren von Blobs

Für den Storage-Dienst ist die VHD-Datei ein Seitenblob. Sie können Momentaufnahmen von Seitenblobs erstellen und sie an einen anderen Ort kopieren, z.B. in ein anderes Speicherkonto.

### <a name="unmanaged-disks"></a>Nicht verwaltete Datenträger

Erstellen Sie [inkrementelle Momentaufnahmen](storage-incremental-snapshots.md) für nicht verwaltete Premium-Datenträger. Die Vorgehensweise ist dabei die gleiche wie bei Momentaufnahmen für Standardspeicher. Storage Premium unterstützt nur lokal redundanten Speicher als Replikationsoption. Wir empfehlen Ihnen, Momentaufnahmen zu erstellen, und anschließend die Momentaufnahmen in ein geografisch redundantes Standardspeicherkonto zu kopieren. Weitere Informationen finden Sie unter [Redundanzoptionen für Azure Storage](storage-redundancy.md).

Falls ein Datenträger an einen virtuellen Computer angefügt ist, sind einige API-Vorgänge auf dem Datenträger nicht zulässig. Sie können beispielsweise den Vorgang [Kopieren von Blob](/rest/api/storageservices/Copy-Blob) für das Blob nicht durchführen, wenn der Datenträger an den virtuellen Computer angefügt ist. Erstellen Sie stattdessen zuerst eine Momentaufnahme dieses Blobs, indem Sie die [Snapshot Blob](/rest/api/storageservices/Snapshot-Blob)-REST-API verwenden. Führen Sie anschließend den [Copy Blob](/rest/api/storageservices/Copy-Blob)-Vorgang der Momentaufnahme durch, um den angefügten Datenträger zu kopieren. Alternativ hierzu können Sie den Datenträger auch trennen und dann die erforderlichen Vorgänge durchführen.

Für Momentaufnahmen von Storage Premium-Blobs gelten folgende Grenzwerte:

| Storage Premium-Grenzwert | Wert |
| --- | --- |
| Maximale Anzahl von Momentaufnahmen pro Blob | 100 |
| Speicherkontokapazität für Momentaufnahmen<br>(Umfasst nur Daten in Momentaufnahmen. Enthält keine Daten im Basisblob.) | 10 TB |
| Mindestzeitraum zwischen aufeinanderfolgenden Momentaufnahmen | 10 Minuten |

Um georedundante Kopien Ihrer Momentaufnahmen aufzubewahren, können Sie Momentaufnahmen aus einem Premium-Speicherkonto mithilfe von "AzCopy" oder "Copy Blob" in ein georedundantes Standardspeicherkonto kopieren. Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](storage-use-azcopy.md) und [Copy Blob](/rest/api/storageservices/Copy-Blob).

Ausführliche Informationen zum Durchführen von REST-Vorgängen für Seitenblobs in einem Storage Premium-Konto finden Sie unter [Using Blob Service Operations with Azure Premium Storage](http://go.microsoft.com/fwlink/?LinkId=521969) (Verwenden von Blob-Dienstvorgängen mit Azure Storage Premium).

### <a name="managed-disks"></a>Verwaltete Datenträger

Eine Momentaufnahme für einen verwalteten Datenträger ist eine schreibgeschützte Kopie des verwalteten Datenträgers. Die Momentaufnahme wird als verwalteter Standarddatenträger gespeichert. Derzeit werden [inkrementelle Momentaufnahmen](storage-incremental-snapshots.md) für verwaltete Datenträger nicht unterstützt. Informationen dazu, wie Sie eine Momentaufnahme für einen verwalteten Datenträger erstellen, finden Sie unter [Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots (Windows)](../virtual-machines/virtual-machines-windows-snapshot-copy-managed-disk.md) (Erstellen einer Kopie einer als verwalteter Azure-Datenträger gespeicherten VHD mithilfe verwalteter Momentaufnahmen (Windows)) oder [Create a copy of a VHD stored as an Azure Managed Disk by using Managed Snapshots (Linux)](../virtual-machines/linux/snapshot-copy-managed-disk.md) (Erstellen einer Kopie einer als verwalteter Azure-Datenträger gespeicherten VHD mithilfe verwalteter Momentaufnahmen (Linux)).

Falls ein verwalteter Datenträger an einen virtuellen Computer angefügt ist, sind einige API-Vorgänge auf dem Datenträger nicht zulässig. So können Sie beispielsweise keine Shared Access Signature (SAS) generieren, um einen Kopiervorgang auszuführen, während der Datenträger an einen virtuellen Computer angefügt ist. Erstellen Sie stattdessen zunächst eine Momentaufnahme des Datenträgers, und führen Sie dann den Kopiervorgang für die Momentaufnahme aus. Alternativ können Sie den Datenträger trennen und anschließend eine SAS (Shared Access Signature) generieren, um den Kopiervorgang durchzuführen.


## <a name="premium-storage-for-linux-vms"></a>Storage Premium für virtuelle Linux-Computer
Die folgenden Informationen sind beim Einrichten Ihrer Linux-VMs in Storage Premium hilfreich:

Um die Skalierbarkeitsziele in Storage Premium für alle Storage Premium-Datenträger mit der Cacheeinstellung **ReadOnly** oder **None** zu erreichen, müssen Sie beim Bereitstellen des Dateisystems so genannte „Barriers“ (Sperren) deaktivieren. Sie benötigen keine Sperren für dieses Szenario, da die Schreibvorgänge auf Storage Premium-Datenträgern für diese Cacheeinstellungen beständig sind. Wenn die Schreibanforderung erfolgreich abgeschlossen wurde, wurden Daten in den permanenten Speicher geschrieben. Verwenden Sie zum Deaktivieren von Sperren eines der folgenden Verfahren. Wählen Sie das richtige Verfahren für Ihr Dateisystem:
  
* Verwenden Sie zum Deaktivieren von Sperren für **reiserFS** die Bereitstellungsoption `barrier=none`. (Verwenden Sie zum Aktivieren von Sperren `barrier=flush`.)
* Verwenden Sie zum Deaktivieren von Sperren für **ext3/ext4** die Bereitstellungsoption `barrier=0`. (Verwenden Sie zum Aktivieren von Sperren `barrier=1`.)
* Verwenden Sie zum Deaktivieren von Sperren für **XFS** die Bereitstellungsoption `nobarrier`. (Verwenden Sie zum Aktivieren von Sperren `barrier`.)
* Aktivieren Sie Sperren für Storage Premium-Datenträger mit der Cacheeinstellung **ReadWrite**, um Beständigkeit für Schreibvorgänge zu erzielen.
* Damit Volumebezeichnungen nach dem Neustart der VM beibehalten werden, müssen Sie „/etc/fstab“ mit den UUID-Verweisen (Universally Unique Identifier) auf die Datenträger aktualisieren. Weitere Informationen finden Sie unter [Hinzufügen eines verwalteten Datenträgers zu einem virtuellen Linux-Computer](../virtual-machines/linux/add-disk.md).

Die folgenden Linux-Distributionen wurden für Azure Storage Premium überprüft. Wir empfehlen Ihnen, Ihre virtuellen Computer auf mindestens eine dieser Versionen (oder eine höhere Version) zu aktualisieren, um eine bessere Leistung und Stabilität mit Storage Premium zu erzielen. Für einige Versionen sind die neuesten Linux-Integrationsdienste (LIS v4.0) für Azure erforderlich. Verwenden Sie den in der folgenden Tabelle angegebenen Link, um eine Distribution herunterzuladen und zu installieren. Wir fügen der Liste weitere Images hinzu, wenn die jeweilige Überprüfung abgeschlossen ist. Beachten Sie Folgendes: Unser Überprüfungen zeigen, dass die Leistung für jedes Image variiert. Die Leistung hängt von den Workloadmerkmalen und Ihren Imageeinstellungen ab. Verschiedene Images werden für verschiedene Arten von Workloads optimiert.

| Verteilung | Version | Unterstützter Kernel | Details |
| --- | --- | --- | --- |
| Ubuntu | 12.04 | 3.2.0-75.110+ | Ubuntu-12_04_5-LTS-amd64-server-20150119-en-us-30GB |
| Ubuntu | 14.04 | 3.13.0-44.73+ | Ubuntu-14_04_1-LTS-amd64-server-20150123-en-us-30GB |
| Debian | 7.x, 8.x | 3.16.7-ckt4-1+ | &nbsp; |
| SUSE | SLES 12| 3.12.36-38.1+| suse-sles-12-priority-v20150213 <br> suse-sles-12-v20150213 |
| SUSE | SLES 11 SP4 | 3.0.101-0.63.1+ | &nbsp; |
| CoreOS | 584.0.0+| 3.18.4+ | CoreOS 584.0.0 |
| CentOS | 6.5, 6.6, 6.7, 7.0 | &nbsp; | [LIS4 erforderlich](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Siehe Hinweis im nächsten Abschnitt* |
| CentOS | 7.1+ | 3.10.0-229.1.2.el7+ | [LIS4 empfohlen](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *Siehe Hinweis im nächsten Abschnitt* |
| Red Hat Enterprise Linux (RHEL) | 6.8+, 7.2+ | &nbsp; | &nbsp; |
| Oracle | 6.0+, 7.2+ | &nbsp; | UEK4 oder RHCK |
| Oracle | 7.0-7.1 | &nbsp; | UEK4 oder RHCK mit [LIS 4.1+](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |
| Oracle | 6.4-6.7 | &nbsp; | UEK4 oder RHCK mit [LIS 4.1+](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |


### <a name="lis-drivers-for-openlogic-centos"></a>LIS-Treiber für OpenLogic CentOS

Wenn Sie virtuelle OpenLogic CentOS-Computer ausführen, sollten Sie den folgenden Befehl zum Installieren der neuesten Treiber verwenden:

```
sudo rpm -e hypervkvpd  ## (Might return an error if not installed. That's OK.)
sudo yum install microsoft-hyper-v
```

Starten Sie den Computer neu, um die neuen Treiber zu aktivieren.

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Bei Verwendung von Storage Premium gilt für die Abrechnung Folgendes:

* **Größe von Storage Premium-Datenträgern und -Blobs**

    Die Abrechnung für einen Storage Premium-Datenträger bzw. ein -Blob hängt von der Größe ab, die für den Datenträger bzw. das Blob bereitgestellt wurde. Azure ordnet die bereitgestellte Größe (aufgerundet) der nächsten Storage Premium-Datenträgeroption zu. Einzelheiten finden Sie in der Tabelle unter [Skalierbarkeits- und Leistungsziele für Storage Premium](#premium-storage-scalability-and-performance-targets). Jeder Datenträger ist einer unterstützten bereitgestellten Datenträgergröße zugeordnet und wird entsprechend berechnet. Die Abrechnung für bereitgestellte Datenträger erfolgt anteilig auf Stundenbasis unter Verwendung des monatlichen Preises für das Storage Premium-Angebot. Wenn Sie z.B. einen Datenträger des Typs P10 bereitgestellt und diesen nach 20 Stunden gelöscht haben, erfolgt die Abrechnung für das P10-Angebot anteilig für 20 Stunden. Dies ist unabhängig von der tatsächlichen Datenmenge, die auf den Datenträger geschrieben wurde, oder dem verwendeten IOPS und Durchsatz.

* **Momentaufnahmen von nicht verwalteten Premium-Datenträgern**

    Für Momentaufnahmen für nicht verwaltete Premium-Datenträger wird die zusätzliche Kapazität in Rechnung gestellt, die von den Momentaufnahmen verwendet wird. Weitere Informationen zu Momentaufnahmen finden Sie unter [Create a snapshot of a blob](/rest/api/storageservices/Snapshot-Blob) (Erstellen einer Momentaufnahme eines Blobs).

* **Momentaufnahmen von verwalteten Premium-Datenträgern**

    Eine Momentaufnahme eines verwalteten Datenträgers ist eine schreibgeschützte Kopie des Datenträgers. Der Datenträger wird als verwalteter Standarddatenträger gespeichert. Für eine Momentaufnahme fallen die gleichen Kosten wie für einen verwalteten Standarddatenträger an. Wenn Sie beispielsweise eine Momentaufnahme eines verwalteten Premium-Datenträgers mit 128 GB erstellen, entsprechen die Kosten für die Momentaufnahme den Kosten für einen verwalteten Standarddatenträger mit 128 GB.  

* **Ausgehende Datenübertragungen**

    [Ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/data-transfers/) (Daten, die von den Azure-Datencentern ausgehen) verursachen Kosten bei der Bandbreitenverwendung.

Ausführliche Informationen zu den Preisen für Storage Premium, von Storage Premium unterstützte VMs und verwaltete Datenträger finden Sie in diesen Artikeln:

* [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Verwaltete Datenträger – Preise](https://azure.microsoft.com/pricing/details/managed-disks/)

## <a name="azure-backup-support"></a>Azure Backup-Unterstützung 

Für eine regionale Notfallwiederherstellung müssen Sie Ihre VM-Datenträger in einer anderen Region sichern, indem Sie [Azure Backup](../backup/backup-introduction-to-azure-backup.md) und ein GRS-Speicherkonto als Sicherungstresor verwenden.

Verwenden Sie Azure Backup, um einen Sicherungsauftrag mit zeitbasierten Sicherungen, unkomplizierter Wiederherstellung von virtuellen Computern und Aufbewahrungsrichtlinien für Sicherungen zu erstellen. Sie können Backup sowohl mit nicht verwalteten als auch mit verwalteten Datenträgern verwenden. Weitere Informationen finden Sie unter [Sichern virtueller Azure-Computer in Recovery Services-Tresoren](../backup/backup-azure-vms-first-look-arm.md) und [Verwendung von virtuellen Computern auf verwalteten Datenträgern](../backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup). 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Storage Premium finden Sie in den folgenden Artikeln.

### <a name="design-and-implement-with-premium-storage"></a>Entwurf und Implementierung mit Storage Premium
* [Hohe Anwendungsleistung mit Storage Premium](storage-premium-storage-performance.md)
* [Using Blob Service Operations with Azure Premium Storage](http://go.microsoft.com/fwlink/?LinkId=521969) (Verwenden von Blob-Dienstvorgängen mit Azure Storage Premium)

### <a name="operational-guidance"></a>Leitfaden
* [Migrieren zu Azure Storage Premium (Nicht verwaltete Datenträger)](storage-migration-to-premium-storage.md)

### <a name="blog-posts"></a>Blogbeiträge
* [Azure Premium Storage, now generally available](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) (Azure Storage Premium, jetzt allgemein verfügbar)
* [Announcing the GS-Series: Adding Premium Storage Support to the Largest VMs in the Public Cloud](https://azure.microsoft.com/blog/azure-has-the-most-powerful-vms-in-the-public-cloud/) (Ankündigung der GS-Serie: Unterstützung von Storage Premium für die größten virtuellen Computer in der öffentlichen Cloud)

