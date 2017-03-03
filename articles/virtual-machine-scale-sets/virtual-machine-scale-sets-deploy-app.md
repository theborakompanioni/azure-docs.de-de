---
title: "Bereitstellen einer App in Skalierungsgruppen für virtuelle Computer | Microsoft Docs"
description: "Bereitstellen einer App in Skalierungsgruppen für virtuelle Computer"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8892199-f2e2-4b82-988a-28ca8a7fd1eb
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: guybo
translationtype: Human Translation
ms.sourcegitcommit: f13545d753690534e0e645af67efcf1b524837eb
ms.openlocfilehash: dad27b11b5f02ed41826b82882cc5089eb69cb04
ms.lasthandoff: 02/09/2017


---
# <a name="deploy-an-app-on-virtual-machine-scale-sets"></a>Bereitstellen einer App in Skalierungsgruppen für virtuelle Computer
Eine Anwendung, die in einer VM-Skalierungsgruppe ausgeführt wird, wird normalerweise auf drei verschiedene Arten bereitgestellt:

* Installieren Sie neue Software auf einem Plattformimage zum Zeitpunkt der Bereitstellung. Ein Plattformimage ist in diesem Kontext ein Betriebssystemimage aus dem Azure Marketplace, z.B. Ubuntu 16.04, Windows Server 2012 R2 usw.

Sie können neue Software mithilfe einer [VM-Erweiterung](../virtual-machines/virtual-machines-windows-extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) auf einem Plattformimage installieren. Eine VM-Erweiterung ist eine Software, die ausgeführt wird, wenn ein virtueller Computer bereitgestellt wird. Sie können beliebigen Code zum Zeitpunkt der Bereitstellung ausführen, indem Sie eine benutzerdefinierte Skripterweiterung verwenden. [Hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) ist ein Beispiel für eine Azure Resource Manager-Vorlage mit zwei VM-Erweiterungen: eine benutzerdefinierte Linux-Skripterweiterung zum Installieren von Apache und PHP sowie eine Diagnoseerweiterung zum Ausgeben von Leistungsdaten, die von der automatischen Skalierung in Azure verwendet werden.

Ein Vorteil dieses Ansatzes ist die Trennung von Anwendungscode und Betriebssystem, und Ihre Anwendung kann separat verwaltet werden. Natürlich bedeutet dies, dass auch mehr „bewegliche Teile“ vorhanden sind, und die Dauer der VM-Bereitstellung verlängert sich möglicherweise, wenn das Skript viel herunterladen und konfigurieren muss.

**Wenn Sie vertrauliche Informationen im Befehl der benutzerdefinierten Skripterweiterung übergeben (z.B. ein Kennwort), müssen Sie den `commandToExecute` im `protectedSettings`-Attribut der benutzerdefinierten Skripterweiterung angeben, nicht im `settings`-Attribut.**

* Erstellen Sie ein benutzerdefiniertes VM-Image, das das Betriebssystem und die Anwendung in einer einzelnen VHD enthält. Hier besteht die Skalierungsgruppe aus einer Gruppe von VMs, die aus einem von Ihnen erstellten Image kopiert wird, das Sie verwalten müssen. Dieser Ansatz erfordert keine zusätzliche Konfiguration zum Zeitpunkt der VM-Bereitstellung. In der Version `2016-03-30` der VM-Skalierungsgruppen (und in früheren Versionen) sind die Betriebssystemdatenträger für die VMs in der Skalierungsgruppe allerdings auf ein einzelnes Speicherkonto beschränkt. Folglich können Sie höchstens 40 VMs in einer Skalierungsgruppe verwenden, im Unterschied zum Höchstwert von 100 VM pro Skalierungsgruppe für Plattformimages. Weitere Details finden Sie unter [Entwerfen von VM-Skalierungsgruppen für die Skalierung](virtual-machine-scale-sets-design-overview.md).

    >[!NOTE]
    >Die VM-Skalierungsgruppen-API mit der Version `2016-04-30-preview` unterstützt das Verwenden von Azure Managed Disks für den Betriebssystem-Datenträger und alle zusätzlichen Datenträger. Weitere Informationen finden Sie unter [Übersicht über Azure Managed Disks](../storage/storage-managed-disks-overview.md) und [Verwenden angefügter Datenträger](virtual-machine-scale-sets-attached-disks.md). 

* Stellen Sie eine Plattform oder ein benutzerdefiniertes Image bereit, bei der bzw. dem es sich im Grunde um einen Containerhost handelt, und installieren Sie die Anwendung als einen oder mehrere Container, die Sie mit einem Orchestrator oder einem Tool für die Konfigurationsverwaltung verwalten. Das Schöne an diesem Ansatz ist, dass Sie Ihre Cloudinfrastruktur von der Anwendungsebene abstrahiert haben und beides separat verwalten können.

