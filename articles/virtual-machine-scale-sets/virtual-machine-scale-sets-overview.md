---
title: "Übersicht über Azure-VM-Skalierungsgruppen | Microsoft-Dokumentation"
description: "Erfahren Sie mehr über Azure-VM-Skalierungsgruppen."
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
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 2071debe24ef2705b2ee05ce1210a813234cf277
ms.lasthandoff: 03/10/2017


---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Was sind VM-Skalierungsgruppen in Azure?
VM-Skalierungsgruppen sind eine Azure-Computeressource, mit der Sie eine Gruppe von identischen virtuellen Computern bereitstellen und verwalten können. Alle virtuellen Computer in einer Skalierungsgruppe sind gleich konfiguriert. Dadurch ermöglichen Skalierungsgruppen eine echte automatische Skalierung. Eine Vorabbereitstellung der virtuellen Computer ist dabei nicht erforderlich. Dies erleichtert die Erstellung leistungsfähiger Dienste, die auf große Rechenleistung, Big Data und Workloads in Containern ausgerichtet sind.

Bei Anwendungen, für die Computeressourcen horizontal herunter- und hochskaliert werden müssen, werden Skalierungsoperationen implizit über Fehler- und Updatedomänen ausgeglichen. Eine Einführung in das Thema „Skalierungsgruppen“ finden Sie in [dieser Ankündigung im Azure-Blog](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Die folgenden Videos enthalten weitere Informationen zu Skalierungsgruppen:

* [Mark Russinovich spricht über Azure-Skalierungsgruppen](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [VM-Skalierungsgruppen mit Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-scale-sets"></a>Erstellen und Verwalten von Skalierungsgruppen
Erstellen Sie im [Azure-Portal](https://portal.azure.com) eine Skalierungsgruppe, indem Sie die Option *Neu* wählen und „Skalierung“ in die Suchleiste eingeben. Die „VM-Skalierungsgruppe“ wird in den Ergebnissen aufgeführt. Sie können anschließend die Pflichtfelder ausfüllen, um Ihre Skalierungsgruppe anzupassen und bereitzustellen. Beachten Sie, dass im Portal auch Optionen zum Einrichten von grundlegenden Regeln für die automatische Skalierung basierend auf der CPU-Nutzung enthalten sind.

Skalierungsgruppen können auch mithilfe von JSON-Vorlagen und [REST-APIs](https://msdn.microsoft.com/library/mt589023.aspx) genauso wie einzelne virtuelle Azure Resource Manager-Computer definiert und bereitgestellt werden. Daher können alle standardmäßigen Azure-Ressourcen-Manager-Bereitstellungsmethoden verwendet werden. Weitere Informationen zu Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../azure-resource-manager/resource-group-authoring-templates.md).

Eine Reihe von Beispielvorlagen für VM-Skalierungsgruppen finden Sie [hier](https://github.com/Azure/azure-quickstart-templates) im GitHub-Repository für Azure-Schnellstartvorlagen (suchen Sie nach Vorlagen mit *vmss* im Titel).

Auf den Detailseiten für diese Vorlagen finden Sie eine Schaltfläche, die eine Verknüpfung mit dem Bereitstellungsfeature des Portals herstellt. Zum Bereitstellen der Skalierungsgruppe klicken Sie auf diese Schaltfläche und geben dann alle Parameter an, die im Portal erforderlich sind. Wenn Sie nicht wissen, ob eine Ressource Großbuchstaben oder eine gemischte Groß- und Kleinschreibung unterstützt, ist es sicherer, für die Parameterwerte nur Kleinbuchstaben und Zahlen zu verwenden. Es ist auch ein anschauliches Video mit der Analyse einer Skalierungsgruppenvorlage verfügbar:

[Analyse einer VM-Skalierungsgruppenvorlage](https://channel9.msdn.com/Blogs/Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-scale-set-out-and-in"></a>Horizontales Hoch- und Herunterskalieren einer Skalierungsgruppe
Sie können die Kapazität einer Skalierungsgruppe im Azure-Portal ändern, indem Sie unter _Einstellungen_ auf den Abschnitt _Skalierung_ klicken. 

Wenn Sie die Kapazität einer Skalierungsgruppe über die Befehlszeile ändern möchten, können Sie den Befehl _scale_ der [Azure-CLI](https://github.com/Azure/azure-cli) verwenden. Gehen Sie beispielsweise wie folgt vor, um für eine Skalierungsgruppe eine Kapazität von zehn VMs festzulegen:

```bash
az vmss scale -g resourcegroupname -n scalesetname --new-capacity 10 
```

Verwenden Sie zum Festlegen der Anzahl von VMs in einer Skalierungsgruppe mit PowerShell den Befehl _Update-AzureRmVmss_:

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName resourcegroupname -VMScaleSetName scalesetname  
$vmss.Sku.Capacity = 10
Update-AzureRmVmss -ResourceGroupName resourcegroupname -Name scalesetname -VirtualMachineScaleSet $vmss
```

Wenn Sie die Anzahl von virtuellen Computern in einer Skalierungsgruppe mit einer Azure Resource Manager-Vorlage erhöhen oder verringern möchten, können Sie die *capacity*-Eigenschaft ändern und die Vorlage erneut bereitstellen. Diese einfache Vorgehensweise erleichtert es Ihnen, Skalierungsgruppen in die automatische Skalierung von Azure zu integrieren oder eine eigene benutzerdefinierte Skalierungsschicht zu schreiben, falls Sie benutzerdefinierte Skalierungsereignisse definieren müssen, die von der automatischen Skalierung in Azure nicht unterstützt werden. 

Wenn Sie eine Azure Resource Manager-Vorlage erneut bereitstellen, um die Kapazität zu erhöhen, können Sie dafür eine viel kleinere Vorlage definieren, die nur das „SKU“-Eigenschaftspaket mit der geänderten Kapazität enthält. Ein Beispiel hierzu finden Sie [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing).

## <a name="autoscale"></a>Autoscale

Eine Skalierungsgruppe kann optional mit Einstellungen der automatischen Skalierung konfiguriert werden, wenn sie im Azure-Portal erstellt wird, sodass die Anzahl von VMs basierend auf der durchschnittlichen CPU-Auslastung erhöht oder verringert werden kann. Für viele Skalierungsgruppenvorlagen in [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates) werden Einstellungen für die automatische Skalierung definiert. Außerdem können Sie Einstellungen für die automatische Skalierung einer vorhandenen Skalierungsgruppe hinzufügen. Hier ist beispielsweise ein Azure PowerShell-Skript zum Hinzufügen der CPU-basierten automatischen Skalierung zu einer Skalierungsgruppe angegeben:

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

 Hier finden Sie eine Liste mit gültigen Metriken für die Skalierung: [Unterstützte Metriken von Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md) (unter der Überschrift _Microsoft.Compute/virtualMachineScaleSets_). Es sind auch erweiterte Optionen für die automatische Skalierung verfügbar, z.B. für die automatische Skalierung nach Zeitplan, und Sie können Webhooks zum Integrieren in Warnsysteme verwenden.

## <a name="monitoring-your-scale-set"></a>Überwachen der Skalierungsgruppe
Im [Azure-Portal](https://portal.azure.com) werden die Skalierungsgruppen und die dazugehörigen Eigenschaften aufgeführt. Im Portal werden auch Verwaltungsvorgänge unterstützt, die sowohl für Skalierungsgruppen als auch für einzelne VMs in einer Skalierungsgruppe durchgeführt werden können. Darüber hinaus wird im Portal ein Graph zur Ressourcennutzung bereitgestellt, den Sie anpassen können. Wenn Sie die zugrunde liegende JSON-Definition einer Azure-Ressource anzeigen oder bearbeiten möchten, können Sie auch den [Azure-Ressourcen-Explorer](https://resources.azure.com) verwenden. Bei Skalierungsgruppen handelt es sich um Ressourcen unter dem Azure-Ressourcenanbieter „Microsoft.Compute“. Sie können sie daher anzeigen, indem Sie auf dieser Website die folgenden Links öffnen:

**Abonnements > Ihr Abonnement > resourceGroups > providers > Microsoft.Compute > virtualMachineScaleSets > Ihre Skalierungsgruppe > usw.**

## <a name="scale-set-scenarios"></a>Szenarien für Skalierungsgruppen
Dieser Abschnitt enthält einige typische Szenarien für Skalierungsgruppen. Einige auf höherer Ebene angesiedelte Azure-Dienste (wie Batch, Service Fabric und Azure Container Service) verwenden diese Szenarien.

* **Zugriff mit RDP oder SSH auf Skalierungsgruppeninstanzen**: Eine Skalierungsgruppe wird in einem VNet erstellt, und den einzelnen virtuellen Computern in der Skalierungsgruppe werden keine öffentlichen IP-Adressen zugeordnet. Mit dieser Richtlinie werden Kosten und Verwaltungsvorgänge vermieden, die anfallen, wenn allen Knoten im Computenetz separate öffentliche IP-Adressen zugeordnet werden. Sie können von anderen Ressourcen in Ihrem VNET eine Verbindung mit diesen VMs herstellen, z.B. Lastenausgleichsmodulen und eigenständigen virtuellen Computern, denen öffentliche IP-Adressen zugeordnet werden können.
* **Herstellen einer Verbindung mit virtuellen Computern mithilfe von NAT-Regeln**: Sie können eine öffentliche IP-Adresse erstellen, sie einem Lastenausgleich zuweisen und einen NAT-Pool für eingehenden Datenverkehr definieren, über den Ports der IP-Adresse einem Port auf dem virtuellen Computer in der Skalierungsgruppe zugeordnet werden. Beispiel:
  
  | Quelle | Quellport | Ziel | Zielport |
  | --- | --- | --- | --- |
  |  Öffentliche IP |Port 50000 |vmss\_0 |Port 22 |
  |  Öffentliche IP |Port 50001 |vmss\_1 |Port 22 |
  |  Öffentliche IP |Port 50002 |vmss\_2 |Port 22 |
  
   In diesem Beispiel werden NAT-Regeln definiert, um eine SSH-Verbindung (Secure Shell) mit jedem virtuellen Computer in einer Skalierungsgruppe über nur eine öffentliche IP-Adresse zu ermöglichen: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)
  
   Hier ist ein Beispiel, bei dem dasselbe mit RDP (Remote Desktop Protocol) und Windows erreicht wird: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)
* **Herstellen einer Verbindung mit VMs mithilfe einer „Jumpbox“**: Wenn Sie eine Skalierungsgruppe und einen eigenständigen virtuellen Computer in demselben VNET erstellen, können der eigenständige virtuelle Computer und die virtuellen Computer in der Skalierungsgruppe eine Verbindung miteinander herstellen, indem sie ihre internen IP-Adressen verwenden, die durch das VNET/Subnetz definiert sind. Wenn Sie eine öffentliche IP-Adresse erstellen und dem eigenständigen virtuellen Computer zuweisen, können Sie per RDP oder SSH eine Verbindung mit dem eigenständigen virtuellen Computer herstellen. Anschließend können Sie von diesem Computer aus eine Verbindung mit den Skalierungsgruppeninstanzen herstellen. Vielleicht fällt Ihnen hierbei auf, dass eine einfache Skalierungsgruppe inhärent sicherer als ein einfacher eigenständiger virtueller Computer mit einer öffentlichen IP-Adresse in der Standardkonfiguration ist.
  
   Mit dieser Vorlage wird beispielsweise eine einfache Skalierungsgruppe mit einer eigenständigen VM bereitgestellt: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox)
* **Lastenausgleich für Skalierungsgruppeninstanzen**: Wenn Sie eine Workload per Roundrobin an einen Computecluster aus virtuellen Computern übergeben möchten, können Sie einen Azure Load Balancer mit entsprechenden Schicht&4;-Lastenausgleichsregeln konfigurieren. Sie können Tests definieren, um zu überprüfen, ob Ihre Anwendung richtig ausgeführt wird. Bei diesen Tests werden die Ports nach einem festgelegten Protokoll in vorgegebenen Zeitabständen und mit einem angegebenen Anforderungspfad per Ping abgefragt. Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) unterstützt darüber hinaus Skalierungsgruppen sowie Schicht&7; und anspruchsvollere Lastenausgleichsszenarien.
  
   In diesem Beispiel wird eine Skalierungsgruppe mit Ausführung von Apache-Webservern erstellt und ein Lastenausgleich verwendet, um die Last für die einzelnen VMs zu verteilen: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl). (Achten Sie auf den Ressourcentyp „Microsoft.Network/loadBalancers“ und „networkProfile“ und „extensionProfile“ in „virtualMachineScaleSet“.)

   In diesem Beispiel wird ein Application Gateway verwendet. Linux:  [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway) Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway) 

* **Bereitstellen einer Skalierungsgruppe als Computecluster in einem PaaS-Cluster-Manager**: Skalierungsgruppen werden mitunter als Workerrolle der nächsten Generation beschrieben. Dies ist zwar eine zutreffende Beschreibung, aber es kann passieren, dass die Features von Skalierungsgruppen mit den Features von Azure Cloud Services verwechselt werden. In gewisser Weise stellen Skalierungsgruppen tatsächlich eine „Workerrolle“ oder eine Workerressource bereit: Es handelt sich um eine generalisierte Computeressource, die von Plattformen und Runtimeumgebungen unabhängig ist, sich anpassen lässt und in die Azure Resource Manager-IaaS integriert ist.
  
   Eine Cloud Services-Workerrolle ist zwar im Hinblick auf die Unterstützung für Plattformen/Laufzeitumgebungen eingeschränkt (nur für Images der Windows-Plattform), aber sie bietet auch Dienste wie VIP-Austausch, konfigurierbare Upgradeeinstellungen und Einstellungen für laufzeitumgebungs- und anwendungsspezifische Bereitstellungen, die entweder *noch* nicht in Skalierungsgruppen zur Verfügung stehen oder die durch auf höherer Ebene angesiedelte PaaS-Dienste wie Service Fabric umgesetzt werden. Sie können sich Skalierungsgruppen als Infrastruktur vorstellen, die PaaS unterstützt. PaaS-Lösungen wie [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) bauen auf dieser Infrastruktur auf.
  
   In einem Beispiel für diesen Ansatz stellt der [Azure Container Service](https://azure.microsoft.com/services/container-service/) einen Cluster basierend auf Skalierungsgruppen mit einem „Orchestrator“ für Container bereit: [https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos).

## <a name="scale-set-performance-and-scale-guidance"></a>Anleitung zur Leistungsfähigkeit und Skalierung von Skalierungsgruppen
* Für Skalierungsgruppen wird die Verwendung von bis zu 1.000 VMs unterstützt. Wenn Sie eigene benutzerdefinierte VM-Images erstellen und hochladen, beträgt der Grenzwert 100. Informationen zu den Aspekten, die bei der Nutzung großer Skalierungsgruppen beachtet werden sollten, finden Sie unter [Verwenden umfangreicher VM-Skalierungsgruppen](virtual-machine-scale-sets-placement-groups.md).
* Für die Verwendung von Skalierungsgruppen müssen Sie vorab keine Azure-Speicherkonten erstellen. Da Skalierungsgruppen Azure Managed Disks unterstützen, sind Leistungsbedenken in Bezug auf die Anzahl von Datenträgern pro Speicherkonto kein Thema. Weitere Informationen finden Sie unter [Azure-VM-Skalierungsgruppen und verwaltete Datenträger](virtual-machine-scale-sets-managed-disks.md).
* Erwägen Sie die Verwendung von Azure Storage Premium-Speicher anstelle von Speicher vom Typ „Standard“, um eine schnellere und besser vorhersagbare VM-Bereitstellung und höhere E/A-Leistung zu erreichen.
* Die Anzahl von VMs, die Sie erstellen können, wird durch das Kernkontingent in der Region beschränkt, in der Sie sie bereitstellen. Unter Umständen müssen Sie sich an den Kundensupport wenden und das Limit im Computekontingent auch dann erhöhen lassen, wenn Sie bereits über ein hohes Limit an Kernen verfügen, die für Azure-Clouddienste genutzt werden können. Sie können Ihr Kontingent mit diesem Befehl der Azure-CLI abfragen: `azure vm list-usage`. Oder verwenden Sie den folgenden PowerShell-Befehl: `Get-AzureRmVMUsage`. (Falls Sie eine ältere Version als PowerShell 1.0 nutzen, verwenden Sie `Get-AzureVMUsage`.)

## <a name="scale-set-frequently-asked-questions"></a>Skalierungsgruppen – Häufig gestellte Fragen
**F.** Wie viele virtuelle Computer können in eine Skalierungsgruppe aufgenommen werden?

**A.** Eine Skalierungsgruppe kann zwischen 0 und 1.000 VMs basierend auf Plattformimages oder 0 bis 100 VMs basierend auf benutzerdefinierten Images enthalten. 

**F.** Werden Datenträger innerhalb von Skalierungsgruppen unterstützt?

**A.** Ja. Für eine Skalierungsgruppe kann eine Konfiguration für angefügte Datenträger definiert werden, die für alle VMs der Gruppe gilt. Weitere Informationen finden Sie unter (Azure scale sets and attached data disks)[virtual-machine-scale-sets-attached-disks.md] (Azure-Skalierungsgruppen und angefügte Datenträger). Beispiele für andere Optionen zum Speichern von Daten:

* Azure-Dateien (SMB-freigegebene Laufwerke)
* Betriebssystemlaufwerk
* Temporäres Laufwerk (lokal, nicht durch Azure Storage gesichert) 
* Azure-Datendienst (z.B. Azure-Tabellen, Azure-Blobs)
* Externer Datendienst (z.B. Remotedatenbank)

**F.** In welchen Azure-Regionen werden Skalierungsgruppen unterstützt?

**A.** Alle Regionen unterstützen Skalierungsgruppen.

**F.** Wie erstellen Sie eine Skalierungsgruppe mit einem benutzerdefinierten Image?

**A.** Erstellen Sie einen verwalteten Datenträger basierend auf Ihrer VHD mit benutzerdefiniertem Image, und verweisen Sie in Ihrer Skalierungsgruppenvorlage darauf. Hier finden Sie ein Beispiel: [https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os).

**F.** Welche virtuellen Computer werden entfernt, wenn ich meine Kapazität für Skalierungsgruppen von 20 auf 15 verringere?

**A.** Die Entfernung der virtuellen Computer aus den Skalierungsgruppen erfolgt gleichmäßig über Upgrade- und Fehlerdomänen hinweg, um die Verfügbarkeit zu maximieren. Die VMs mit den höchsten IDs werden zuerst entfernt.

**F.** Was passiert, wenn ich dann die Kapazität von 15 auf 18 erhöhe?

**A.** Wenn Sie die Kapazität auf 18 erhöhen, werden drei neue VMs erstellt. Die VM-Instanz-ID wird jeweils ausgehend vom vorherigen höchsten Wert erhöht (z.B. 20, 21, 22). Die virtuellen Computer werden gleichmäßig über Fehlerdomänen (FDs) und Updatedomänen (UDs) verteilt.

**F.** Kann ich beim Verwenden mehrerer Erweiterungen in einer Skalierungsgruppe eine Ausführungsreihenfolge erzwingen?

**A.** Nicht direkt. Bei der customScript-Erweiterung kann das Skript jedoch warten, bis eine andere Erweiterung abgeschlossen ist ([z.B. durch Überwachung des Erweiterungsprotokolls](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Weitere Informationen zur Erweiterungssequenzierung finden Sie im folgenden Blogbeitrag: [Extension Sequencing in Azure VM Scale Sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/) (Erweiterungssequenzierung in Skalierungsgruppen mit virtuellen Azure-Computern).

**F.** Funktionieren Skalierungsgruppen mit Azure-Verfügbarkeitsgruppen?

**A.** Ja. Eine Skalierungsgruppe ist eine implizite Verfügbarkeitsgruppe mit fünf Fehlerdomänen (FDs) und fünf Updatedomänen (UDs). Skalierungsgruppen mit mehr als 100 VMs gelten über mehrere „Platzierungsgruppen“ hinweg, die mehreren Verfügbarkeitsgruppen entsprechen. Weitere Informationen zu Platzierungsgruppen finden Sie unter [Verwenden umfangreicher VM-Skalierungsgruppen](virtual-machine-scale-sets-placement-groups.md). Eine Verfügbarkeitsgruppe mit VMs kann in demselben VNet wie eine Skalierungsgruppe mit VMs vorhanden sein. Ein häufiger Konfigurationsansatz besteht darin, Steuerknoten-VMs – für die häufig eine eindeutige Konfiguration erforderlich ist – in einer Verfügbarkeitsgruppe und Datenknoten in der Skalierungsgruppe anzuordnen.

Weitere häufig gestellte Fragen zu Skalierungsgruppen finden Sie unter [Azure Virtual Machine Scale Sets FAQ](virtual-machine-scale-sets-faq.md) (Azure-VM-Skalierungsgruppen – Häufig gestellte Fragen).

