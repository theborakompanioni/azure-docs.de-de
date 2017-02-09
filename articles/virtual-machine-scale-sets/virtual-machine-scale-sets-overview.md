---
title: "Übersicht über VM-Skalierungsgruppen | Microsoft Docs"
description: "Erfahren Sie mehr über VM-Skalierungsgruppen"
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
ms.date: 11/15/2016
ms.author: guybo
translationtype: Human Translation
ms.sourcegitcommit: 3ed7c49603891b9719819143003d27888f800a95
ms.openlocfilehash: 21a7feb9eb2588643ffc67408de9f8e60aff7798


---
# <a name="virtual-machine-scale-sets-overview"></a>Übersicht über VM-Skalierungsgruppen
VM-Skalierungsgruppen sind eine Azure-Computeressource, mit der Sie eine Gruppe von identischen virtuellen Computern bereitstellen und verwalten können. Alle virtuellen Computer in einer VM-Skalierungsgruppe sind gleich konfiguriert. Dadurch ermöglichen VM-Skalierungsgruppen eine echte automatische Skalierung. Eine Vorabbereitstellung der virtuellen Computer ist dabei nicht erforderlich. Das erleichtert die Erstellung leistungsfähiger Dienste, die auf große Rechenleistung, Big Data und Workloads in Containern ausgerichtet sind.

Bei Anwendungen, für die Computeressourcen horizontal herunter- und hochskaliert werden müssen, werden Skalierungsoperationen implizit über Fehler- und Updatedomänen ausgeglichen. Eine Einführung in das Thema „VM-Skalierungsgruppen“ finden Sie in der [Ankündigung im Azure-Blog](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/)(in englischer Sprache).

Sehen Sie sich diese Videos an, um weitere Informationen zu VM-Skalierungsgruppen zu erhalten:

