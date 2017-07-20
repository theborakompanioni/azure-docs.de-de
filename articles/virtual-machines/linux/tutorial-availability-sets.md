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
ms.date: 05/22/2017
ms.author: cynthn
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 927eb9f4f40759c0a79daa6dd3e91e5ab0853520
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="how-to-use-availability-sets"></a>Verwenden von Verfügbarkeitsgruppen


In diesem Tutorial erfahren Sie etwas darüber, wie Sie die Verfügbarkeit und Zuverlässigkeit Ihrer Virtual Machine-Lösungen in Azure mithilfe Verfügbarkeitsgruppen erhöhen. Verfügbarkeitsgruppen sorgen dafür, dass die von Ihnen in Azure bereitgestellten virtuellen Computer auf mehrere isolierte Hardwarecluster verteilt werden. Dadurch wird sichergestellt, dass bei einem Hardware- oder Softwarefehler in Azure nur ein Teil Ihrer virtuellen Computer beeinträchtigt wird und dass die Lösung insgesamt aus Sicht Ihrer Kunden verfügbar und betriebsbereit bleibt.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Verfügbarkeitsgruppe erstellen
> * Erstellen eines virtuellen Computers in einer Verfügbarkeitsgruppe
> * Überprüfen der verfügbaren VM-Größen


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="availability-set-overview"></a>Übersicht über Verfügbarkeitsgruppen

Eine Verfügbarkeitsgruppe ist eine Funktion zur logischen Gruppierung, mit der Sie in Azure sicherstellen können, dass die darin enthaltenen VM-Ressourcen voneinander isoliert sind, wenn sie in einem Azure-Rechenzentrum bereitgestellt werden. Azure stellt sicher, dass die virtuellen Computer innerhalb einer Verfügbarkeitsgruppe auf mehrere physische Server, Compute-Racks, Speichereinheiten und Netzwerkswitches verteilt werden. Dadurch wird im Fall eines Hardware- oder Softwarefehlers in Azure nur ein Teil Ihrer virtuellen Computer beeinträchtigt, und die Anwendung insgesamt bleibt betriebsbereit und weiterhin für Ihre Kunden verfügbar. Verfügbarkeitsgruppen sind eine wesentliche Funktion für die Einrichtung zuverlässiger Cloudlösungen.

In einer typischen VM-basierten Lösung gibt es möglicherweise vier Front-End-Webserver und zwei Back-End-VMs, die eine Datenbank hosten. Sie können in Azure zwei Verfügbarkeitsgruppen definieren, bevor Sie Ihre virtuellen Computer bereitstellen: eine Verfügbarkeitsgruppe für die Ebene „Web“ und eine Verfügbarkeitsgruppe für die Ebene „Datenbank“. Bei der Erstellung eines neuen virtuellen Computers geben Sie dann die Verfügbarkeitsgruppe als Parameter für den Befehl „az vm create“ an, damit Azure automatisch sicherstellen kann, dass die virtuellen Computer, die Sie in der Verfügbarkeitsgruppe erstellen über mehrere physische Hardwareressourcen isoliert werden. Wenn bei der physischen Hardware, auf der Ihre Webserver- oder Datenbankserver-VMs ausgeführt werden, ein Problem auftritt, können Sie sich sicher sein, dass die anderen Instanzen Ihrer Webserver- und Datenbank-VMs weiterhin einwandfrei ausgeführt werden, da sie sich auf anderer Hardware befinden.

Sie sollten immer Verfügbarkeitsgruppen verwenden, wenn Sie zuverlässige VM-basierte Lösungen in Azure bereitstellen möchten.


## <a name="create-an-availability-set"></a>Verfügbarkeitsgruppe erstellen

