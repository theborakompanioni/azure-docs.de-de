---
title: "Azure CLI-Skriptbeispiel – Verschlüsseln einer Linux-VM | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Verschlüsseln einer Linux-VM"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: 7948c99b7b60d77a927743c7869d74147634ddbf
ms.openlocfilehash: 9388bce04e37d049301521f808cd8494c327e335
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="encrypt-a-linux-virtual-machine-in-azure"></a>Verschlüsseln eines virtuellen Linux-Computers in Azure

Dieses Skript erstellt eine sichere Azure Key Vault-Lösung, Verschlüsselungsschlüssel, ein Azure Active Directory-Dienstprinzipal und einen virtuellen Linux-Computer (VM). Die VM wird anschließend mithilfe des Verschlüsselungsschlüssels aus Key Vault und Dienstprinzipal-Anmeldeinformationen verschlüsselt.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[Main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_vm.sh "Verschlüsseln von VM-Datenträgern")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, Azure Key Vault, ein Dienstprinzipal, einen virtuellen Computer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az keyvault create](https://docs.microsoft.com/cli/azure/keyvault#create) | Erstellt Azure Key Vault, um sichere Daten wie Verschlüsselungsschlüssel zu speichern. |
| [az keyvault key create](https://docs.microsoft.com/cli/azure/keyvault/key#create) | Erstellt einen Verschlüsselungsschlüssel in Key Vault. |
| [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | Erstellt ein Azure Active Directory-Dienstprinzipal für die sichere Authentifizierung und Steuerung des Zugriffs auf die Verschlüsselungsschlüssel. |
| [az keyvault set-policy](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | Legt Berechtigungen für Key Vault fest, um dem Dienstprinzipal Zugriff auf Verschlüsselungsschlüssel zu gewähren. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#create) | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der NSG. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest.  |
| [az vm encryption enable](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | Aktiviert die Verschlüsselung mithilfe der Dienstprinzipal-Anmeldeinformationen und Verschlüsselungsschlüssel auf einer VM. |
| [az vm encryption show](https://docs.microsoft.com/cli/azure/vm/encryption#show) | Zeigt den Status des Verschlüsselungsprozesses der VM an. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