* [Mark Russinovich spricht über Azure-Skalierungsgruppen](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [VM-Skalierungsgruppen mit Guy Bowerman](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Erstellen und Verwalten von VM-Skalierungsgruppen
Erstellen Sie im [Azure-Portal](https://portal.azure.com) eine VM-Skalierungsgruppe, indem Sie *Neu* auswählen und „Skalierung“ in die Suchleiste eingeben. In den Ergebnissen wird „VM-Skalierungsgruppe“ angezeigt. Sie können anschließend die Pflichtfelder ausfüllen, um Ihre Skalierungsgruppe anzupassen und bereitzustellen. 

VM-Skalierungsgruppen können auch mithilfe von JSON-Vorlagen und [REST-APIs](https://msdn.microsoft.com/library/mt589023.aspx) ebenso wie einzelne virtuelle Azure-Ressourcen-Manager-Computer definiert und bereitgestellt werden. Daher können alle standardmäßigen Azure-Ressourcen-Manager-Bereitstellungsmethoden verwendet werden. Weitere Informationen zu Vorlagen finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](../resource-group-authoring-templates.md).

Eine Reihe von Beispielvorlagen für VM-Skalierungsgruppen finden Sie [hier](https://github.com/Azure/azure-quickstart-templates) im GitHub-Repository für Azure-Schnellstartvorlagen (suchen Sie nach Vorlagen mit *vmss* im Titel).

Auf den Detailseiten für diese Vorlagen finden Sie eine Schaltfläche, die eine Verknüpfung mit dem Bereitstellungsfeature des Portals herstellt. Zum Bereitstellen der VM-Skalierungsgruppe klicken Sie auf diese Schaltfläche und geben dann alle Parameter an, die im Portal erforderlich sind. Wenn Sie sich nicht sicher sind, ob eine Ressource Großbuchstaben oder eine gemischte Groß- und Kleinbuchschreibung unterstützt, ist es sicherer, für die Parameterwerte nur Kleinbuchstaben zu verwenden. Es gibt auch ein anschauliches Video mit einer Analyse einer VM-Skalierungsgruppenvorlage:

[Analyse einer VM-Skalierungsgruppenvorlage](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Hochskalieren und Herunterskalieren einer VM-Skalierungsgruppe
Wenn Sie die Anzahl der virtuellen Computer in einer VM-Skalierungsgruppe erhöhen oder verringern möchten, ändern Sie einfach die Eigenschaft *capacity* und stellen die Vorlage erneut bereit. Diese Einfachheit erleichtert es, eine benutzerdefinierte Skalierungsschicht zu schreiben, falls Sie benutzerdefinierte Skalierungsereignisse definieren möchten, die von der automatischen Skalierung in Azure nicht unterstützt werden.

Wenn Sie eine Vorlage erneut bereitstellen, um die Kapazität zu erhöhen, können Sie dafür eine viel kleinere Vorlage definieren, die nur die SKU und die geänderte Kapazität enthält. Ein Beispiel sehen Sie [hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing)

Die Schritte, mit denen eine Skalierungsgruppe erstellt wird, die automatisch skaliert wird, werden unter [Automatisches Skalieren von Computern in einer VM-Skalierungsgruppe](virtual-machine-scale-sets-windows-autoscale.md)

## <a name="monitoring-your-vm-scale-set"></a>Überwachen der VM-Skalierungsgruppe
Im [Azure-Portal](https://portal.azure.com) werden Skalierungsgruppen ausgelistet, und es werden grundlegende Eigenschaften und Vorgänge angezeigt, z.B. eine Auflistung der VMs in der Gruppe und ein Graph zur Ressourcenverwendung. Wenn Sie ausführlichere Informationen benötigen, können Sie den [Azure-Ressourcen-Explorer](https://resources.azure.com) verwenden, um VM-Skalierungsgruppen anzuzeigen. Bei VM-Skalierungsgruppen handelt es sich um Ressourcen unter „Microsoft.Compute“. Sie können sie daher anzeigen, indem Sie auf dieser Website die folgenden Links öffnen:

**Abonnements > Ihr Abonnement > resourceGroups > providers > Microsoft.Compute > virtualMachineScaleSets > Ihre VM-Skalierungsgruppe > usw.**

## <a name="vm-scale-set-scenarios"></a>Szenarien für VM-Skalierungsgruppen
Dieser Abschnitt enthält einige typische Szenarien für VM-Skalierungsgruppen. Einige auf höherer Ebene angesiedelte Azure-Dienste (wie Batch, Service Fabric und Azure Container Service) verwenden diese Szenarien.

* **Zugriff mit RDP oder SSH auf VM-Skalierungsgruppeninstanzen**: Eine VM-Skalierungsgruppe wird in einem VNet erstellt, und den einzelnen virtuellen Computern in der Skalierungsgruppe werden keine öffentlichen IP-Adressen zugeordnet. Das ist gut so, weil Sie im Allgemeinen den Aufwand und die Verwaltungsarbeiten vermeiden möchten, die mit dem Zuordnen von separaten öffentlichen IP-Adressen zu allen zustandslosen Ressourcen in Ihrem Computenetz verbunden sind. Außerdem können Sie über andere Ressourcen in Ihrem VNET, die über öffentliche IP-Adressen verfügen, leicht eine Verbindung mit diesen virtuellen Computern herstellen, z.B. über Load Balancer oder eigenständige virtuelle Computer.
* **Herstellen einer Verbindung mit virtuellen Computern mithilfe von NAT-Regeln** : Sie können eine öffentliche IP-Adresse erstellen, sie einem Load Balancer zuweisen und NAT-Regeln (Network Address Translation) für eingehenden Datenverkehr definieren, die einen Port der IP-Adresse einem Port auf dem virtuellen Computer in der VM-Skalierungsgruppe zuordnen. Beispiel:
  
  | Quelle | Quellport | Ziel | Zielport |
  | --- | --- | --- | --- |
  |  Öffentliche IP |Port 50000 |vmss\_0 |Port 22 |
  |  Öffentliche IP |Port 50001 |vmss\_1 |Port 22 |
  |  Öffentliche IP |Port 50002 |vmss\_2 |Port 22 |
  
   Hier ist ein Beispiel für das Erstellen einer VM-Skalierungsgruppe, bei der NAT-Regeln verwendet werden, um eine SSH-Verbindung (Secure Shell) mit jedem virtuellen Computer in der Skalierungsgruppe über eine einzige öffentliche IP-Adresse zu ermöglichen: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)
  
   Hier ist ein Beispiel, bei dem dasselbe mit RDP (Remote Desktop Protocol) und Windows erreicht wird: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)
* **Herstellen einer Verbindung mit VMs mithilfe einer „Jumpbox“**: Wenn Sie eine VM-Skalierungsgruppe und einen eigenständigen virtuellen Computer in demselben VNET erstellen, können der eigenständige virtuelle Computer und die virtuellen Computer in der VM-Skalierungsgruppe eine Verbindung miteinander herstellen, indem sie ihre internen IP-Adressen verwenden, die durch das VNet/Subnetz definiert sind. Wenn Sie eine öffentliche IP-Adresse erstellen und dem eigenständigen virtuellen Computer zuweisen, können Sie mit RDP oder SSH eine Verbindung mit dem eigenständigen virtuellen Computer herstellen. Anschließend können Sie von diesem Computer aus eine Verbindung mit den VM-Skalierungsgruppeninstanzen herstellen. Vielleicht erkennen Sie an dieser Stelle, dass eine einfache VM-Skalierungsgruppe inhärent sicherer ist als ein einfacher eigenständiger virtueller Computer in Standardkonfiguration mit einer öffentlichen IP-Adresse.
  
   Mit dieser Vorlage wird beispielsweise eine einfache Skalierungsgruppe mit einer eigenständigen VM bereitgestellt: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox)
* **Lastenausgleich für VM-Skalierungsgruppeninstanzen**: Wenn Sie eine Workload per Roundrobin an einen Computecluster aus virtuellen Computern übergeben möchten, können Sie einen Azure Load Balancer mit entsprechenden Lastenausgleichsregeln konfigurieren. Sie können Tests definieren, um zu überprüfen, ob Ihre Anwendung ordnungsgemäß ausgeführt wird. Diese Tests pingen die Ports mit einem festgelegten Protokoll in vorgegebenen Zeitabständen und mit einem angegebenen Anforderungspfad. Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) unterstützt darüber hinaus Skalierungsgruppen sowie anspruchsvollere Lastenausgleichsszenarien.
  
   Hier ist ein Beispiel angegeben, in dem eine VM-Skalierungsgruppe mit Ausführung von Apache-Webservern erstellt und ein Lastenausgleich verwendet wird, um die Last für die einzelnen VMs zu verteilen: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl). (Achten Sie auf den Ressourcentyp „Microsoft.Network/loadBalancers“ und „networkProfile“ und „extensionProfile“ in „virtualMachineScaleSet“.)
* **Bereitstellen einer VM-Skalierungsgruppe als Computecluster in einem PaaS-Cluster-Manager** : VM-Skalierungsgruppen werden mitunter als Workerrolle der nächsten Generation beschrieben. Diese Beschreibung ist zwar zutreffend, aber sie kann zu Verwechslungen zwischen den Features von Skalierungsgruppen und PaaS v1-Workerrollen führen. In einem bestimmten Sinn stellen VM-Skalierungsgruppen tatsächlich eine „Workerrolle“ oder eine Workerressource bereit: Sie bieten eine generalisierte Computeressource, die von  Plattformen und Laufzeitumgebungen unabhängig ist, sich anpassen lässt und in die Azure-Ressourcen-Manager-IaaS integriert ist.
  
   Eine PaaS v1-Workerrolle ist zwar im Hinblick auf die Unterstützung für Plattformen/Laufzeitumgebungen eingeschränkt (nur für Images der Windows-Plattform), aber sie bietet auch Dienste wie VIP-Austausch, konfigurierbare Upgradeeinstellungen und Einstellungen für laufzeitumgebungs- und anwendungsspezifische Bereitstellungen, die entweder *noch* nicht in VM-Skalierungsgruppen zur Verfügung stehen oder die durch auf höherer Ebene angesiedelte PaaS-Dienste wie Service Fabric umgesetzt werden. Vor diesem Hintergrund können Sie VM-Skalierungsgruppen als eine Infrastruktur betrachten, die PaaS unterstützt. Das heißt: PaaS-Lösungen wie Service Fabric oder Cluster-Manager wie Mesos können auf VM-Skalierungsgruppen aufsetzen, um eine skalierbare Compute-Ebene zu bilden.
  
   In einem Beispiel für diesen Ansatz stellt der Azure Container Service einen Cluster basierend auf Skalierungsgruppen mit einem „Orchestrator“ für Container bereit: [https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos).

## <a name="vm-scale-set-performance-and-scale-guidance"></a>Richtlinien zur Leistungsfähigkeit von VM-Skalierungsgruppen und zum Skalieren
* Erstellen Sie höchstens 500 virtuelle Computer in mehreren VM-Skalierungsgruppen gleichzeitig.
* Planen Sie nicht mehr als 20 virtuelle Computer pro Speicherkonto ein (es sei denn, Sie legen die *overprovision*-Eigenschaft auf „false“ fest, in diesem Fall können Sie bis zu 40 festlegen).
* Nutzen Sie für die Namen der Speicherkonten verschiedene und im Alphabet möglichst weit auseinanderliegende Buchstaben.  Die Beispielvorlagen für VM-Skalierungsgruppen (VMSS, Virtual Machine Scale Sets) in den [Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates/) veranschaulichen die empfohlene Vorgehensweise.
* Wenn Sie benutzerdefinierte virtuelle Computer verwenden, sollten Sie mit höchstens 40 virtuellen Computern pro VM-Skalierungsgruppe in einem einzelnen Speicherkonto planen.  Sie müssen das Image vorab in das Speicherkonto kopieren, bevor Sie mit der Bereitstellung der VM-Skalierungsgruppe beginnen können. Weitere Informationen finden Sie in den FAQ.
* Planen Sie mit höchstens 4096 virtuellen Computern pro VNET.
* Die Anzahl von VMs, die Sie erstellen können, wird durch das Kernkontingent in der Region beschränkt, in der Sie sie bereitstellen. Unter Umständen müssen Sie sich an den Kundensupport  wenden und das Limit im Computekontingent auch dann erhöhen lassen, wenn Sie heute über ein hohes Limit an Kernen verfügen, die für Clouddienste oder IaaS v1 genutzt werden können. Sie können Ihr Kontingent mit dem folgenden Befehl der Azure-CLI abfragen: `azure vm list-usage`. Oder Sie verwenden dazu den folgenden PowerShell-Befehl: `Get-AzureRmVMUsage` (Falls Sie eine ältere Version als PowerShell 1.0 nutzen, verwenden Sie `Get-AzureVMUsage`).

## <a name="vm-scale-set-frequently-asked-questions"></a>VM-Skalierungsgruppen – häufig gestellte Fragen
**F.**  Wie viele virtuelle Computer können in eine VM-Skalierungsgruppe aufgenommen werden?

**A.**  100, sofern Sie Plattformimages verwenden, die über mehrere Speicherkonten verteilt werden können. Wenn Sie benutzerdefinierte Images verwenden, bis zu 40 (wenn die *overprovision*-Eigenschaft auf „false“ festgelegt ist, denn standardmäßig sind es 20), da benutzerdefinierte Images derzeit auf ein einzelnes Speicherkonto begrenzt sind.

**F.** Welche anderen Ressourceneinschränkungen bestehen für VM-Skalierungsgruppen?

**A.**  Während eines 10-minütigen Zeitraums dürfen höchstens 500 virtuelle Computer in mehreren Skalierungsgruppen pro Region erstellt werden. Die bestehenden [Diensteinschränkungen des Azure-Abonnements](../azure-subscription-service-limits.md) gelten.

**F.**  Werden Datenträger innerhalb von VM-Skalierungsgruppen unterstützt?

**A.** Nicht in der ersten Version (Datenträger für Daten sind in der Vorschauversion aber derzeit verfügbar). Zum Speichern von Daten haben Sie folgende Möglichkeiten:

* Azure-Dateien (SMB-freigegebene Laufwerke)
* Betriebssystemlaufwerk
* Temporäres Laufwerk (lokal, nicht durch Azure Storage gesichert) 
* Azure-Datendienst (z. B. Azure-Tabellen, Azure-Blobs)
* Externer Datendienst (z. B. Remote-DB)

**F.**  In welchen Azure-Regionen werden VM-Skalierungsgruppen unterstützt?

**A.**  VM-Skalierungsgruppen werden in allen Regionen unterstützt, die den Azure-Ressourcen-Manager unterstützen.

**F.**  Wie erstellen Sie eine VM-Skalierungsgruppe mit einem benutzerdefinierten Image?

**A.** Lassen Sie die vhdContainers-Eigenschaft leer, wie z. B hier:

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": "https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd"
            },
            "osType": "Windows"
        }
    },


