---
title: "Azure CLI-Skriptbeispiel – Erstellen eines Docker-Hosts | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen eines Docker-Hosts"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/15/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: c1e0be0e305ba4d53d8e26c55a92b63e1291171d
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---

# <a name="create-a-vm-with-docker"></a>Erstellen eines virtuellen Computers mit Docker

Dieses Skript erstellt den virtuellen Computer mit aktiviertem Docker und startet einen Docker-Container, in dem NGINX ausgeführt wird. Nach dem Ausführen des Skripts können Sie über den FQDN des virtuellen Azure-Computers auf den NGINX-Webserver zugreifen. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker-Host")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, den virtuellen Computer und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle zum Erstellen der Bereitstellung. Jedes Element in der Tabelle ist mit der befehlsspezifischen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der Netzwerksicherheitsgruppe. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest.  |
| [az vm open-port](https://docs.microsoft.com/cli/azure/vm#az_vm_open_port) | Erstellt eine Netzwerksicherheitsgruppenregel zum Zulassen von eingehendem Datenverkehr. In diesem Beispiel wird Port 80 für HTTP-Datenverkehr geöffnet. |
| [azure vm extension set](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Fügt eine VM-Erweiterung zu einem virtuellen Computer hinzu und führt diese aus. In diesem Beispiel wird die Docker VM-Erweiterung verwendet, um einen Docker-Host zu konfigurieren.|
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az_vm_extension_set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

