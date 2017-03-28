---
title: "Übersicht über virtuelle Windows-Computer | Microsoft Docs"
description: Hier erfahren Sie, wie Sie virtuelle Windows-Computer in Azure erstellen und verwalten.
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: fbae9c8e-2341-4ed0-bb20-fd4debb2f9ca
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/01/2017
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: f86b47f3886571b0795bc858a1a2c0757c6fb7b6
ms.lasthandoff: 03/17/2017


---
# <a name="overview-of-windows-virtual-machines-in-azure"></a>Übersicht über virtuelle Windows-Computer in Azure
Virtuelle Azure-Computer (Virtual Machines, VMs) sind eine von mehreren [bedarfsgesteuerten, skalierbaren Computerressourcen](../app-service-web/choose-web-site-cloud-service-vm.md), die von Azure angeboten werden. Virtuelle Computer werden in der Regel verwendet, wenn Sie mehr Kontrolle über Ihre Computerumgebung benötigen als bei den anderen Optionen zur Verfügung steht. In diesem Artikel erfahren Sie, was Sie vor der Erstellung eines virtuellen Computers berücksichtigen sollten und wie Sie ihn erstellen und verwalten.

Ein virtueller Azure-Computer bietet Ihnen die Flexibilität der Virtualisierung, ohne Zeit und Geld für den Kauf und die Verwaltung der Hardware aufwenden zu müssen, mit der der virtuelle Computer betrieben wird. Der virtuelle Computer muss allerdings weiterhin verwaltet werden – beispielsweise durch Konfigurieren, Patchen und Verwalten der darauf ausgeführten Software.

Virtuelle Azure-Computer können auf vielfältige Weise genutzt werden. Hier einige Beispiele:

* **Entwickeln und Testen:** Virtuelle Azure-Computer stellen eine schnelle und einfache Möglichkeit zum Erstellen eines Computers mit speziellen Konfigurationen dar, die zum Programmieren und Testen einer Anwendung erforderlich sind.
* **Anwendungen in der Cloud:** Da die Nutzung Ihrer Anwendung Schwankungen unterliegen kann, ist es unter Umständen wirtschaftlich sinnvoll, sie auf einem virtuellen Computer in Azure auszuführen. Sie bezahlen für zusätzliche virtuelle Computer, wenn Sie sie benötigen, und fahren sie andernfalls einfach herunter.
* **Erweitertes Datencenter:** Virtuelle Computer in einem virtuellen Azure-Netzwerk lassen sich problemlos mit dem Netzwerk Ihrer Organisation verbinden.

Die Anzahl virtueller Computer, die von Ihrer Anwendung genutzt werden, kann zentral oder horizontal hochskaliert werden, um Ihren jeweiligen Anforderungen gerecht zu werden.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Was muss ich vor dem Erstellen eines virtuellen Computers berücksichtigen?
Beim Einrichten einer Anwendungsinfrastruktur in Azure müssen immer zahlreiche [Designaspekte](virtual-machines-windows-infrastructure-virtual-machine-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) berücksichtigt werden. Machen Sie sich daher zunächst Gedanken über Folgendes:

* Die Namen Ihrer Anwendungsressourcen
* Den Speicherort der Ressourcen
* Die Größe des virtuellen Computers
* Die maximal erstellbare Anzahl virtueller Computer
* Das Betriebssystem für den virtuellen Computer
* Die Konfiguration des virtuellen Computers nach dem Start
* Die zugehörigen Ressourcen, die der virtuelle Computer benötigt

