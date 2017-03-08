---
title: "Azure CLI-Skriptbeispiel – Erstellen einer Linux-VM mit OMS-Überwachung | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen einer Linux-VM mit OMS-Überwachung"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 07d91dfa905d22334bd051f9d5f3d936d38efc88
ms.openlocfilehash: 6eae6db78e85bd4d02678c0875e9bf9163af5276
ms.lasthandoff: 02/28/2017

---

# <a name="monitor-a-vm-with-operations-management-suite"></a>Überwachen einer VM mit der Operations Management Suite

Dieses Skript erstellt eine Azure-VM, installiert den OMS-Agent (Operations Management Suite) und registriert das System bei einem OMS-Arbeitsbereich. Nach Ausführung des Skripts wird der virtuelle Computer in der OMS-Konsole angezeigt.

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über den Befehl `az login` eine Verbindung mit Azure hergestellt wurde. Auch die OMS-Arbeitsbereichs-ID und der Arbeitsbereichsschlüssel im Skript müssen aktualisiert werden.

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../virtual-machines-windows-cli-options.md).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-vm-monitor-oms.sh "Schnelles Erstellen einer VM")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die VM und alle zugehörigen Ressourcen entfernt werden.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, einen virtuellen Computer und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az vm create](https://docs.microsoft.com/cli/azure/vm#create) | Erstellt den virtuellen Computer und verbindet diesen mit der Netzwerkkarte, dem virtuellen Netzwerk, dem Subnetz und der NSG. Dieser Befehl legt außerdem das zu verwendende VM-Image und die Administratoranmeldeinformationen fest.  |
| [azure vm extension set](https://docs.microsoft.com/cli/azure/vm/extension#set) | Führt einen VM-Erweiterungs-Agent für einen virtuellen Computer aus. In diesem Fall wird die Operations Management Suite-Agent-Erweiterung verwendet, um den OMS-Agent zu installieren und die VM in einem OMS-Arbeitsbereich zu registrieren. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche VM-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Linux-VMs in Azure](../virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

