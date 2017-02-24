---
title: Erneutes Bereitstellen von virtuellen Linux-Computern in Azure | Microsoft Docs
description: Erneutes Bereitstellen von virtuellen Linux-Computern zum Beheben von Problemen mit der SSH-Verbindung.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/16/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 13ae22245d105b32b9fc50d7dbc8a6d50ad4560a
ms.openlocfilehash: 85fcc919b97d4cc06f89fc1ea5dc701ff61c2b27


---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>Erneutes Bereitstellen eines virtuellen Linux-Computers in einem neuen Azure-Knoten
Wenn Sie Schwierigkeiten mit der Problembehandlung bei SSH oder dem Anwendungszugriff auf einen virtuellen Linux-Computer in Azure haben, lassen sich diese u.U. durch erneutes Bereitstellen des virtuellen Computers beseitigen. Wenn Sie einen virtuellen Computer erneut bereitstellen, wird er innerhalb der Azure-Infrastruktur auf einen neuen Knoten verschoben und dann wieder eingeschaltet. Dabei werden alle Ihre Konfigurationsoptionen und zugehörigen Ressourcen beibehalten. In diesem Artikel erfahren Sie, wie ein virtueller Computer mithilfe der Azure-Befehlszeilenschnittstelle oder dem Azure-Portal erneut bereitgestellt wird.

> [!NOTE]
> Nachdem Sie einen virtuellen Computer erneut bereitgestellt haben, geht der temporäre Datenträger verloren, und die der virtuellen Netzwerkschnittstelle zugeordneten dynamischen IP-Adressen werden aktualisiert. 

Sie können einen virtuellen Computer mithilfe einer der folgenden Optionen erneut bereitstellen. Sie müssen nur eine Option auswählen, um Ihren virtuellen Computer erneut bereitzustellen:

- [Azure CLI 1.0](#azure-cli-10)
- [Azure CLI 2.0 (Vorschau)](#azure-cli-20-preview)
- [Azure-Portal](#using-azure-portal)


## <a name="azure-cli-10"></a>Azure-Befehlszeilenschnittstelle 1.0
Installieren Sie die [Azure-Befehlszeilenschnittstelle 1.0](../xplat-cli-install.md), melden Sie sich bei einem Azure-Konto an, und stellen Sie sicher, dass der Resource Manager-Modus (`azure config mode arm`) aktiviert ist.

Das folgende Beispiel stellt den virtuellen Computer mit dem Namen `myVM` in der Ressourcengruppe `myResourceGroup` erneut bereit:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

## <a name="azure-cli-20-preview"></a>Azure CLI 2.0 (Vorschau)
Installieren Sie die neueste [Azure-Befehlszeilenschnittstelle 2.0 (Vorschau)](/cli/azure/install-az-cli2), und melden Sie sich mit [az login](/cli/azure/#login) bei einem Azure-Konto an.

Stellen Sie mit [az vm redeploy](/cli/azure/vm#redeploy) Ihren virtuellen Computer erneut bereit. Das folgende Beispiel stellt den virtuellen Computer mit dem Namen `myVM` in der Ressourcengruppe `myResourceGroup` erneut bereit:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Nächste Schritte
Falls beim Herstellen einer Verbindung mit Ihrem virtuellen Computer Probleme auftreten, finden Sie spezifische Hilfe unter [Problembehandlung bei SSH-Verbindungen](virtual-machines-linux-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [Ausführliche Schritte zur Problembehandlung bei SSH](virtual-machines-linux-detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sie können auch die Informationen zur [Problembehandlung bei der Anwendung](virtual-machines-linux-troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lesen, wenn Sie auf eine Anwendung, die auf Ihrem virtuellen Computer ausgeführt wird, nicht zugreifen können.




<!--HONumber=Feb17_HO3-->


