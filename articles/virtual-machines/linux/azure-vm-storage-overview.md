---
title: Azure-Linux-VMs und Azure Storage | Microsoft-Dokumentation
description: "Beschreibt die Verwendung von Azure Storage Standard und Azure Storage Premium sowie von verwalteten und nicht verwalteten Datenträgern mit virtuellen Linux-Computern."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: d364c69e-0bd1-4f80-9838-bbc0a95af48c
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 2/7/2017
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 0151e188fde38c7a617cf2070939c6498142dd71
ms.lasthandoff: 04/03/2017


---
# <a name="azure-and-linux-vm-storage"></a>Azure- und Linux-VM-Speicher
Azure Storage ist eine Cloudspeicherlösung für moderne Anwendungen, die eine Kombination aus Dauerhaftigkeit, Verfügbarkeit und Skalierbarkeit benötigen, um die Anforderungen ihrer Kunden zu erfüllen.  Mit Azure Storage können Entwickler nicht nur umfangreiche Anwendungen für neue Szenarien entwickeln, es bildet auch die Speichergrundlage für Azure Virtual Machines.

## <a name="managed-disks"></a>Verwaltete Datenträger

Für virtuelle Azure-Computer ist jetzt [Azure Managed Disks](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verfügbar – dieser Dienst ermöglicht Ihnen das Erstellen von virtuellen Computern, ohne dass Sie selbst [Azure Storage-Konten](../../storage/storage-introduction.md) erstellen oder verwalten müssen. Sie geben an, ob Sie Storage Standard oder Premium verwenden möchten, und wie groß der Datenträger sein soll – Azure erstellt die Datenträger für die virtuellen Computer für Sie. Virtuelle Computer mit verwalteten Datenträgern bietet eine Vielzahl wichtiger Features, wie z.B.:

- Unterstützung für automatische Skalierbarkeit. Azure erstellt die Datenträger und verwaltet den zugrunde liegenden Speicher, um bis zu 10.000 Datenträger pro Abonnement zu unterstützen.
- Höhere Zuverlässigkeit mit Verfügbarkeitsgruppen. Azure stellt sicher, dass Datenträger für virtuelle Computer innerhalb der gleichen Verfügbarkeitsgruppe automatisch voneinander isoliert werden.
- Verbesserte Zugriffssteuerung. Verwaltete Datenträger machen verschiedene Vorgänge verfügbar, die über die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](../../active-directory/role-based-access-control-what-is.md) gesteuert werden.

