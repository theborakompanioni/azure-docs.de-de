---
title: "Azure CLI-Skriptbeispiel – Erstellen einer VM mit einer VHD | Microsoft-Dokumentation"
description: 'Azure-CLI-Skriptbeispiel: Erstellen einer VM mit einer virtuellen Festplatte.'
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/09/2017
ms.author: allclark
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: c654db0142df904acc5212b0f53991c54cbf2087
ms.lasthandoff: 04/06/2017

---

# <a name="create-a-vm-with-a-virtual-hard-disk"></a>Erstellen einer VM mit einer virtuellen Festplatte

In diesem Beispiel wird ein virtueller Computer mithilfe einer VHD erstellt.
Es wird eine Ressourcengruppe, ein Speicherkonto und ein Container erstellt. Anschließend wird eine VM erstellt, indem die VHD in den Container hochgeladen wird.
Der öffentliche SSH-Schlüssel wird durch Ihren öffentlichen Schlüssel ersetzt, damit Sie Zugriff auf die VM haben.

Sie benötigen eine startbare virtuelle Festplatte.
Sie können die virtuelle Festplatte, die wir verwendet haben, unter „https://azclisamples.blob.core.windows.net/vhds/sample.vhd“ herunterladen oder Ihre eigene VHD verwenden. Das Skript sucht nach `~/sample.vhd`.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Erstellen einer VM mithilfe einer VHD")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer, eine Verfügbarkeitsgruppe, einen Load Balancer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account list](https://docs.microsoft.com/cli/azure/storage/account#list) | Listet Speicherkonten auf |
| [az storage account check-name](https://docs.microsoft.com/cli/azure/storage/account#check-name) | Stellt sicher, dass ein Speicherkontoname gültig und nicht bereits vorhanden ist |
| [az storage account keys list](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | Listet Schlüssel für die Speicherkonten auf |
| [az storage blob exists](https://docs.microsoft.com/cli/azure/storage/blob#exists) | Überprüft, ob das Blob vorhanden ist |
| [az storage container create](https://docs.microsoft.com/cli/azure/storage/container#create) | Erstellt einen Container in einem Speicherkonto |
| [az storage blob upload](https://docs.microsoft.com/cli/azure/storage/blob#upload) | Erstellt ein Blob im Container durch Hochladen der VHD |
| [az vm list](https://docs.microsoft.com/cli/azure/vm#list) | Wird zusammen mit `--query` verwendet, um zu überprüfen, ob der Name der VM bereits verwendet wird | 
| [az vm create](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | Erstellt die virtuellen Computer |
| [az vm access set-linux-user](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | Setzt den SSH-Schlüssel zurück, um dem aktuellen Benutzer Zugriff auf die VM zu gewähren |
| [az vm list-ip-addresses](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | Ruft die IP-Adresse des virtuellen Computers ab, der erstellt wurde |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

