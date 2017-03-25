---
title: "Azure CLI-Skriptbeispiel – Neustarten von VMs | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Neustarten von VMs nach Tag und nach ID"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/01/2017
ms.author: allclark
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: be1c613744d510e4ace636b47fdf730462a2ae07
ms.lasthandoff: 03/15/2017

---

# <a name="restart-vms"></a>Neustarten von VMs

Dieses Beispiel zeigt verschiedene Möglichkeiten, einige VMs abzurufen und sie neu zu starten.

Bei der ersten werden alle virtuellen Computer in der Ressourcengruppe neu gestartet.

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

Bei der zweiten werden die markierten VMs mit `az resouce list` abgerufen, und es wird nach den Ressourcen gefiltert, die virtuelle Computer sind, und diese virtuellen Computer werden neu gestartet.

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../virtual-machines-windows-cli-options.md).


## <a name="sample-script"></a>Beispielskript

Das Beispiel enthält drei Skripts.
Das erste stellt die virtuellen Computer bereit.
Es verwendet die „Nicht-warten“-Option, damit der Befehl nicht auf jeden bereitzustellenden virtuellen Computer wartet.
Das zweite wartet, bis die virtuellen Computer vollständig bereitgestellt sind.
Das dritte Skript startet alle virtuellen Computer neu, die bereitgestellt wurden, und dann einfach die markierten VMs.

### <a name="provision-the-vms"></a>Bereitstellen der VMs

Dieses Skript erstellt eine Ressourcengruppe und dann drei neu zu startende virtuelle Computer.
Zwei davon sind gekennzeichnet.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Bereitstellen der VMs")]

### <a name="wait"></a>Warten

Dieses Skript überprüft alle 20 Sekunden den Bereitstellungsstatus, bis alle drei virtuellen Computer bereitgestellt sind, oder bei der Bereitstellung eines von ihnen ein Fehler auftritt.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Warten auf das Bereitstellen der virtuellen Computer")]

### <a name="restart-the-vms"></a>Neustarten der VMs

Dieses Skript startet alle virtuellen Computer in der Ressourcengruppe neu und startet dann nur die markierten virtuellen Computer neu.

[!code-azurecli[Main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Neustarten von VMs nach Tag")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach der Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppen, VMs und alle zugehörigen Ressourcen entfernt werden.

```azurecli
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer, eine Verfügbarkeitsgruppe, einen Load Balancer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Erstellt die virtuellen Computer.  |
| [az vm list](https://docs.microsoft.com/cli/azure/vm#list) | Wird mit `--query` verwendet, um sicherzustellen, dass die virtuellen Computer vor ihrem Neustart bereitgestellt werden, und dann die IDs der virtuellen Computer abzurufen, um sie neu zu starten. |
| [az resource list](https://docs.microsoft.com/cli/azure/vm#list) | Wird mit `--query` verwendet, um die IDs der virtuellen Computer mit dem Tag abzurufen. |
| [az vm restart](https://docs.microsoft.com/cli/azure/vm#list) | Startet die VMs neu. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

