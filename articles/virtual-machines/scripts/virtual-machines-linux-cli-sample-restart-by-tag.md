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
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: b292a02770fa74812e74fa38f870e47ed7473b63
ms.lasthandoff: 03/04/2017

---

# <a name="restart-vms-by-tag"></a>Neustarten von VMs nach Tag

In diesem Beispiel werden virtuelle Maschinen mit einem bestimmten Tag in mehreren Ressourcengruppen erstellt.
Die virtuellen Computer werden parallel unter Verwendung von `--no-wait` erstellt, und es wird dann auf ihren kollektiven Abschluss gewartet.

Nachdem die virtuellen Computer erstellt wurden, werden sie unter Verwendung von zwei unterschiedlichen Abfragemechanismen neu gestartet.

Der erste startet die VMs mit derselben Abfrage neu, die verwendet wurde, um auf deren asynchrone Erstellung zu warten.
```bash
az vm restart --ids $(az vm list --query "join(' ', ${GROUP_QUERY}] | [].id)" \
    -o tsv) $1>/dev/null
```

Der zweite verwendet eine allgemeine Ressourcenauflistung und -abfrage, um deren IDs nach Tag abzurufen.
```bash
az vm restart --ids $(az resource list --tag ${TAG} \
    --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv) $1>/dev/null
```

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../virtual-machines-windows-cli-options.md).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[Main](../../../cli_scripts/virtual-machine/restart-by-tag/restart-by-tag.sh "Neustarten von VMs nach Tag")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach der Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppen, VMs und alle zugehörigen Ressourcen entfernt werden.

```azurecli
az group delete -n GROUP1 --no-wait --yes && \ 
az group delete -n GROUP2 --no-wait --yes && \
az group delete -n GROUP3 --no-wait --yes
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer, eine Verfügbarkeitsgruppe, einen Load Balancer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Erstellt die virtuellen Computer.  |
| [az vm list](https://docs.microsoft.com/cli/azure/vm#list) | Wird mit `--query` verwendet, um sicherzustellen, dass die VMs vor dem Neustart bereitgestellt werden. |
| [az vm restart](https://docs.microsoft.com/cli/azure/vm#list) | Startet die VMs neu. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