## <a name="what-happens-when-a-vm-scale-set-scales-out"></a>Was geschieht, wenn eine VM-Skalierungsgruppe horizontal hochskaliert wird?
Wenn Sie einer Skalierungsgruppe VMs hinzufügen, indem Sie die Kapazität erhöhen (manuell oder durch automatische Skalierung), wird die Anwendung automatisch installiert. Wenn z.B. Erweiterungen für die Skalierungsgruppe definiert wurden, werden sie auf einer neuen VM jedes Mal ausgeführt, wenn sie erstellt wird. Wenn die Skalierungsgruppe auf einem benutzerdefinierten Image basiert, ist jede neue VM eine Kopie des benutzerdefinierten Quellimages. Wenn es sich bei den Skalierungsgruppen-VMs um Containerhosts handelt, könnten Sie Startcode zum Laden der Container in einer benutzerdefinierten Skripterweiterung verwenden. Alternativ kann eine Erweiterung einen Agent installieren, der bei einem Clusterorchestrator (z.B. Azure Container Service) registriert wird.

## <a name="how-do-you-manage-application-updates-in-vm-scale-sets"></a>Wie verwalten Sie Anwendungsupdates in VM-Skalierungsgruppen?
Für Anwendungsupdates in VM-Skalierungsgruppen ergeben sich aus den drei vorherigen Bereitstellungsmethoden für Anwendungen drei Verfahren:

* Aktualisieren mit VM-Erweiterungen. VM-Erweiterungen, die für eine VM-Skalierungsgruppe definiert wurden, werden jedes Mal ausgeführt, wenn eine neue VM bereitgestellt, für eine vorhandene VM ein Reimaging durchgeführt oder eine VM-Erweiterung aktualisiert wird. Wenn Sie die Anwendung aktualisieren müssen, ist das direkte Aktualisieren einer Anwendung über Erweiterungen eine geeignete Vorgehensweise: Sie aktualisieren einfach die Definition der Erweiterung. Dazu können Sie einfach die fileUris so ändern, dass sie auf die neue Software verweisen.

* Das Verfahren mit einem unveränderlichen benutzerdefinierten Image. Wenn Sie die Anwendung (oder App-Komponenten) in ein VM-Image integrieren, können Sie sich darauf konzentrieren, eine verlässliche Pipeline zum Automatisieren von Builds, Tests und Bereitstellungen der Images zu erstellen. Sie können Ihre Architektur so entwerfen, dass der schnelle Austausch einer bereitgestellten Skalierungsgruppe in der Produktion erleichtert wird. Ein gutes Beispiel dieses Verfahrens ist die [Azure Spinnaker-Treibermethode](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

Packer und Terraform unterstützen Azure Resource Manager ebenfalls. Daher können Sie Images auch „als Code“ definieren und in Azure erstellen und dann die VHD in der Skalierungsgruppe verwenden. Allerdings entstehen dadurch Probleme mit Marketplace-Images, in denen Erweiterungen/benutzerdefinierte Skripts wichtiger werden, da Sie Marketplace-Elemente nicht direkt bearbeiten.

* Aktualisieren von Containern. Fassen Sie die Verwaltung des Anwendungslebenszyklus auf einer Ebene über der Cloudinfrastruktur zusammen, indem Sie z.B. Anwendungen und App-Komponenten in Containern kapseln und diese Container durch Containerorchestratoren und Konfigurations-Manager wie Chef/Puppet verwalten.

Die Skalierungsgruppen-VMs werden dann zu einem stabilen Substrat für die Container und erfordern nur gelegentliche Sicherheits- und Betriebssystemupdates. Wie bereits erwähnt, ist Azure Container Service ein gutes Beispiel für diesen Ansatz und das Erstellen eines entsprechenden Diensts.

## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Wie verteilen Sie ein Betriebssystemupdate auf Updatedomänen?
Nehmen wir an, dass Sie das Betriebssystemimage aktualisieren möchten, während gleichzeitig die VM-Skalierungsgruppe weiter ausgeführt wird. Eine Möglichkeit ist, die VM-Images der einzelnen VMs nacheinander zu aktualisieren. Sie können dazu PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden. Es gibt gesonderte Befehle zum Aktualisieren des VM-Skalierungsgruppenmodells (der Konfigurationsdefinition) und zur Ausgabe von Aufrufen zum „manuellen Upgrade“ auf einzelnen VMs. Das Azure-Dokument [Upgraden einer VM-Skalierungsgruppe](./virtual-machine-scale-sets-upgrade-scale-set.md) bietet auch weitere Informationen zu Optionen zum Ausführen von Betriebssystemupgrades in einer VM-Skalierungsgruppe.