### <a name="naming"></a>Benennung
Einem virtuellen Computer wird ein [Name](virtual-machines-windows-infrastructure-naming-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zugewiesen, und im Zusammenhang mit dem Betriebssystem wird ein Computername konfiguriert. Der Name eines virtuellen Computers kann bis zu 15 Zeichen lang sein.

Wenn Sie den Betriebssystem-Datenträger mithilfe von Azure erstellen, sind der Computername und der Name des virtuellen Computers identisch. Wenn Sie [ein eigenes Image hochladen und verwenden](virtual-machines-windows-upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), das ein vorab konfiguriertes Betriebssystem enthält, und auf dessen Grundlage einen virtuellen Computer erstellen, können die Namen unterschiedlich sein. Beim Hochladen einer eigenen Imagedatei empfiehlt es sich, für den Computernamen im Betriebssystem und für den Namen des virtuellen Computers den gleichen Namen zu verwenden.

### <a name="locations"></a>Standorte
Alle in Azure erstellten Ressourcen werden auf [geografische Regionen](https://azure.microsoft.com/regions/) auf der ganzen Welt verteilt. Bei der Erstellung eines virtuellen Computers wird die Region in der Regel als **Standort** bezeichnet. Der Standort gibt für einen virtuellen Computer an, wo die virtuellen Festplatten gespeichert sind.

Die folgende Tabelle enthält einige Methoden, mit denen Sie eine Liste verfügbarer Standorte abrufen können:

| Methode | Beschreibung |
| --- | --- |
| Azure-Portal |Wählen Sie beim Erstellen eines virtuellen Computers einen Standort aus der Liste aus. |
| Azure PowerShell |Verwenden Sie den Befehl [Get-AzureRmLocation](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/get-azurermlocation). |
| REST-API |Verwenden Sie den Vorgang [List locations](https://docs.microsoft.com/rest/api/resources/subscriptions#Subscriptions_ListLocations) (Standorte auflisten). |

### <a name="vm-size"></a>Größe des virtuellen Computers
Die [Größe](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) des virtuellen Computers richtet sich nach der Workload, die Sie ausführen möchten. Von der gewählten Größe hängen Faktoren wie Rechenleistung, Arbeitsspeicher und Speicherplatz ab. Azure bietet eine Vielzahl von Größen zur Unterstützung vieler Anwendungstypen.

Bei Azure wird auf der Grundlage von Größe und Betriebssystem des virtuellen Computers ein [Stundenpreis](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) berechnet. Für angefangene Stunden werden lediglich die in Anspruch genommenen Minuten abgerechnet. Speicherplatz wird separat bewertet und in Rechnung gestellt.

### <a name="vm-limits"></a>Grenzwerte für virtuelle Computer
Für Ihr Abonnement gelten standardmäßig bestimmte [Kontingentgrenzen](../azure-subscription-service-limits.md), die die Bereitstellung einer hohen Anzahl virtueller Computer für Ihr Projekt beeinträchtigen können. Der derzeitige Grenzwert pro Abonnement liegt bei 20 VMs pro Region. Zur Erhöhung der Grenzwerte können Sie ein Supportticket erstellen und eine Erhöhung beantragen.

### <a name="operating-system-disks-and-images"></a>Betriebssystem-Datenträger und Images
Betriebssystem (Operating System, OS) und Daten werden bei virtuellen Computern auf [virtuellen Festplatten (Virtual Hard Disks, VHDs)](../storage/storage-about-disks-and-vhds-windows.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) gespeichert. VHDs werden auch für die Images verwendet, die Sie auswählen können, um ein Betriebssystem zu installieren. 

Azure bietet zahlreiche [Marketplace-Images](https://azure.microsoft.com/marketplace/virtual-machines/) für verschiedene Versionen und Arten von Windows Server-Betriebssystemen. Marketplace-Images werden anhand von Herausgeber, Angebot, SKU und Version (üblicherweise die aktuelle Version) identifiziert. 

Die folgende Tabelle enthält einige Methoden zur Ermittlung von Informationen zu einem Image:

| Methode | Beschreibung |
| --- | --- |
| Azure-Portal |Die Werte werden automatisch angegeben, wenn Sie ein zu verwendendes Image auswählen. |
| Azure PowerShell |[Get-AzureRMVMImagePublisher](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimagepublisher) -Location "location"<BR>[Get-AzureRMVMImageOffer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimageoffer) -Location "location" -Publisher "publisherName"<BR>[Get-AzureRMVMImageSku](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/get-azurermvmimagesku) -Location "location" -Publisher "publisherName" -Offer "offerName" |
| REST-APIs |[List image publishers](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publishers) (Imageherausgeber auflisten)<BR>[List image offers](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offers) (Imageangebote auflisten)<BR>[List image skus](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus) (Image-SKUs auflisten) |

Sie können auch [ein eigenes Image hochladen und verwenden](virtual-machines-windows-upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). In diesem Fall werden Herausgebername, Angebot und SKU nicht verwendet.

### <a name="extensions"></a>Erweiterungen
Durch [Erweiterungen](virtual-machines-windows-extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) können Sie den Funktionsumfang Ihres virtuellen Computers mittels Konfiguration nach der Bereitstellung sowie mittels automatisierter Aufgaben erweitern.

Mit Erweiterungen können folgende allgemeine Aufgaben umgesetzt werden:

* **Ausführen benutzerdefinierter Skripts:** Die [benutzerdefinierte Skripterweiterung](virtual-machines-windows-extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) unterstützt Sie beim Konfigurieren von Workloads auf dem virtuellen Computer, indem beim Bereitstellen des virtuellen Computers Ihr Skript ausgeführt wird.
* **Bereitstellen und Verwalten von Konfigurationen:** Die [PowerShell-Erweiterung zum Konfigurieren des gewünschten Zustands](virtual-machines-windows-extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) unterstützt Sie beim Einrichten der Konfiguration des gewünschten Zustands auf einem virtuellen Computer, um Konfigurationen und Umgebungen zu verwalten.
* **Sammeln von Diagnosedaten:** Die [Azure-Diagnoseerweiterung](virtual-machines-windows-extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) unterstützt Sie dabei, den virtuellen Computer für die Sammlung von Diagnosedaten zu konfigurieren, die zum Überwachen der Integrität Ihrer Anwendung verwendet werden können.

### <a name="related-resources"></a>Zugehörige Ressourcen
Die Ressourcen in der folgenden Tabelle werden vom virtuellen Computer verwendet und müssen beim Erstellen des virtuellen Computers vorhanden sein oder erstellt werden:

| Ressource | Erforderlich | Beschreibung |
| --- | --- | --- |
| [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) |Ja |Der virtuelle Computer muss sich in einer Ressourcengruppe befinden. |
| [Speicherkonto](../storage/storage-create-storage-account.md) |Ja |Der virtuelle Computer benötigt das Speicherkonto zum Speichern der virtuellen Festplatten. |
| [Virtuelles Netzwerk](../virtual-network/virtual-networks-overview.md) |Ja |Der virtuelle Computer muss einem virtuellen Netzwerk angehören. |
| [Öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) |Nein |Für den Remotezugriff kann dem virtuellen Computer eine öffentliche IP-Adresse zugewiesen werden. |
| [Netzwerkschnittstelle](../virtual-network/virtual-network-network-interface.md) |Ja |Der virtuelle Computer benötigt die Netzwerkschnittstelle für die Kommunikation im Netzwerk. |
| [Datenträger](virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |Nein |Der virtuelle Computer kann Datenträger enthalten, um die Speicherkapazität zu erhöhen. |

## <a name="how-do-i-create-my-first-vm"></a>Wie erstelle ich meinen ersten virtuellen Computer?
Ein virtueller Computer kann auf unterschiedliche Weise erstellt werden. Die Vorgehensweise hängt von Ihrer Umgebung ab. 

Die folgende Tabelle enthält hilfreiche Informationen zur Erstellung Ihres ersten virtuellen Computers:

| Methode | Artikel |
| --- | --- |
| Azure-Portal |[Create a virtual machine running Windows using the portal (Erstellen eines virtuellen Windows-Computers mithilfe des Portals)](virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Vorlagen |[Erstellen Sie einen virtuellen Windows-Computer mit einer Resource Manager-Vorlage](virtual-machines-windows-ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Azure PowerShell |[Create a Windows VM using PowerShell (Erstellen eines virtuellen Windows-Computers mithilfe von PowerShell)](virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Client-SDKs |[Bereitstellen von Azure-Ressourcen mit C#](virtual-machines-windows-csharp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| REST-APIs |[Erstellen oder Aktualisieren eines virtuellen Computers](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-create-or-update) |

Man hofft zwar immer, dass es nicht dazu kommt, gelegentlich tritt dann aber doch der eine oder andere Fehler auf. In diesem Fall finden Sie unter [Behandeln von Problemen beim Erstellen eines neuen virtuellen Windows-Computers in Azure (Resource Manager-Bereitstellungsmodell)](virtual-machines-windows-troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hilfreiche Informationen.

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Wie verwalte ich den erstellten virtuellen Computer?
Virtuelle Computer können über ein browserbasiertes Portal, über Befehlszeilentools mit Skriptunterstützung oder direkt über APIs verwaltet werden. Gängige Verwaltungsaufgaben sind das Abrufen von Informationen zu einem virtuellen Computer, das Anmelden bei einem virtuellen Computer, das Verwalten der Verfügbarkeit und die Durchführung von Sicherungen.

### <a name="get-information-about-a-vm"></a>Abrufen von Informationen zu einem virtuellen Computer
Die folgende Tabelle enthält einige der Methoden, mit denen Sie Informationen zu einem virtuellen Computer abrufen können:

| Methode | Beschreibung |
| --- | --- |
| Azure-Portal |Klicken Sie im Hub-Menü auf **Virtual Machines**, und wählen Sie dann in der Liste den gewünschten virtuellen Computer aus. Auf dem Blatt für den virtuellen Computer finden Sie Übersichtsinformationen, Einstellungswerte und Überwachungsmetriken. |
| Azure PowerShell |Informationen zum Verwalten von virtuellen Computern mithilfe von PowerShell finden Sie unter [Verwalten von virtuellen Azure-Computern mit Resource Manager und PowerShell](virtual-machines-windows-ps-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| REST-API |Verwenden Sie den Vorgang [Get VM information](https://docs.microsoft.com/rest/api/compute/virtualmachines/virtualmachines-get) (VM-Informationen abrufen), um Informationen zu einem virtuellen Computer abzurufen. |
| Client-SDKs |Informationen zum Verwalten von virtuellen Computern mithilfe von C# finden Sie unter [Verwalten von virtuellen Azure-Computern mit Azure Resource Manager und C#](virtual-machines-windows-csharp-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |

### <a name="log-on-to-the-vm"></a>Anmelden beim virtuellen Computer
Verwenden Sie zum [Starten einer Remotedesktopsitzung](virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) die Schaltfläche „Verbinden“ im Azure-Portal. Bei Remoteverbindungen können gelegentlich Probleme auftreten. In diesem Fall finden Sie unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hilfreiche Informationen.

### <a name="manage-availability"></a>Verwalten der Verfügbarkeit
Informieren Sie sich darüber, wie Sie eine [hohe Verfügbarkeit für Ihre Anwendung sicherstellen](virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Diese Konfiguration umfasst das Erstellen mehrerer virtueller Computer, um zu gewährleisten, dass mindestens einer davon aktiv ist.

Damit Ihre Bereitstellung den Verfügbarkeitswert von 99,95 Prozent unserer VM-Vereinbarung zum Servicelevel erreicht, müssen Sie mindestens zwei virtuelle Computer bereitstellen, die Ihre Workload innerhalb einer [Verfügbarkeitsgruppe](virtual-machines-windows-infrastructure-availability-sets-guidelines.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ausführen. So wird sichergestellt, dass Ihre virtuellen Computer auf mehrere Fehlerdomänen verteilt und auf Hosts mit unterschiedlichen Wartungsfenstern bereitgestellt werden. Die vollständige [Azure-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) erläutert die garantierte Verfügbarkeit von Azure insgesamt.

### <a name="back-up-the-vm"></a>Sichern des virtuellen Computers
Ein [Recovery Services-Tresor](../backup/backup-introduction-to-azure-backup.md) dient zum Schutz von Daten und Assets in Azure Backup und Azure Site Recovery. Mit einem Recovery Services-Tresor können Sie [Sicherungen für mit Resource Manager bereitgestellte virtuelle Computer mithilfe von PowerShell bereitstellen und verwalten](../backup/backup-azure-vms-automation.md). 

## <a name="next-steps"></a>Nächste Schritte
* Wenn Sie virtuelle Linux-Computer verwenden möchten, sehen Sie sich [Azure und Linux](virtual-machines-linux-azure-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) an.
* Weitere Informationen zu den Richtlinien im Zusammenhang mit der Einrichtung Ihrer Infrastruktur finden Sie unter [Exemplarische Vorgehensweise für eine Azure-Beispielinfrastruktur](virtual-machines-windows-infrastructure-example.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Bewährte Methoden für das Ausführen eines virtuellen Windows-Computers in Azure finden Sie [hier](virtual-machines-windows-guidance-compute-single-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


