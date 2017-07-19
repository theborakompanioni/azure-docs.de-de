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
ms.devlang: azurecli
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/23/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 7a8653a82775e718c38f65f246d997ba61f99d58
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017


---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>Erneutes Bereitstellen eines virtuellen Linux-Computers in einem neuen Azure-Knoten
Wenn Sie Schwierigkeiten mit der Problembehandlung bei SSH oder dem Anwendungszugriff auf einen virtuellen Linux-Computer in Azure haben, lassen sich diese u.U. durch erneutes Bereitstellen des virtuellen Computers beseitigen. Wenn Sie einen virtuellen Computer erneut bereitstellen, wird er innerhalb der Azure-Infrastruktur auf einen neuen Knoten verschoben und dann wieder eingeschaltet. Dabei werden alle Ihre Konfigurationsoptionen und zugehörigen Ressourcen beibehalten. In diesem Artikel erfahren Sie, wie ein virtueller Computer mithilfe der Azure-Befehlszeilenschnittstelle oder dem Azure-Portal erneut bereitgestellt wird.

> [!NOTE]
> Nachdem Sie einen virtuellen Computer erneut bereitgestellt haben, geht der temporäre Datenträger verloren, und die der virtuellen Netzwerkschnittstelle zugeordneten dynamischen IP-Adressen werden aktualisiert. 

Sie können einen virtuellen Computer mithilfe einer der folgenden Optionen erneut bereitstellen. Sie müssen nur eine Option auswählen, um Ihren virtuellen Computer erneut bereitzustellen:

- [Azure CLI 2.0](#azure-cli-20)
- [Azure CLI 1.0](#azure-cli-10)
- [Azure-Portal](#using-azure-portal)

## <a name="use-the-azure-cli-20"></a>Verwenden von Azure CLI 2.0
Installieren Sie die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2), und melden Sie sich mit [az login](/cli/azure/#login) bei einem Azure-Konto an.

Stellen Sie mit [az vm redeploy](/cli/azure/vm#redeploy) Ihren virtuellen Computer erneut bereit. Im folgenden Beispiel wird die VM *myVM* in der Ressourcengruppe *myResourceGroup* erneut bereitgestellt:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-the-azure-cli-10"></a>Verwenden der Azure-Befehlszeilenschnittstelle 1.0
Installieren Sie die [Azure-Befehlszeilenschnittstelle 1.0](../../cli-install-nodejs.md), melden Sie sich bei einem Azure-Konto an, und stellen Sie sicher, dass der Resource Manager-Modus (`azure config mode arm`) aktiviert ist.

Im folgenden Beispiel wird die VM *myVM* in der Ressourcengruppe *myResourceGroup* erneut bereitgestellt:

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Nächste Schritte
Falls beim Herstellen einer Verbindung mit Ihrem virtuellen Computer Probleme auftreten, finden Sie spezifische Hilfe unter [Problembehandlung bei SSH-Verbindungen](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oder [Ausführliche Schritte zur Problembehandlung bei SSH](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sie können auch die Informationen zur [Problembehandlung bei der Anwendung](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lesen, wenn Sie auf eine Anwendung, die auf Ihrem virtuellen Computer ausgeführt wird, nicht zugreifen können.


