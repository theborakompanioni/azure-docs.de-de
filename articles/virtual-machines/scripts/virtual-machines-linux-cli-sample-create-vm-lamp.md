---
title: 'Azure CLI-Skriptbeispiel: Bereitstellen des LAMP-Stapels in einer VM-Skalierungsgruppe mit Lastenausgleich | Microsoft-Dokumentation'
description: Verwenden Sie eine benutzerdefinierte Skripterweiterung zum Bereitstellen des LAMP-Stapels in einer VM-Skalierungsgruppe mit Lastenausgleich in Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2017
ms.author: allclark
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: 23170923d7c05c9b7230cf331725250b2a3c0f09
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---

# <a name="deploy-the-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>Bereitstellen des LAMP-Stapels in einer VM-Skalierungsgruppe mit Lastenausgleich

Mit diesem Beispiel wird eine VM-Skalierungsgruppe erstellt und eine Erweiterung angewendet, mit der auf jedem virtuellen Computer in der Skalierungsgruppe ein benutzerdefiniertes Skript zum Bereitstellen des LAMP-Stapels ausgeführt wird.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Erstellen der VM-Skalierungsgruppe mit LAMP-Stapel")]

## <a name="connect"></a>Verbinden

Verwenden Sie folgenden Code zum Herstellen einer Verbindung mit den virtuellen Computern und der Skalierungsgruppe.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Zugriff auf die VM-Skalierungsgruppe")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, die Skalierungsgruppe und die virtuellen Computer sowie alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer, eine Verfügbarkeitsgruppe, einen Load Balancer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az_vmss_create) | Erstellt eine VM-Skalierungsgruppe. |
| [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | Fügt einen Endpunkt mit Lastenausgleich hinzu. |
| [az vmss extension set](https://docs.microsoft.com/cli/azure/vmss/extension#az_vmss_extension_set) | Erstellt die Erweiterung, mit der das benutzerdefinierte Skript für die Bereitstellung auf einem virtuellen Computer ausgeführt wird. |
| [az vmss update-instances](https://docs.microsoft.com/cli/azure/vmss#az_vmss_update_instances) | Führt das benutzerdefinierte Skript auf den VM-Instanzen aus, die bereitgestellt wurden, bevor die Erweiterung auf die Skalierungsgruppe angewendet wurde. |
| [az vmss scale](https://docs.microsoft.com/cli/azure/vmss#az_vmss_scale) | Skaliert die Skalierungsgruppe durch Hinzufügen weiterer VM-Instanzen zentral hoch. Das benutzerdefinierte Skript wird auf diesen Instanzen ausgeführt, wenn sie bereitgestellt werden. |
| [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip#az_network_public_ip_list) | Ruft die IP-Adressen der Computer ab, die in diesem Beispiel erstellt wurden. |
| [az network lb show](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_show) | Ruft die vom Load Balancer verwendeten Front-End- und Back-End-Ports ab. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