Die Preise für verwaltete Datenträger unterscheiden sich von denen für nicht verwaltete Datenträger. Informationen dazu finden Sie unter [Preise und Abrechnung für verwaltete Datenträger](../../storage/storage-managed-disks-overview.md#pricing-and-billing).

Sie können vorhandene virtuelle Computer, die nicht verwaltete Datenträger verwenden, mithilfe von [az vm convert](/cli/azure/vm#convert) zur Verwendung von verwalteten Datenträgern konvertieren. Weitere Informationen finden Sie unter [Konvertieren eines virtuellen Linux-Computers von nicht verwalteten Datenträgern zu Azure Managed Disks](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sie können einen nicht verwalteten Datenträger nicht in einen verwalteten Datenträger konvertieren, wenn der nicht verwaltete Datenträger sich in einem Speicherkonto befindet, das mithilfe von [Azure Storage Service Encryption (SSE)](../../storage/storage-service-encryption.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verschlüsselt ist oder jemals war. Die folgenden Schritte beschreiben, wie Sie nicht verwaltete Datenträger konvertieren, die sich in einem verschlüsselten Speicherkonto befinden oder jemals befanden:

- Kopieren Sie die virtuelle Festplatte (VHD) mit [az storage blob copy start](/cli/azure/storage/blob/copy#start) in ein Speicherkonto, das nie für Azure Storage Service Encryption aktiviert wurde.
- Erstellen Sie einen virtuellen Computer, der verwaltete Datenträger verwendet, und geben Sie diese VHD-Datei während der Erstellung mit [az vm create](/cli/azure/vm#create) an. Alternativ dazu:
- Fügen Sie die kopierte VHD mit [az vm disk attach](/cli/azure/vm/disk#attach) an einen ausgeführten virtuellen Computer mit verwalteten Datenträgern an.


## <a name="azure-storage-standard-and-premium"></a>Azure Storage: Standard und Premium
Virtuelle Azure-Computer – unabhängig davon, ob sie verwaltete oder nicht verwaltete Datenträger verwenden – können basierend auf Standard- oder Premium-Speicherdatenträgern erstellt werden. Wenn Sie zum Auswählen Ihres virtuellen Computers das Portal verwenden, müssen Sie auf dem Bildschirm **Grundlagen** eine Dropdownliste umschalten, um sowohl Standard- als auch Premium-Datenträger anzuzeigen. Beim Umschalten zu SSD werden nur für Storage Premium geeignete VMs angezeigt, die alle durch SSD-Laufwerke gestützt werden.  Bei Auswahl von HDD werden für Storage Standard geeignete virtuelle Computer mit rotierenden Datenträgern sowie für Storage Premium geeignete virtuelle Computer mit SSD-Laufwerken angezeigt.

Beim Erstellen eines virtuellen Computers aus der `azure-cli` können Sie über das `-z`- oder `--vm-size`-CLI-Flag bei der Auswahl der VM-Größe zwischen Standard und Premium wählen.

## <a name="creating-a-vm-with-a-managed-disk"></a>Erstellen eines virtuellen Computers mit einem verwalteten Datenträger

Das folgende Beispiel erfordert Azure CLI 2.0, die Sie [hier installieren] können.

Erstellen Sie zuerst eine Ressourcengruppe für die Verwaltung der Ressourcen:

```azurecli
az group create --location westus --name myResourceGroup
```

Erstellen Sie dann mit dem Befehl `az vm create` den virtuellen Computer, wie in folgendem Beispiel gezeigt. Denken Sie daran, ein eindeutiges `--public-ip-address-dns-name`-Argument anzugeben, da wahrscheinlich `manageddisks` verwendet wird.

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username azureuser \
--ssh-key-value ~/.ssh/id_rsa.pub
--public-ip-address-dns-name manageddisks \
--resource-group myResourceGroup \
--location westus \
--name myVM
```

Im vorherigen Beispiel wurde ein virtueller Computer mit einem verwalteten Datenträger in einem Standard-Speicherkonto erstellt. Um ein Premium-Speicherkonto zu verwenden, fügen Sie das `--storage-sku Premium_LRS`-Argument hinzu, wie in diesem Beispiel gezeigt:

```azurecli
az vm create \
--storage-sku Premium_LRS
--image credativ:Debian:8:latest \
--admin-username azureuser \
--ssh-key-value ~/.ssh/id_rsa.pub
--public-ip-address-dns-name manageddisks \
--resource-group myResourceGroup \
--location westus \
--name myVM
```


### <a name="create-a-vm-with-an-unmanaged-standard-disk-using-the-azure-cli-10"></a>Erstellen eines virtuellen Computers mit einem nicht verwalteten Standarddatenträger über die Azure-Befehlszeilenschnittstelle 1.0

Sie können natürlich auch die Azure-Befehlszeilenschnittstelle 1.0 verwenden, um virtuelle Computer mit Standard- und Premium-Datenträgern zu erstellen. Zurzeit ist die Erstellung von virtuellen Computern mit verwalteten Datenträgern über die Azure-Befehlszeilenschnittstelle 1.0 nicht möglich.

Die Option `-z` dient zur Auswahl von Standard_A1, einem auf Standardspeicher basierenden virtuellen Linux-Computer.

```azurecli
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_A1
```

### <a name="create-a-vm-with-premium-storage-using-the-azure-cli-10"></a>Erstellen einer VM mit Storage Premium über die Azure-Befehlszeilenschnittstelle 1.0
Die Option `-z` dient zur Auswahl von Standard_DS1, einem auf Premium-Speicher basierenden virtuellen Linux-Computer.

```azurecli
azure vm quick-create -g rbg \
exampleVMname \
-l westus \
-y Linux \
-Q Debian \
-u exampleAdminUser \
-M ~/.ssh/id_rsa.pub
-z Standard_DS1
```

## <a name="standard-storage"></a>Standardspeicher
Azure-Standardspeicher ist der Standardtyp des Speichers.  Standardspeicher ist kostengünstig und bietet dabei eine gute Leistung.  

## <a name="premium-storage"></a>Storage Premium
Azure Storage Premium bietet Datenträgerunterstützung für hohe Leistung mit geringer Latenz für virtuelle Computer mit E/A-intensiven Workloads. Datenträger von virtuellen Computern (VM), die Storage Premium nutzen, speichern Daten auf SSDs (Solid State Drives). Sie können die VM-Datenträger Ihrer Anwendung zu Azure Storage Premium migrieren, um von der Geschwindigkeit und Leistung dieser Laufwerke zu profitieren.

Features von Storage Premium:

* Storage Premium-Datenträger: Azure Storage Premium unterstützt VM-Datenträger, die an virtuelle Azure-Computer der DS-, DSv2- oder GS-Serie angefügt werden können.
* Storage Premium-Seitenblob: Storage Premium unterstützt Azure-Seitenblobs. Sie werden verwendet, um persistente Datenträger für virtuelle Azure-Computer zu speichern.
* Lokal redundanter Storage Premium: Ein Storage Premium-Konto unterstützt nur den lokal redundanten Speicher (LRS) als die Replikatsoption und verwaltet drei Kopien der Daten innerhalb einer einzelnen Region.
* [Storage Premium](../../storage/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Durch Storage Premium unterstützte virtuelle Computer
Storage Premium unterstützt virtuelle Azure-Computer der DS-, DSv2-, GS- und Fs-Serie. Mit durch Storage Premium unterstützten virtuellen Computern können Sie Standard- und Premium-Speicherdatenträger verwenden. Sie können jedoch keine Storage Premium-Datenträger mit Serien von virtuellen Computern verwenden, die nicht mit Storage Premium kompatibel sind.

Nachfolgend sind die Linux-Distributionen aufgeführt, die für Premium-Speicher überprüft wurden.

| Verteilung | Version | Unterstützter Kernel |
| --- | --- | --- |
| Ubuntu |12.04 |3.2.0-75.110+ |
| Ubuntu |14.04 |3.13.0-44.73+ |
| Debian |7.x, 8.x |3.16.7-ckt4-1+ |
| SLES |SLES 12 |3.12.36-38.1+ |
| SLES |SLES 11 SP4 |3.0.101-0.63.1+ |
| CoreOS |584.0.0+ |3.18.4+ |
| Centos |6.5, 6.6, 6.7, 7.0, 7.1 |3.10.0-229.1.2.el7+ |
| RHEL |6.8+, 7.2+ | |

## <a name="file-storage"></a>File Storage
Der Azure-Dateispeicher verfügt über Dateifreigaben in der Cloud unter Verwendung des standardmäßigen SMB-Protokolls. Mit Azure Files können Sie Unternehmensanwendungen migrieren, die auf Dateiservern in Azure basieren. Anwendungen, die in Azure ausgeführt werden, können Dateifreigaben von virtuellen Azure-Computern unter Linux problemlos einbinden. Mit der neuesten Version des Dateispeichers können Sie außerdem eine Dateifreigabe aus einer lokalen Anwendung einbinden, die SMB 3.0 unterstützt.  Da es sich bei den Dateifreigaben um SMB-Freigaben handelt, können Sie darauf über standardmäßige Dateisystem-APIs zugreifen.

Der Dateispeicher beruht auf der gleichen Technologie wie Blob-, Tabellen-, und Warteschlangendienste. Dies bedeutet, dass der Dateispeicher die Verfügbarkeit, Dauerhaftigkeit, Skalierbarkeit und geografische Redundanz bietet, die in die Azure Storage-Plattform integriert ist. Weitere Informationen zu Leistungszielen und Grenzwerten des Dateispeichers finden Sie unter „Skalierbarkeits- und Leistungsziele für Azure Storage“.

* [Verwenden des Azure-Dateispeichers unter Linux](../../storage/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>„Heißer“ Speicher
Die Speicherebene „Heiß“ von Azure ist für die Speicherung von Daten optimiert, auf die häufig zugegriffen wird.  „Heißer“ Speicher ist der Standardspeichertyp für Blobspeicher.

## <a name="cool-storage"></a>„Kalter“ Speicher
Die Speicherebene „Kalt“ von Azure ist für die Speicherung von Daten optimiert, auf die weniger häufig zugegriffen wird und die langlebiger sind. Beispiele für den Einsatz von „kaltem“ Speicher sind Sicherungen, Medieninhalt, wissenschaftliche Daten, Compliance- und Archivdaten. Im Allgemeinen sind alle Daten, auf die selten zugegriffen wird, perfekte Kandidaten für „kalten“ Speicher.

|  | Speicherebene „Heiß“ (Hot) | Speicherebene „Kalt“ (Cool) |
|:--- |:---:|:---:|
| Availability |99,9 % |99 % |
| Verfügbarkeit (RA-GRS-Lesevorgänge) |99,99 % |99,9 % |
| Usage Charges |Höhere Speicherkosten |Geringere Speicherkosten |
| Geringerer Zugriff |Höherer Zugriff | |
| und Transaktionskosten |und Transaktionskosten | |

## <a name="redundancy"></a>Redundanz
Die Daten in Ihrem Microsoft Azure-Speicherkonto werden stets repliziert, um Beständigkeit und hohe Verfügbarkeit sicherzustellen sowie die Azure Storage-SLA auch bei vorübergehend auftretenden Hardwareausfällen zu erfüllen.

Wenn Sie ein Speicherkonto erstellen, müssen Sie eine der folgenden Replikationsoptionen auswählen:

* Lokal redundanter Speicher (LRS)
* Zonenredundanter Speicher (ZRS)
* Georedundanter Speicher (GRS)
* Georedundanter Speicher mit Lesezugriff (RA-GRS)

### <a name="locally-redundant-storage"></a>Lokal redundanter Speicher
Lokal redundanter Speicher (LRS) repliziert Ihre Daten innerhalb der Region, in der Sie das Speicherkonto erstellt haben. Um die Beständigkeit zu maximieren, wird jede im Speicherkonto an Ihre Daten gerichtete Anforderung dreimal repliziert. Diese drei Replikate befinden sich in separaten Fehler- und Upgradedomänen.  Eine Anforderung wird nur einmal erfolgreich zurückgegeben, nachdem sie in alle drei Replikate geschrieben wurde.

### <a name="zone-redundant-storage"></a>Zonenredundanter Speicher
Zonenredundanter Speicher (ZRS) repliziert Ihre Daten in zwei bis drei Rechenzentren, entweder innerhalb einer einzelnen Region oder in zwei Regionen, wodurch eine höhere Beständigkeit als bei LRS gewährleistet ist. Wenn für Ihr Speicherkonto ZRS aktiviert ist, sind Ihre Daten selbst bei einem Ausfall eines der Rechenzentren beständig gespeichert.

### <a name="geo-redundant-storage"></a>Georedundanter Speicher
Georedundanter Speicher (GRS) repliziert Ihre Daten in eine sekundäre Region, die Hunderte von Kilometern von der primären Region entfernt ist. Wenn für Ihr Speicherkonto GRS aktiviert ist, sind Ihre Daten beständig gespeichert, selbst bei einem regionalen Komplettausfall oder einem Notfall, nach dem die primäre Region nicht mehr wiederherstellbar ist.

### <a name="read-access-geo-redundant-storage"></a>Georedundanter Speicher mit Lesezugriff
Georedundanter Speicher mit Lesezugriff (RA-GRS) maximiert die Verfügbarkeit für das Speicherkonto, indem ein schreibgeschützter Zugriff auf Daten am sekundären Standort zusätzlich zur von GRS gebotenen Replikation in zwei Regionen bereitgestellt wird. Falls Daten in der primären Region nicht verfügbar sind, kann Ihre Anwendung Daten aus der sekundären Region lesen.

Fundierte Einblicke in Azure-Speicherredundanz finden Sie unter:

* [Azure Storage-Replikation](../../storage/storage-redundancy.md)

## <a name="scalability"></a>Skalierbarkeit
Dank der äußerst flexiblen Skalierbarkeit von Azure Storage können Sie Hunderte Terabytes an Daten speichern und verarbeiten, um den Big Data-Anforderungen von Anwendungen in den Bereichen Wissenschaft, Finanzanalyse und Medien gerecht zu werden. Sie können aber ebenso gut die geringen Datenmengen der Website eines kleinen Unternehmens speichern. In jedem Fall bezahlen Sie nur für die Daten, die Sie tatsächlich speichern. Azure Storage speichert derzeit mehrere Billionen einzigartiger Kundenobjekte und verarbeitet im Schnitt mehrere Millionen Anforderungen pro Sekunde.

Für Standardspeicherkonten: Ein Standardspeicherkonto hat eine maximale Gesamtanforderungsrate von 20.000 IOPS. Die gesamten IOPS auf allen Datenträgern eines virtuellen Computers in einem Standardspeicherkonto dürfen dieses Limit nicht überschreiten.

Für Storage Premium-Konten: Ein Storage Premium-Konto hat eine maximale Gesamtdurchsatzrate von 50 GBit/s. Der Gesamtdurchsatz aller Ihrer VM-Datenträger darf dieses Limit nicht überschreiten.

## <a name="availability"></a>Verfügbarkeit
Wir garantieren eine erfolgreiche Verarbeitung von Anforderungen zum Lesen von Daten aus RA-GRS-Konten (georedundanter Speicher mit Lesezugriff) in mindestens 99,99% der Zeit (99,9% bei der Zugriffsstufe „Kalt“), wenn nicht erfolgreiche Versuche, Daten aus der primären Region zu lesen, in der sekundären Region wiederholt werden.

Für Anforderungen zum Lesen von Daten aus LRS-, ZRS- und GRS-Konten (lokal redundanter, zonenredundanter und georedundanter Speicher) garantieren wir eine erfolgreiche Verarbeitung in mindestens 99,9% der Zeit (99% bei der Zugriffsstufe „Kalt“).

Für Anforderungen zum Schreiben von Daten in LRS-, ZRS-, GRS- und RA-GRS-Konten (lokal redundanter, zonenredundanter, georedundanter Speicher und georedundanter Speicher mit Lesezugriff) garantieren wir eine erfolgreiche Verarbeitung in mindestens 99,9% der Zeit (99% bei der Zugriffsstufe „Kalt“).

* [Azure-SLA für Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)

## <a name="regions"></a>Regionen
Azure ist in 30 Regionen weltweit allgemein verfügbar. Die Verfügbarkeit in 4 weiteren Regionen ist geplant und wurde bereits angekündigt. Um die Leistung für unsere Kunden weiter zu steigern und die Kundenanforderungen im Hinblick auf den Speicherort ihrer Daten zu erfüllen, ist die geografische Erweiterung eine unserer Prioritäten.  Die neueste zu startende Azure-Region ist Deutschland.

Die Microsoft Cloud Deutschland bietet eine differenzierte Option zur Nutzung der bereits in ganz Europa verfügbaren Microsoft Cloud Services und schafft höhere Innovations- und Wirtschaftswachstumschancen für Partner und Kunden in Deutschland, der Europäischen Union (EU) und der Europäischen Freihandelsassoziation (EFTA), die strengen Regeln unterliegen.

Die Kundendaten werden in diesen neuen Rechenzentren in Magdeburg und Frankfurt unter der Kontrolle eines Datentreuhänders verwaltet – T-Systems International, ein unabhängiges deutsches Unternehmen und eine Tochtergesellschaft von Deutsche Telekom. Microsofts kommerzielle Clouddienste in diesen Rechenzentren halten die deutschen Vorschriften zur Datenverarbeitung ein und bieten Kunden weitere Möglichkeiten, zu wählen, wie und wo die Daten verarbeitet werden.

* [Azure-Regionenkarte](https://azure.microsoft.com/regions/)

## <a name="security"></a>Sicherheit
Azure Storage bietet einen umfassenden Satz von Sicherheitsfunktionen, die Entwicklern das Erstellen sicherer Anwendungen ermöglichen. Das Speicherkonto selbst kann mit rollenbasierter Zugriffssteuerung und Azure Active Directory geschützt werden. Daten können während der Übertragung zwischen einer Anwendung und Azure mit clientseitiger Verschlüsselung, HTTPS oder SMB 3.0 geschützt werden. Es kann festgelegt werden, dass Daten automatisch mit Storage Service Encryption (SSE) verschlüsselt werden, wenn sie in Azure Storage geschrieben werden. Betriebssystemdatenträger und sonstige Datenträger, die von virtuellen Computern verwendet werden, können mit Azure Disk Encryption verschlüsselt werden. Delegierter Zugriff auf die Datenobjekte in Azure Storage kann mit Shared Access Signatures erteilt werden.

### <a name="management-plane-security"></a>Sicherheit auf Verwaltungsebene
Die Verwaltungsebene besteht aus den Ressourcen, die zum Verwalten Ihres Speicherkonto verwendet werden. In diesem Abschnitt erläutern wir das Azure Resource Manager-Bereitstellungsmodell und beschreiben, wie Sie die rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC) verwenden können, um den Zugriff auf Ihre Speicherkonten zu steuern. Wir behandeln auch die Verwaltung Ihrer Speicherkontoschlüssel, und wie Sie diese erneut generieren können.

### <a name="data-plane-security"></a>Sicherheit auf Datenebene
In diesem Abschnitt betrachten wir die Gewährung des Zugriffs auf die tatsächlichen Datenobjekte in Ihrem Speicherkonto, z. B. Blobs, Dateien, Warteschlangen und Tabellen mit SAS und gespeicherten Zugriffsrichtlinien. Wir betrachten SAS sowohl auf Dienst- als auch auf Kontoebene. Wir behandeln auch die Beschränkung des Zugriffs auf eine bestimmte IP-Adresse (oder einen Bereich von IP-Adressen), die Beschränkung des verwendeten HTTPS-Protokolls und das Widerrufen einer SAS, ohne ihren Ablauf abzuwarten.

## <a name="encryption-in-transit"></a>Verschlüsselung während der Übertragung
In diesem Abschnitt wird erläutert, wie Sie Daten sichern, wenn Sie sie in oder aus Azure Storage übertragen. Wir behandeln die empfohlene Verwendung von HTTPS und die Verschlüsselung, die SMB 3.0 für Azure-Dateifreigaben verwendet. Wir werfen auch einen Blick auf die clientseitige Verschlüsselung, mit der Sie die Daten verschlüsseln können, bevor sie in einer Clientanwendung in den Speicher übertragen werden, und nach der Übertragung aus dem Speicher entschlüsseln können.

## <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten
Wir erläutern Storage Service Encryption (SSE) und beschreiben, und wie Sie diese Verschlüsselung für ein Speicherkonto aktivieren können, sodass Ihre Block-, Seiten- und Anfügeblobs beim Schreiben in Azure Storage automatisch verschlüsselt werden. Außerdem erläutern wie die Verwendung von Azure Disk Encryption und untersuchen die grundlegenden Anwendungsfälle von Azure Disk Encryption, SSE und clientseitiger Verschlüsselung sowie deren wesentlichen Unterschiede. Wir betrachten kurz die FIPS-Konformität für die US- Regierungscomputer.

* [Azure Storage-Sicherheitsleitfaden](../../storage/storage-security-guide.md)

## <a name="temporary-disk"></a>Temporärer Datenträger
Jede VM verfügt über einen temporären Datenträger. Der temporäre Datenträger bietet kurzfristigen Speicher für Anwendungen und Prozesse und ist ausschließlich dafür ausgelegt, Daten wie z.B. Seiten-oder Auslagerungsdateien zu speichern. Daten auf dem temporären Datenträger können während eines [Wartungsereignisses](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#understand-planned-vs-unplanned-maintenance) verloren gehen, oder wenn Sie [eine VM erneut bereitstellen](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Während eines standardmäßigen Neustarts der VM sollten die Daten auf dem virtuellen Datenträger erhalten bleiben.

Auf virtuellen Linux-Computern lautet der Datenträger in der Regel **/dev/sdb**. Er wird vom Azure Linux-Agent formatiert und in **/mnt/** eingebunden. Die Größe des temporären Datenträgers variiert basierend auf der Größe des virtuellen Computers. Weitere Informationen finden Sie unter [Größen für virtuelle Computer in Azure](sizes.md).

Weitere Informationen zur Verwendung des temporären Datenträgers in Azure finden Sie unter [Understanding the temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="cost-savings"></a>Kostenersparnis
* [Speicherkosten](https://azure.microsoft.com/pricing/details/storage/)
* [Speicherkostenrechner](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Speichergrenzwerte
* [Grenzwerte für den Speicherdienst](../../azure-subscription-service-limits.md#storage-limits)