Eine Verfügbarkeitsgruppe können Sie mithilfe von [az vm availability-set create](/cli/azure/availability-set#create) erstellen. Im folgenden Beispiel wird die Anzahl der Update- sowie der Fehlerdomänen für die Verfügbarkeitsgruppe *myAvailabilitySet* in der Ressourcengruppe *myResourceGroupAvailability* auf *2* festgelegt.

Erstellen Sie eine Ressourcengruppe.

```azurecli-interactive 
az group create --name myResourceGroupAvailability --location eastus
```


```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Mithilfe von Verfügbarkeitsgruppen können Sie Ressourcen in „Fehlerdomänen“ und „Updatedomänen“ isolieren. Eine **Fehlerdomäne** stellt eine isolierte Sammlung von Server-, Netzwerk- und Speicherressourcen dar. Im vorherigen Beispiel haben wir angegeben, dass unsere Verfügbarkeitsgruppe auf mindestens zwei Fehlerdomänen verteilt werden soll, wenn unsere VMs bereitgestellt werden. Wir geben auch an, dass wir unsere Verfügbarkeitsgruppe über zwei **Updatedomänen** verteilen möchten.  Zwei Updatedomänen stellen sicher, dass unsere VM-Ressourcen bei Softwareupdates in Azure isoliert sind. Damit wird verhindert, dass die zugrunde liegende Software auf allen VM gleichzeitig aktualisiert wird.

## <a name="create-vms-inside-an-availability-set"></a>Erstellen von virtuellen Computern in einer Verfügbarkeitsgruppe

Virtuelle Computer müssen in der Verfügbarkeitsgruppe erstellt werden, um sicherzustellen, dass sie ordnungsgemäß auf die Hardwarekomponenten verteilt werden. Nach der Erstellung kann einer Verfügbarkeitsgruppe kein vorhandener virtueller Computer mehr hinzugefügt werden. 

Beim Erstellen eines virtuellen Computers mit [az vm create](/cli/azure/vm#create) legen Sie die Verfügbarkeitsgruppe mit dem Parameter `--availability-set` fest, um den Namen der Verfügbarkeitsgruppe anzugeben.

```azurecli-interactive 
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

Wir verfügen jetzt über zwei virtuelle Computer in unseren neu erstellten Verfügbarkeitsgruppe. Da sie sich in derselben Verfügbarkeitsgruppe befinden, stellt Azure sicher, dass die virtuellen Computer und alle ihre Ressourcen (einschließlich Datenträger) auf isolierte physische Hardware verteilt werden. Mithilfe dieser Verteilung wird eine deutlich höhere Verfügbarkeit unserer gesamten VM-Lösung sichergestellt.

Sie werden beim Hinzufügen virtueller Computer feststellen, dass eine bestimmte Größe für virtuelle Computer nicht mehr für die Verwendung in der Verfügbarkeitsgruppe verfügbar ist. Dieses Problem kann auftreten, wenn nicht mehr genügend Kapazität für das Hinzufügen vorhanden ist, um die Isolationsregeln die Verfügbarkeitsgruppe weiter einzuhalten. Sie können die verfügbaren VM-Größen innerhalb einer vorhandenen Verfügbarkeitsgruppe mit dem `--availability-set list-sizes`-Parameter überprüfen.

## <a name="check-for-available-vm-sizes"></a>Prüfen der verfügbaren VM-Größen 

Sie können der Verfügbarkeitsgruppe später weitere virtuelle Computer hinzufügen. Dazu müssen Sie jedoch wissen, welche VM-Größen in der Hardware verfügbar sind. Verwenden Sie [az vm availability-set list-sizes](/cli/azure/availability-set#list-sizes), um alle verfügbaren Größen im Hardwarecluster für die Verfügbarkeitsgruppe aufzulisten.

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Verfügbarkeitsgruppe erstellen
> * Erstellen eines virtuellen Computers in einer Verfügbarkeitsgruppe
> * Überprüfen der verfügbaren VM-Größen

Im nächsten Tutorial erhalten Sie Informationen zu VM-Skalierungsgruppen.

> [!div class="nextstepaction"]
> [Erstellen einer VM-Skalierungsgruppe](tutorial-create-vmss.md)


