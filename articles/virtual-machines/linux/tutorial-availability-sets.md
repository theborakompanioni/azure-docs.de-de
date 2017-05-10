---
title: "Tutorial zu Verfügbarkeitsgruppen für virtuelle Linux-Computer in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie etwas über die Verfügbarkeitsgruppen für virtuelle Linux-Computer in Azure."
documentationcenter: 
services: virtual-machines-linux
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/20/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: d082b37a2e070136178259c54ada8dc141f81e13
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---

# <a name="how-to-use-availability-sets"></a>Verwenden von Verfügbarkeitsgruppen

In diesem Tutorial erhalten Sie Informationen dazu, wie Sie die Verfügbarkeit von virtuellen Computern (VMs) steigern können, indem Sie sie einer logischen Gruppierung – einer sogenannten Verfügbarkeitsgruppe – zuweisen. Wenn Sie virtuelle Computer in einer Verfügbarkeitsgruppe erstellen, werden diese auf der Azure-Plattform in der zugrunde liegenden Infrastruktur verteilt. Bei einem Hardwarefehler oder einer geplanten Wartung auf der Plattform wird durch die Verwendung von Verfügbarkeitsgruppen sichergestellt, dass zumindest ein virtueller Computer weiterhin ausgeführt wird.

Die Schritte in diesem Tutorial können mit der neuesten Version von [Azure CLI 2.0](/cli/azure/install-azure-cli) ausgeführt werden.

## <a name="availability-set-overview"></a>Übersicht über Verfügbarkeitsgruppen

Virtuelle Computer können in logischen Hardwaregruppierungen im zugrunde liegenden Azure-Rechenzentrum erstellt werden. Wenn Sie mehrere virtuelle Computer erstellen, werden die Compute- und Speicherressourcen auf die Hardware (z.B. Server, Netzwerkswitches und Speicher) verteilt. Diese Verteilung gewährleistet die Verfügbarkeit Ihrer App, wenn eine der Hardwarekomponenten gewartet wird. Diese logische Gruppierung lässt sich mit Verfügbarkeitsgruppen definieren.

Verfügbarkeitsgruppen bieten hohe Verfügbarkeit für die virtuellen Computer. Sie sollten zudem sicherstellen, dass Ihre Anwendungen auch so konzipiert werden, dass Ausfälle oder Wartungsereignisse toleriert werden.

## <a name="create-an-availability-set"></a>Verfügbarkeitsgruppe erstellen

Eine Verfügbarkeitsgruppe können Sie mithilfe von [az vm availability-set create](/cli/azure/availability-set#create) erstellen. Im folgenden Beispiel wird die Anzahl der Update- sowie der Fehlerdomänen für die Verfügbarkeitsgruppe *myAvailabilitySet* in der Ressourcengruppe *myResourceGroupAvailability* auf *2* festgelegt.

```azurecli
az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

## <a name="create-vms-inside-an-availability-set"></a>Erstellen von virtuellen Computern in einer Verfügbarkeitsgruppe

Virtuelle Computer müssen in der Verfügbarkeitsgruppe erstellt werden, um sicherzustellen, dass sie ordnungsgemäß auf die Hardwarekomponenten verteilt werden. Nach der Erstellung kann einer Verfügbarkeitsgruppe kein vorhandener virtueller Computer mehr hinzugefügt werden. 

Die Hardware an einem Standort ist in mehrere Updatedomänen und Fehlerdomänen unterteilt. Eine **Updatedomäne** ist eine Gruppe von virtuellen Computern und der zugrunde liegenden physischen Hardwarekomponenten, die gleichzeitig neu gestartet werden können. Virtuelle Computer in ein und derselben **Fehlerdomäne** nutzen denselben Speicher sowie eine Stromquelle und einen Netzwerkswitch gemeinsam. 

Beim Erstellen eines virtuellen Computers mit [az vm create](/cli/azure/vm#create) legen Sie die Verfügbarkeitsgruppe mit dem Parameter `--availability-set` fest, um den Namen der Verfügbarkeitsgruppe anzugeben.

```azurecli
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --size Standard_DS1_v2  \
     --image Canonical:UbuntuServer:14.04.4-LTS:latest \
     --admin-username azureuser \
     --generate-ssh-keys \
     --no-wait
done 
```

Damit wurden 2 virtuelle Computer auf die zugrunde liegenden Hardwarekomponenten verteilt. 

## <a name="check-for-available-vm-sizes"></a>Prüfen der verfügbaren VM-Größen 

Sie können der Verfügbarkeitsgruppe später weitere virtuelle Computer hinzufügen. Dazu müssen Sie jedoch wissen, welche VM-Größen in der Hardware verfügbar sind. Verwenden Sie [az vm availability-set list-sizes](/cli/azure/availability-set#list-sizes), um alle verfügbaren Größen im Hardwarecluster für die Verfügbarkeitsgruppe aufzulisten.

```azurecli
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Verfügbarkeitsgruppen verwendet werden. Im nächsten Tutorial erhalten Sie Informationen zu VM-Skalierungsgruppen.

[Erstellen einer VM-Skalierungsgruppe](tutorial-create-vmss.md)


