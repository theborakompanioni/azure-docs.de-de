---
title: "Übersicht über Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation"
description: "Hier erfahren Sie mehr über Azure-VM-Skalierungsgruppen."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/10/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 14a5da0430b4eaaa61ef875e59454e2b6d88de91
ms.lasthandoff: 03/31/2017


---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Was sind VM-Skalierungsgruppen in Azure?
VM-Skalierungsgruppen sind eine Azure-Computeressource, mit der Sie eine Gruppe von identischen virtuellen Computern bereitstellen und verwalten können. Da alle virtuellen Computer in einer Skalierungsgruppe gleich konfiguriert sind, ermöglichen Skalierungsgruppen eine echte automatische Skalierung ohne Vorabbereitstellung virtueller Computer. Das erleichtert die Erstellung umfangreicher Dienste, die auf hohe Rechenleistung, Big Data und Workloads in Containern ausgelegt sind.

Bei Anwendungen, für die Computeressourcen horizontal herunter- und hochskaliert werden müssen, werden Skalierungsoperationen implizit über Fehler- und Updatedomänen ausgeglichen. Eine weitergehende Einführung in das Thema „Skalierungsgruppen“ finden Sie in [dieser Ankündigung im Azure-Blog](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Die folgenden Videos enthalten weitere Informationen zu Skalierungsgruppen:

* [Mark Russinovich spricht über Azure-Skalierungsgruppen](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [VM-Skalierungsgruppen mit Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-scale-sets"></a>Erstellen und Verwalten von Skalierungsgruppen
Eine Skalierungsgruppe können Sie im [Azure-Portal](https://portal.azure.com) wie folgt erstellen: Wählen Sie **Neu** aus, und geben Sie **Skalierung** in die Suchleiste ein. In den Ergebnissen wird **VM-Skalierungsgruppe** aufgeführt. Sie können anschließend die Pflichtfelder ausfüllen, um Ihre Skalierungsgruppe anzupassen und bereitzustellen. Im Portal stehen auch Optionen zum Einrichten grundlegender Regeln für die automatische Skalierung auf der Grundlage der CPU-Auslastung zur Verfügung.

VM-Skalierungsgruppen können genau wie einzelne virtuelle Azure Resource Manager-Computer mithilfe von JSON-Vorlagen und [REST-APIs](https://msdn.microsoft.com/library/mt589023.aspx) definiert und bereitgestellt werden. Ihnen stehen daher alle Standardbereitstellungsmethoden von Azure Resource Manager zur Verfügung. Weitere Informationen zu Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

Eine Reihe von Beispielvorlagen für VM-Skalierungsgruppen finden Sie im [GitHub-Repository für Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates). (Suchen Sie nach Vorlagen mit **vmss** im Titel.)

Über eine Schaltfläche gelangen Sie auf den Detailseiten für diese Vorlagen zum Bereitstellungsfeature des Portals. Zum Bereitstellen der Skalierungsgruppe klicken Sie auf diese Schaltfläche und geben dann alle Parameter an, die im Portal erforderlich sind. Wenn Sie nicht wissen, ob eine Ressource Großbuchstaben oder eine gemischte Groß- und Kleinschreibung unterstützt, ist es sicherer, für die Parameterwerte nur Kleinbuchstaben und Zahlen zu verwenden. Unter [VM Scale Set Template Dissection](https://channel9.msdn.com/Blogs/Azure/VM-Scale-Set-Template-Dissection/player) (Analyse einer VM-Skalierungsgruppenvorlage) finden Sie eine anschauliche Videoanalyse einer Skalierungsgruppenvorlage.

## <a name="scaling-a-scale-set-out-and-in"></a>Horizontales Hoch- und Herunterskalieren einer Skalierungsgruppe
Sie können die Kapazität einer Skalierungsgruppe im Azure-Portal ändern, indem Sie unter **Einstellungen** auf den Abschnitt **Skalierung** klicken. 

Wenn Sie die Kapazität einer Skalierungsgruppe über die Befehlszeile ändern möchten, können Sie den Befehl **scale** der [Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-cli) verwenden. Verwenden Sie beispielsweise den folgenden Befehl, um für eine Skalierungsgruppe eine Kapazität von zehn virtuellen Computern festzulegen:

```bash
az vmss scale -g resourcegroupname -n scalesetname --new-capacity 10 
```

Wenn Sie die Anzahl virtueller Computer in einer Skalierungsgruppe mithilfe von PowerShell festlegen möchten, verwenden Sie den Befehl **Update-AzureRmVmss**:

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName resourcegroupname -VMScaleSetName scalesetname  
$vmss.Sku.Capacity = 10
Update-AzureRmVmss -ResourceGroupName resourcegroupname -Name scalesetname -VirtualMachineScaleSet $vmss
```

Wenn Sie die Anzahl virtueller Computern in einer Skalierungsgruppe mit einer Azure Resource Manager-Vorlage erhöhen oder verringern möchten, können Sie die Eigenschaft **capacity** ändern und die Vorlage erneut bereitstellen. Diese einfache Vorgehensweise erleichtert es Ihnen, Skalierungsgruppen in die automatische Skalierung von Azure zu integrieren oder eine eigene benutzerdefinierte Skalierungsschicht zu schreiben, falls Sie benutzerdefinierte Skalierungsereignisse definieren müssen, die die automatische Skalierung in Azure nicht unterstützt. 

Wenn Sie eine Azure Resource Manager-Vorlage erneut bereitstellen, um die Kapazität zu ändern, können Sie eine viel kleinere Vorlage definieren, die nur das **SKU**-Eigenschaftspaket mit der aktualisierten Kapazität enthält. Ein entsprechendes Beispiel finden Sie [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing).

## <a name="autoscale"></a>Autoscale

Eine Skalierungsgruppe kann bei der Erstellung im Azure-Portal optional mit Einstellungen für die automatische Skalierung konfiguriert werden. Die Anzahl virtueller Computer kann dann auf der Grundlage der CPU-Auslastung erhöht oder verringert werden. 

Viele der Skalierungsgruppenvorlagen in den [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates) definieren Einstellungen für die automatische Skalierung. Außerdem können Sie Einstellungen für die automatische Skalierung einer vorhandenen Skalierungsgruppe hinzufügen. Das folgende Azure PowerShell-Skript fügt beispielsweise einer Skalierungsgruppe eine CPU-basierte automatische Skalierung hinzu:

```PowerShell

$subid = "yoursubscriptionid"
$rgname = "yourresourcegroup"
$vmssname = "yourscalesetname"
$location = "yourlocation" # e.g. southcentralus

$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Increase -ScaleActionValue 1
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator LessThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Decrease -ScaleActionValue 1
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "autoprofile1"
Add-AzureRmAutoscaleSetting -Location $location -Name "autosetting1" -ResourceGroup $rgname -TargetResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -AutoscaleProfiles $profile1
```

 Eine Liste mit gültigen Metriken für die Skalierung finden Sie unter [Unterstützte Metriken von Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md) (unter der Überschrift „Microsoft.Compute/virtualMachineScaleSets“). Es sind auch komplexere Optionen für die automatische Skalierung verfügbar – etwa für die automatische Skalierung nach Zeitplan und für die Verwendung von Webhooks zur Integration in Warnsysteme.

## <a name="monitoring-your-scale-set"></a>Überwachen der Skalierungsgruppe
Im [Azure-Portal](https://portal.azure.com) werden die Skalierungsgruppen und die dazugehörigen Eigenschaften aufgeführt. Darüber hinaus unterstützt das Portal auch Verwaltungsvorgänge. Sie können Verwaltungsvorgänge sowohl für Skalierungsgruppen als auch für einzelne virtuelle Computer innerhalb einer Skalierungsgruppe ausführen. Darüber hinaus wird im Portal ein Graph zur Ressourcennutzung bereitgestellt, den Sie anpassen können. 

Zum Anzeigen oder Bearbeiten der zugrunde liegenden JSON-Definition einer Azure-Ressource können Sie auch den [Azure-Ressourcen-Explorer](https://resources.azure.com) verwenden. Bei Skalierungsgruppen handelt es sich um eine Ressource unter dem Azure-Ressourcenanbieter „Microsoft.Compute“. Dort können Sie sie durch Erweitern folgender Links anzeigen:

**Abonnements** > **Ihr Abonnement** > **resourceGroups** > **providers** > **Microsoft.Compute** > **virtualMachineScaleSets** > **Ihre Skalierungsgruppe** > usw.

## <a name="scale-set-scenarios"></a>Szenarien für Skalierungsgruppen
Dieser Abschnitt enthält einige typische Szenarien für Skalierungsgruppen. Einige auf höherer Ebene angesiedelte Azure-Dienste (wie Batch, Service Fabric und Azure Container Service) verwenden diese Szenarien.

* **Herstellen einer Verbindung mit Skalierungsgruppeninstanzen mithilfe von RDP oder SSH:** Eine Skalierungsgruppe wird in einem virtuellen Netzwerk erstellt, und den einzelnen virtuellen Computern in der Skalierungsgruppe werden keine öffentlichen IP-Adressen zugeordnet. Mit dieser Richtlinie werden Kosten und Verwaltungsvorgänge vermieden, die anfallen, wenn allen Knoten im Computenetz separate öffentliche IP-Adressen zugeordnet werden. Sie können über andere Ressourcen in Ihrem virtuellen Netzwerk (beispielsweise Load Balancer und eigenständige virtuelle Computer), denen öffentliche IP-Adressen zugeordnet werden können, eine Verbindung mit diesen virtuellen Computern herstellen.
* **Herstellen einer Verbindung mit virtuellen Computern mithilfe von NAT-Regeln:** Sie können eine öffentliche IP-Adresse erstellen, sie einem Load Balancer zuweisen und einen NAT-Pool für eingehenden Datenverkehr definieren. Dadurch werden Ports der IP-Adresse einem Port auf einem virtuellen Computer in der Skalierungsgruppe zugeordnet. Beispiel:
  
  | Quelle | Quellport | Ziel | Zielport |
  | --- | --- | --- | --- |
  |  Öffentliche IP-Adresse |Port 50000 |vmss\_0 |Port 22 |
  |  Öffentliche IP |Port 50001 |vmss\_1 |Port 22 |
  |  Öffentliche IP |Port 50002 |vmss\_2 |Port 22 |
  
   In [diesem Beispiel](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat) werden NAT-Regeln definiert, um eine SSH-Verbindung (Secure Shell) mit jedem virtuellen Computer in einer Skalierungsgruppe über eine einzelne öffentliche IP-Adresse zu ermöglichen.
  
   In [diesem Beispiel](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat) wird das gleiche Ergebnis mithilfe von RDP und Windows erzielt.
* **Herstellen einer Verbindung mit virtuellen Computern mithilfe einer „Jumpbox“:** Wenn Sie eine Skalierungsgruppe und einen eigenständigen virtuellen Computer im gleichen virtuellen Netzwerk erstellen, können der eigenständige virtuelle Computer und der virtuelle Computer der Skalierungsgruppe über ihre internen IP-Adressen eine Verbindung miteinander herstellen (gemäß Definition durch das virtuelle Netzwerk oder Subnetz). Wenn Sie eine öffentliche IP-Adresse erstellen und sie dem eigenständigen virtuellen Computer zuweisen, können Sie mit RDP oder SSH eine Verbindung mit dem eigenständigen virtuellen Computer herstellen. Anschließend können Sie von diesem Computer aus eine Verbindung mit Ihren VM-Skalierungsgruppeninstanzen herstellen. Wahrscheinlich haben Sie bereits erkannt, dass eine einfache Skalierungsgruppe inhärent sicherer ist als ein einfacher eigenständiger virtueller Computer mit einer öffentlichen IP-Adresse in der Standardkonfiguration.
  
   [Diese Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox) stellt beispielsweise eine einfache Skalierungsgruppe mit einem eigenständigen virtuellen Computer bereit. 
* **Lastenausgleich für Skalierungsgruppeninstanzen:** Wenn Sie eine Workload per Roundrobin an einen Computecluster aus virtuellen Computern übergeben möchten, können Sie eine Azure Load Balancer-Instanz mit entsprechenden Schicht 4-Lastenausgleichsregeln konfigurieren. Sie können Tests definieren, um sich zu vergewissern, dass Ihre Anwendung ausgeführt wird. Bei diesen Tests werden die Ports nach einem festgelegten Protokoll in vorgegebenen Zeitabständen und mit einem angegebenen Anforderungspfad per Ping abgefragt. [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) unterstützt sowohl Skalierungsgruppen als auch Schicht 7 und anspruchsvollere Lastenausgleichsszenarien.
  
   [Dieses Beispiel](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) erstellt eine Skalierungsgruppe, die Apache-Webserver ausführt, und verwendet einen Load Balancer, um die Lasten für die einzelnen virtuellen Computer auszugleichen. (Sehen Sie sich insbesondere den Ressourcentyp „Microsoft.Network/loadBalancers“ sowie „networkProfile“ und „extensionProfile“ in „virtualMachineScaleSet“ an.)

   In [diesem Linux-Beispiel](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway) und [diesem Windows-Beispiel](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway) wird jeweils Application Gateway verwendet.  

* **Bereitstellen einer Skalierungsgruppe als Computecluster in einem PaaS-Cluster-Manager:** Skalierungsgruppen werden mitunter als Workerrolle der nächsten Generation beschrieben. Dies ist zwar eine zutreffende Beschreibung, aber es kann passieren, dass die Features von Skalierungsgruppen mit den Features von Azure Cloud Services verwechselt werden. In gewisser Weise stellen Skalierungsgruppen eine echte Workerrolle oder Workerressource bereit. Sie sind eine generalisierte, plattform-/laufzeitunabhängige Computeressource, die sich anpassen lässt und in die Azure Resource Manager-IaaS integriert wird.
  
   Die Plattform-/Laufzeitunterstützung einer Cloud Services-Workerrolle ist auf Images der Windows-Plattform beschränkt. Sie umfasst aber auch Dienste wie VIP-Austausch, konfigurierbare Upgradeeinstellungen und spezifische Laufzeit-/App-Bereitstellungseinstellungen. Diese Dienste stehen in VM-Skalierungsgruppen *noch nicht* zur Verfügung oder werden durch auf höherer Ebene angesiedelte PaaS-Dienste wie etwa Azure Service Fabric bereitgestellt. Sie können sich Skalierungsgruppen als Infrastruktur vorstellen, die PaaS unterstützt. PaaS-Lösungen wie [Service Fabric](https://azure.microsoft.com/services/service-fabric/) basieren auf dieser Infrastruktur.
  
   In [diesem Beispiel](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos) des Konzepts stellt [Azure Container Service](https://azure.microsoft.com/services/container-service/) einen Cluster auf der Grundlage von Skalierungsgruppen mit einem Containerorchestrator bereit.

## <a name="scale-set-performance-and-scale-guidance"></a>Anleitung zur Leistungsfähigkeit und Skalierung von Skalierungsgruppen
* Eine Skalierungsgruppe unterstützt bis zu 1.000 virtuelle Computer. Wenn Sie eigene benutzerdefinierte VM-Images erstellen und hochladen, beträgt der Grenzwert 100. Informationen zu Aspekten, die bei der Nutzung großer Skalierungsgruppen zu berücksichtigen sind, finden Sie unter [Verwenden umfangreicher VM-Skalierungsgruppen](virtual-machine-scale-sets-placement-groups.md).
* Für die Verwendung von Skalierungsgruppen müssen Sie vorab keine Azure-Speicherkonten erstellen. Da Skalierungsgruppen Azure Managed Disks unterstützen, sind Leistungsbedenken in Bezug auf die Anzahl von Datenträgern pro Speicherkonto kein Thema. Weitere Informationen finden Sie unter [Azure-VM-Skalierungsgruppen und verwaltete Datenträger](virtual-machine-scale-sets-managed-disks.md).
* Erwägen Sie die Verwendung von Azure Storage Premium anstelle von Azure Storage, um eine schnellere und besser vorhersagbare VM-Bereitstellung und eine höhere E/A-Leistung zu erreichen.
* Wie viele virtuelle Computer Sie erstellen können, hängt vom Kernkontingent der Region ab, in der Sie sie bereitstellen. Unter Umständen müssen Sie das Limit für das Computekontingent durch den Kundensupport erhöhen lassen. Das gilt auch, wenn Sie bereits über ein hohes Limit an Kernen verfügen, die für Azure Cloud Services genutzt werden können. Ihr Kontingent können Sie über die Azure-Befehlszeile mithilfe des folgenden Befehls abfragen: `azure vm list-usage`. Sie können aber auch den folgenden PowerShell-Befehl ausführen: `Get-AzureRmVMUsage`.

## <a name="frequently-asked-questions-for-scale-sets"></a>Häufig gestellte Fragen zu Skalierungsgruppen
**F.** Wie viele virtuelle Computer können in einer Skalierungsgruppe enthalten sein?

**A.** Eine Skalierungsgruppe kann zwischen 0 und 1.000 virtuelle Computer (Plattformimages) bzw. 0 bis 100 virtuelle Computer (benutzerdefinierte Images) enthalten. 

**F.** Werden Datenträger in Skalierungsgruppen unterstützt?

**A.** Ja. Für eine Skalierungsgruppe kann eine Konfiguration für angefügte Datenträger definiert werden, die für alle virtuellen Computer der Gruppe gilt. Weitere Informationen finden Sie unter [Azure-VM-Skalierungsgruppen und angefügte Datenträger](virtual-machine-scale-sets-attached-disks.md). Beispiele für andere Optionen zum Speichern von Daten:

* Azure-Dateien (SMB-freigegebene Laufwerke)
* Betriebssystemlaufwerk
* Temporäres Laufwerk (lokal; nicht durch Azure Storage gesichert)
* Azure-Datendienst (z.B. Azure-Tabellen, Azure-Blobs)
* Externer Datendienst (z.B. Remotedatenbank)

**F.** In welchen Azure-Regionen werden Skalierungsgruppen unterstützt?

**A.** Alle Regionen unterstützen Skalierungsgruppen.

**F.** Wie erstelle ich eine Skalierungsgruppe mit einem benutzerdefinierten Image?

**A.** Erstellen Sie einen verwalteten Datenträger auf der Grundlage Ihrer VHD mit benutzerdefiniertem Image, und verweisen Sie in Ihrer Skalierungsgruppenvorlage darauf. Ein entsprechendes Beispiel finden Sie [hier](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os).

**F.** Welche virtuellen Computer werden entfernt, wenn ich meine Kapazität für Skalierungsgruppen von 20 auf 15 verringere?

**A.** Die Entfernung der virtuellen Computer aus den Skalierungsgruppen erfolgt gleichmäßig über Update- und Fehlerdomänen hinweg, um die Verfügbarkeit zu maximieren. Die virtuellen Computer mit den höchsten IDs werden zuerst entfernt.

**F.** Was passiert, wenn ich dann die Kapazität von 15 auf 18 erhöhe?

**A.** Wenn Sie die Kapazität auf 18 erhöhen, werden drei neue VMs erstellt. Die VM-Instanz-ID wird jeweils ausgehend vom vorherigen höchsten Wert erhöht (Beispiel: 20, 21, 22). Virtuelle Computer werden über Fehler- und Updatedomänen hinweg gleichmäßig verteilt.

**F.** Kann ich bei Verwendung mehrerer Erweiterungen in einer Skalierungsgruppe eine Ausführungsreihenfolge erzwingen?

**A.** Nicht direkt. Bei der customScript-Erweiterung kann das Skript jedoch auf den Abschluss einer anderen Erweiterung warten (beispielsweise durch [Überwachen des Erweiterungsprotokolls](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Weitere Informationen zur Erweiterungssequenzierung finden Sie im Blogbeitrag [Extension Sequencing in Azure VM Scale Sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/) (Erweiterungssequenzierung in Azure-VM-Skalierungsgruppen).

**F.** Funktionieren Skalierungsgruppen mit Azure-Verfügbarkeitsgruppen?

**A.** Ja. Eine Skalierungsgruppe ist eine implizite Verfügbarkeitsgruppe mit fünf Fehlerdomänen und fünf Updatedomänen. Skalierungsgruppen mit mehr als 100 virtuellen Computern gelten über mehrere *Platzierungsgruppen* hinweg, die wiederum mehreren Verfügbarkeitsgruppen entsprechen. Weitere Informationen zu Platzierungsgruppen finden Sie unter [Verwenden umfangreicher VM-Skalierungsgruppen](virtual-machine-scale-sets-placement-groups.md). Eine Verfügbarkeitsgruppe mit virtuellen Computern kann sich im gleichen virtuellen Netzwerk befinden wie eine Skalierungsgruppe mit virtuellen Computern. Oftmals werden virtuelle Steuerknotencomputer (für die häufig eine eindeutige Konfiguration erforderlich ist) in einer Verfügbarkeitsgruppe und Datenknoten in der Skalierungsgruppe platziert.

Weitere Antworten auf Fragen zu Skalierungsgruppen finden Sie unter [Häufig gestellte Fragen zu Azure-VM-Skalierungsgruppen](virtual-machine-scale-sets-faq.md).

