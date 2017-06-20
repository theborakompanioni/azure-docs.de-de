---
title: "Azure CLI-Skriptbeispiel – Erstellen eines virtuellen Computers aus einer Momentaufnahme | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen eines virtuellen Computers aus einer Momentaufnahme"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: 960a44e0df2c46cea464d9277f22f449b62e7d06
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---

# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a>Erstellen eines virtuellen Computers aus einer Momentaufnahme über die Befehlszeilenschnittstelle

Dieses Skript erstellt einen virtuellen Computer aus einer Momentaufnahme eines Betriebssystemdatenträgers.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Erstellen eines virtuellen Computers aus einer Momentaufnahme")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um einen verwalteten Datenträger, einen virtuellen Computer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az snapshot show](https://docs.microsoft.com/cli/azure/snapshot#show) | Ruft die Momentaufnahme unter Verwendung des Momentaufnahmen- und Ressourcengruppennamens ab. Die ID-Eigenschaft des zurückgegebenen Objekts wird verwendet, um einen verwalteten Datenträger zu erstellen.  |
| [az disk create](https://docs.microsoft.com/cli/azure/disk#create) | Erstellt verwaltete Datenträger aus einer Momentaufnahme unter Verwendung der Momentaufnahmen-ID, des Datenträgernamens, des Speichertyps und der Größe.  |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#create) | Erstellt einen virtuellen Computer unter Verwendung eines verwalteten Betriebssystemdatenträgers. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