**F.**  Welche virtuellen Computer werden entfernt, wenn ich meine Kapazität für VM-Skalierungsgruppen von 20 auf 15 verringere?

**A.**  Die Entfernung der virtuellen Computer aus den Skalierungsgruppen erfolgt gleichmäßig über Upgrade- und Fehlerdomänen hinweg, um die Verfügbarkeit zu maximieren. Die VMs mit den höchsten IDs werden zuerst entfernt.

**F.**  Was passiert, wenn ich dann die Kapazität von 15 auf 18 erhöhe?

**A.** Wenn Sie die Kapazität auf 18 erhöhen, werden 3 neue VMs erstellt. Jedes Mal wird die VM-Instanz-ID vom vorherigen höchsten Wert ausgehend erhöht (z.B. 20, 21, 22). Die virtuellen Computer werden gleichmäßig über Fehlerdomänen (FDs) und Updatedomänen (UDs) verteilt.

**F.**  Kann ich beim Verwenden mehrerer Erweiterungen in einer VM-Skalierungsgruppe eine Ausführungsreihenfolge erzwingen?

**A.** Nicht direkt. Bei der customScript-Erweiterung kann das Skript jedoch warten, bis eine andere Erweiterung abgeschlossen ist ([z.B. durch Überwachung des Erweiterungsprotokolls](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Weitere Informationen zur Erweiterungssequenzierung finden Sie im folgenden Blogbeitrag: [Extension Sequencing in Azure VM Scale Sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/) (Erweiterungssequenzierung in Skalierungsgruppen mit virtuellen Azure-Computern).

**F.**  Funktionieren VM-Skalierungsgruppen mit Azure-Verfügbarkeitsgruppen?

**A.**  Ja. Eine VM-Skalierungsgruppe ist eine implizite Verfügbarkeitsgruppe mit 5 Fehlerdomänen (FDs) und 5 Updatedomänen (UDs). Unter virtualMachineProfile brauchen Sie nichts zu konfigurieren. In künftigen Versionen werden VM-Skalierungsgruppen wahrscheinlich mehrere Mandanten umspannen, aber derzeit entspricht eine Skalierungsgruppe einer einzigen Verfügbarkeitsgruppe.




<!--HONumber=Nov16_HO3-->


